# mysql_sys.x-statements_with_full_table_scans-

SELECT 
    `performance_schema`.`events_statements_summary_by_digest`.`DIGEST_TEXT` AS `query`,
    `performance_schema`.`events_statements_summary_by_digest`.`SCHEMA_NAME` AS `db`,
    `performance_schema`.`events_statements_summary_by_digest`.`COUNT_STAR` AS `exec_count`,
    `performance_schema`.`events_statements_summary_by_digest`.`SUM_TIMER_WAIT` AS `total_latency`,
    `performance_schema`.`events_statements_summary_by_digest`.`SUM_NO_INDEX_USED` AS `no_index_used_count`,
    `performance_schema`.`events_statements_summary_by_digest`.`SUM_NO_GOOD_INDEX_USED` AS `no_good_index_used_count`,
    ROUND((IFNULL((`performance_schema`.`events_statements_summary_by_digest`.`SUM_NO_INDEX_USED` / NULLIF(`performance_schema`.`events_statements_summary_by_digest`.`COUNT_STAR`,
                            0)),
                    0) * 100),
            0) AS `no_index_used_pct`,
    `performance_schema`.`events_statements_summary_by_digest`.`SUM_ROWS_SENT` AS `rows_sent`,
    `performance_schema`.`events_statements_summary_by_digest`.`SUM_ROWS_EXAMINED` AS `rows_examined`,
    ROUND((`performance_schema`.`events_statements_summary_by_digest`.`SUM_ROWS_SENT` / `performance_schema`.`events_statements_summary_by_digest`.`COUNT_STAR`),
            0) AS `rows_sent_avg`,
    ROUND((`performance_schema`.`events_statements_summary_by_digest`.`SUM_ROWS_EXAMINED` / `performance_schema`.`events_statements_summary_by_digest`.`COUNT_STAR`),
            0) AS `rows_examined_avg`,
    `performance_schema`.`events_statements_summary_by_digest`.`FIRST_SEEN` AS `first_seen`,
    `performance_schema`.`events_statements_summary_by_digest`.`LAST_SEEN` AS `last_seen`,
    `performance_schema`.`events_statements_summary_by_digest`.`DIGEST` AS `digest`
FROM
    `performance_schema`.`events_statements_summary_by_digest`
WHERE
    (((`performance_schema`.`events_statements_summary_by_digest`.`SUM_NO_INDEX_USED` > 0)
        OR (`performance_schema`.`events_statements_summary_by_digest`.`SUM_NO_GOOD_INDEX_USED` > 0))
        AND (NOT ((`performance_schema`.`events_statements_summary_by_digest`.`DIGEST_TEXT` LIKE 'SHOW%'))))
ORDER BY ROUND((IFNULL((`performance_schema`.`events_statements_summary_by_digest`.`SUM_NO_INDEX_USED` / NULLIF(`performance_schema`.`events_statements_summary_by_digest`.`COUNT_STAR`,
                        0)),
                0) * 100),
        0) DESC , `performance_schema`.`events_statements_summary_by_digest`.`SUM_TIMER_WAIT` DESC
