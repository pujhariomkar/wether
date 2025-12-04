package com.tcs.bancs.microservices.configuration;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

import com.google.gson.Gson;
import com.tcs.bancs.microservices.RESBEAN;

import java.io.IOException;
import java.time.Instant;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.ConcurrentHashMap;

@Order(1)
@Component
public class RateLimitFilter implements Filter {
	
//	// take value from application.properties file 
//	@Value("${MAX_REQUEST}")
//	private int MAX_REQUEST;
//	
//	@Value("${WINDOW_MS}")
//	private long WINDOW_MS;
	
	private static final int MAX_REQUESTS = 5;        // allowed calls
    private static final long WINDOW_MS = 60_000; 
    private final Map<String, RequestCounter> requestMap = new ConcurrentHashMap<>();



    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
    	
    	
    	Gson gson = new Gson();
		RESBEAN resp = new RESBEAN();

        HttpServletRequest req = (HttpServletRequest) request;
        HttpServletRequest res=(HttpServletRequest) response;
        
//        String key = req.getRemoteAddr();  // Limit by IP
        String uri = req.getRequestURI();
        
        final Set<String> Ret_Lim=Set.of("/headerFileStatus/CheckFileStatus-Debit","/EncFile/CheckFiles-Debit");
                
        if(!Ret_Lim.contains(uri)) {
        	chain.doFilter(request, response);
        	return;
        }
        
        RequestCounter counter = requestMap.computeIfAbsent(uri, k -> new RequestCounter());

        synchronized (counter) {
            long now = Instant.now().toEpochMilli();

            // Reset window if time expired
            if (now - counter.windowStart > WINDOW_MS) {
                counter.windowStart = now;
                counter.count = 0;
            }

            counter.count++;

            if (counter.count > MAX_REQUESTS) {
            	String json= "{\"RESPONSE_STATUS\": \"1\"}";
            	 
            	
            	
            	
                response.getWriter().write("Rate limit exceeded. Try again later.");
                System.out.println("Rate limit exceeded. Try again later.");
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
