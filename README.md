package com.example.openbanking.filter;

import com.example.openbanking.ratelimit.SimpleRateLimiter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.MediaType;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * RateLimit filter that only protects the target endpoint.
 */
@Component
public class RateLimitFilter extends OncePerRequestFilter {

    @Autowired
    private SimpleRateLimiter rateLimiter;

    // Endpoint to protect
    private static final String PROTECTED_PATH = "/headerFilestatus/checkFileStatus-Dabit";

    @Override
    protected boolean shouldNotFilter(HttpServletRequest request) throws ServletException {
        // Apply only to POST on the specific path
        String path = request.getServletPath();
        String method = request.getMethod();
        return !(PROTECTED_PATH.equals(path) && "POST".equalsIgnoreCase(method));
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) throws ServletException, IOException {

        // Choose key: by IP here. To use API key / username, read header or principal.
        String clientIp = extractClientIp(request);

        if (!rateLimiter.isAllowed(clientIp)) {
            // Rate limit exceeded
            int retryAfterSeconds = (int) (rateLimiter.getWindowMillis() / 1000);
            response.setStatus(429); // Too Many Requests
            response.setHeader("Retry-After", String.valueOf(retryAfterSeconds));
            response.setContentType(MediaType.APPLICATION_JSON_VALUE);
            String message = String.format("{\"error\":\"Too Many Requests\",\"message\":\"Rate limit exceeded. Try again later.\"}");
            response.getWriter().write(message);
            return;
        }

        // proceed normally
        filterChain.doFilter(request, response);
    }

    /**
     * Extract client IP. If behind proxy, consider X-Forwarded-For header.
     */
    private String extractClientIp(HttpServletRequest request) {
        String xf = request.getHeader("X-Forwarded-For");
        if (xf != null && !xf.isBlank()) {
            // may contain multiple IPs, first is original client
            return xf.split(",")[0].trim();
        }
        return request.getRemoteAddr();
    }
}
