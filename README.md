package com.example.openbanking.ratelimit;

import org.springframework.stereotype.Service;

import java.util.ArrayDeque;
import java.util.Deque;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

/**
 * Sliding-window rate limiter (no external libs).
 * - Uses a Deque<Long> of timestamps per key.
 * - Thread-safe via per-key synchronization on the Deque.
 */
@Service
public class SimpleRateLimiter {

    // key -> deque of request timestamps (millis)
    private final Map<String, Deque<Long>> requests = new ConcurrentHashMap<>();

    // Configurable limits
    private final int MAX_REQUESTS;
    private final long WINDOW_MILLIS;

    public SimpleRateLimiter() {
        this.MAX_REQUESTS = 5;            // max requests
        this.WINDOW_MILLIS = 60_000L;     // window length (1 minute)
    }

    // Optional constructor to programmatically set limits
    public SimpleRateLimiter(int maxRequests, long windowMillis) {
        this.MAX_REQUESTS = maxRequests;
        this.WINDOW_MILLIS = windowMillis;
    }

    /**
     * Check if a request is allowed for given key (IP or user id).
     * @param key identifying client (e.g. IP or API key)
     * @return true if allowed, false if rate limit exceeded
     */
    public boolean isAllowed(String key) {
        long now = System.currentTimeMillis();

        // get or create deque
        Deque<Long> deque = requests.computeIfAbsent(key, k -> new ArrayDeque<>());

        synchronized (deque) {
            // remove timestamps older than window
            while (!deque.isEmpty() && (now - deque.peekFirst()) > WINDOW_MILLIS) {
                deque.removeFirst();
            }

            if (deque.size() >= MAX_REQUESTS) {
                // rate limit exceeded
                return false;
            } else {
                // record current timestamp
                deque.addLast(now);
                return true;
            }
        }
    }

    /**
     * Utility: how many requests remain in current window for this key.
     */
    public int remaining(String key) {
        long now = System.currentTimeMillis();
        Deque<Long> deque = requests.get(key);
        if (deque == null) return MAX_REQUESTS;
        synchronized (deque) {
            while (!deque.isEmpty() && (now - deque.peekFirst()) > WINDOW_MILLIS) {
                deque.removeFirst();
            }
            return Math.max(0, MAX_REQUESTS - deque.size());
        }
    }

    public int getMaxRequests() { return MAX_REQUESTS; }
    public long getWindowMillis() { return WINDOW_MILLIS; }
}
