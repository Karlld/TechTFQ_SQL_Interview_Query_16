```sql

WITH monthly_cases AS (SELECT EXTRACT(month FROM dates) AS case_month,
                              SUM(cases_reported) AS cases
	                       FROM covid_cases
	                       GROUP BY case_month
	                       ORDER BY case_month ASC
					   ),
					   
sum_cases AS (SELECT case_month,
	                 SUM(cases) OVER (ORDER BY case_month) AS total_cases
			     FROM monthly_cases 
			  )
	
	SELECT case_month,
	      total_cases,
		  ROUND((100*(total_cases-(LAG(total_cases, 1, null ) OVER (ORDER BY case_month)))
						  /(LAG(total_cases, 1, null ) OVER (ORDER BY case_month))),2) AS percent_change
			FROM sum_cases; 
```

| case_month | total_cases | percent_change |
|------------|-------------|----------------|
| 1	| 125262 |	 |
| 2	| 190282 |	51.91 |
| 3	| 473582 |	148.88 |
| 4	| 764877 |	61.51 |
| 5	| 1201277 |	57.05 |
| 6	| 1323471 |	10.17 |
| 7	| 1399515 |	5.75 |
| 8	| 1454506 |	3.93 |
| 9	| 1569611 |	7.91 |
| 10	 | 1596198 |	1.69 |
| 11 | 	1698210 |	6.39 |
| 12 |	1824381 |	7.43 |
