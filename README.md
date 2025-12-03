package com.tcs.bancs.microservices.configuration;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import org.springframework.stereotype.Component;
import java.io.IOException;
import java.time.Instant;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

@Component
public class RateLimitFilter implements Filter {

    private final Map<String, RequestCounter> requestMap = new ConcurrentHashMap<>();

    private static final int MAX_REQUESTS = 5;        // allowed calls
    private static final long WINDOW_MS = 60_000;     // 1 minute window

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {

        HttpServletRequest req = (HttpServletRequest) request;

        String key = req.getRemoteAddr();  // Limit by IP

        RequestCounter counter = requestMap.computeIfAbsent(key, k -> new RequestCounter());

        synchronized (counter) {
            long now = Instant.now().toEpochMilli();

            // Reset window if time expired
            if (now - counter.windowStart > WINDOW_MS) {
                counter.windowStart = now;
                counter.count = 0;
            }

            counter.count++;

            if (counter.count > MAX_REQUESTS) {
                response.getWriter().write("Rate limit exceeded. Try again later.");
                response.getWriter().flush();
                return;
            }
        }

        chain.doFilter(request, response);
    }

    static class RequestCounter {
        int count = 0;
        long windowStart = Instant.now().toEpochMilli();
    }
}
