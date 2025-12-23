
SELECT s.status,
       COUNT(d.status) AS status_count
FROM (
    SELECT 0 status FROM dual UNION ALL
    SELECT 1 FROM dual UNION ALL
    SELECT 2 FROM dual UNION ALL
    SELECT 3 FROM dual UNION ALL
    SELECT 4 FROM dual UNION ALL
    SELECT 5 FROM dual
) s
LEFT JOIN ADESH_DEC_LOGS d
  ON d.status = s.status
 AND d.file_received_date >= :input_date
 AND d.file_received_date <  :input_date + 1
GROUP BY s.status
ORDER BY s.status;
