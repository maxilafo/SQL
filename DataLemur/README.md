# Solutions to questions in DataLemur
Here is the solution made with PostgreSQL for each of the SQL queries. You can click on each title, and it will take you to the problem posed.

## <p><u><a href="https://datalemur.com/questions/sql-page-with-no-likes">Page With No Likes</a></u></p>
<p>SELECT DISTINCT(page_id) FROM pages</p>
<p>EXCEPT</p>
<p>SELECT DISTINCT(page_id) FROM page_likes</p>
<p>ORDER BY page_id;</p>

## <p><u><a href="https://datalemur.com/questions/tesla-unfinished-parts">Unfinished Parts</a></u></p>
<p>SELECT part, assembly_step FROM parts_assembly</p>
<p>WHERE finish_date IS NULL;</p>

## <p><u><a href="https://datalemur.com/questions/sql-histogram-tweets">Histogram of Tweets</a></u></p>
<p>WITH aux AS (SELECT user_id, COUNT(*) FROM tweets</p>
<p>WHERE tweet_date BETWEEN '2022-01-01' AND '2022-12-31'</p>
<p>GROUP BY user_id)</p>
<p>&nbsp;</p>
<p>SELECT count AS tweet_bucket, COUNT(*) AS users_num FROM aux</p>
<p>GROUP BY tweet_bucket</p>

## <p><u><a href="https://datalemur.com/questions/laptop-mobile-viewership">Laptop vs. Mobile Viewership</a></u></p>
<p>SELECT</p>
<p>SUM(CASE WHEN device_type = 'laptop' THEN 1 ELSE 0 END) AS laptop_views,</p>
<p>SUM(CASE WHEN device_type IN ('phone','tablet') THEN 1 ELSE 0 END) AS mobile_views</p>
<p>FROM viewership</p>

## <p><u><a href="https://datalemur.com/questions/matching-skills">Data Science Skills</a></u></p>
<p>SELECT candidate_id FROM candidates</p>
<p>WHERE skill = 'Python' or skill = 'PostgreSQL' or skill = 'Tableau'</p>
<p>GROUP BY candidate_id</p>
<p>HAVING COUNT(candidate_id) = 3</p>
<p>ORDER BY candidate_id</p>

## <p><u><a href="https://datalemur.com/questions/sql-average-post-hiatus-1">Average Post Hiatus (Part 1)</a></u></p>
<p>SELECT user_id,</p>
<p>&nbsp; DATE_PART('day',MAX(post_date)-MIN(post_date)) AS days_between</p>
<p>FROM posts</p>
<p>WHERE DATE_PART('year', post_date) = 2021</p>
<p>GROUP BY user_id</p>
<p>HAVING COUNT(post_id) &gt;1;</p>

## <p><u><a href="https://datalemur.com/questions/teams-power-users">Teams Power Users</a></u></p>
<p>SELECT sender_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; COUNT(*) AS message_count</p>
<p>FROM messages</p>
<p>WHERE DATE_PART('year', sent_date) = 2022 AND DATE_PART('month', sent_date) = 8</p>
<p>GROUP BY sender_id</p>
<p>ORDER BY message_count DESC</p>
<p>LIMIT 2</p>
<p>;</p>

## <p><u><a href="https://datalemur.com/questions/duplicate-job-listings">Duplicate Job Listings</a></u></p>
<p>SELECT COUNT(*) FROM (SELECT company_id AS duplicate_companies FROM job_listings</p>
<p>GROUP BY company_id</p>
<p>HAVING COUNT(CONCAT(title,description)) &gt; 1) AS subquery</p>
<p>;</p>

## <p><u><a href="https://datalemur.com/questions/completed-trades">Cities With Completed Trades</a></u></p>
<p>SELECT&nbsp; u.city,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; COUNT(DISTINCT(t.order_id)) AS total_orders</p>
<p>FROM trades AS t</p>
<p>INNER JOIN users AS u ON t.user_id = u.user_id</p>
<p>WHERE status = 'Completed'</p>
<p>GROUP BY u.city</p>
<p>ORDER BY total_orders DESC</p>
<p>LIMIT 3</p>

## <p><u><a href="https://datalemur.com/questions/sql-avg-review-ratings">Average Review Ratings</a></u></p>
<p>SELECT&nbsp; DATE_PART('month', submit_date) AS mth,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; product_id AS product,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ROUND(AVG(stars),2) AS avg_stars</p>
<p>FROM reviews</p>
<p>GROUP BY mth, product_id</p>
<p>ORDER BY mth,product</p>

## <p><u><a href="https://datalemur.com/questions/click-through-rate">App Click-through Rate (CTR)</a></u></p>
<p>SELECT&nbsp; app_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ROUND(100.0</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *SUM(CASE WHEN event_type = 'click' THEN 1 ELSE 0 END)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; /SUM(CASE WHEN event_type = 'impression' THEN 1 ELSE 0 END),2) AS ctr</p>
<p>FROM events</p>
<p>WHERE DATE_PART('year',timestamp) = 2022</p>
<p>GROUP BY app_id</p>

## <p><u><a href="https://datalemur.com/questions/second-day-confirmation">Second Day Confirmation</a></u></p>
<p>WITH user1 AS (SELECT user_id, DATE_PART('day', action_date-signup_date) FROM emails AS e</p>
<p>INNER JOIN texts AS t ON e.email_id = t.email_id</p>
<p>WHERE signup_action = 'Confirmed')</p>
<p>SELECT user_id FROM user1 WHERE date_part = 1</p>

## <p><u><a href="https://datalemur.com/questions/cards-issued-difference">Cards Issued Difference</a></u></p>
<p>SELECT card_name, MAX(issued_amount)-MIN(issued_amount) AS diff</p>
<p>FROM monthly_cards_issued</p>
<p>GROUP BY card_name</p>
<p>ORDER BY diff DESC</p>

## <p><u><a href="https://datalemur.com/questions/alibaba-compressed-mean">Compressed Mean</a></u></p>
<p>SELECT ROUND(SUM(item_count::DECIMAL*order_occurrences::DECIMAL)/SUM(order_occurrences)::DECIMAL,1)</p>
<p>FROM items_per_order;</p>

## <p><u><a href="https://datalemur.com/questions/top-profitable-drugs">Pharmacy Analytics (Part 1)</a></u></p>
<p>SELECT drug, SUM(total_sales - cogs) AS total_profit</p>
<p>FROM pharmacy_sales</p>
<p>GROUP BY drug</p>
<p>ORDER BY total_profit DESC</p>
<p>LIMIT 3</p>

## <p><u><a href="https://datalemur.com/questions/non-profitable-drugs">Pharmacy Analytics (Part 2)</a></u></p>
<p>WITH profits AS (SELECT&nbsp; manufacturer,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; drug,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; SUM(total_sales - cogs) AS profit</p>
<p>FROM pharmacy_sales</p>
<p>GROUP BY manufacturer, drug</p>
<p>ORDER BY profit)</p>
<p>&nbsp;</p>
<p>SELECT manufacturer, COUNT(drug), ABS(SUM(profit)) AS losses</p>
<p>FROM profits</p>
<p>WHERE profit &lt; 0</p>
<p>GROUP BY manufacturer</p>
<p>ORDER BY losses DESC</p>
<p><u><a href="https://datalemur.com/questions/total-drugs-sales">Pharmacy Analytics (Part 3)</a></u></p>
<p>SELECT&nbsp; manufacturer,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; CONCAT('$',ROUND(SUM(total_sales) / 1000000),' million') AS sale</p>
<p>FROM pharmacy_sales</p>
<p>GROUP BY manufacturer</p>
<p>ORDER BY SUM(total_sales) DESC, manufacturer</p>

## <p><u><a href="https://datalemur.com/questions/sql-third-transaction">User's Third Transaction</a></u></p>
<p>SELECT user_id, spend, transaction_date FROM</p>
<p>(SELECT user_id, spend, transaction_date, ROW_NUMBER () OVER(</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; PARTITION BY user_id</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ORDER BY transaction_date)</p>
<p>FROM transactions) AS t1</p>
<p>WHERE row_number = 3</p>

## <p><u><a href="https://datalemur.com/questions/time-spent-snaps">Sending vs. Opening Snaps</a></u></p>
<p><br /> </p>
<p>&nbsp;</p>
<p>SELECT&nbsp; b.age_bucket,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ROUND((100.0</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *SUM(CASE WHEN activity_type = 'send' THEN time_spent ELSE 0 END)::DECIMAL</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; /SUM(CASE WHEN activity_type IN ('open','send') THEN time_spent ELSE 0 END)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ),2) AS send_perc,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ROUND((100.0</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *SUM(CASE WHEN activity_type = 'open' THEN time_spent ELSE 0 END)::DECIMAL</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;/SUM(CASE WHEN activity_type IN ('open','send') THEN time_spent ELSE 0 END)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ),2) AS open_perc</p>
<p>FROM activities AS a</p>
<p>INNER JOIN age_breakdown AS b ON a.user_id = b.user_id</p>
<p>GROUP BY b.age_bucket</p>

## <p><u><a href="https://datalemur.com/questions/rolling-average-tweets">Tweets' Rolling Averages</a></u></p>
<p>SELECT&nbsp; user_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; tweet_date,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ROUND(AVG(tweet_count) OVER(PARTITION BY user_id ROWS BETWEEN 2 PRECEDING AND CURRENT ROW),2) AS rolling_avg_3d</p>
<p>FROM tweets</p>
<p>ORDER BY user_id, tweet_date</p>

## <p><u><a href="https://datalemur.com/questions/sql-highest-grossing">Highest-Grossing Items</a></u></p>
<p>WITH ranking AS (</p>
<p>SELECT&nbsp; category,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; product,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; SUM(spend) AS total_spend,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ROW_NUMBER() OVER(PARTITION BY category ORDER BY SUM(spend) DESC) AS rank</p>
<p>FROM product_spend</p>
<p>WHERE DATE_PART('year', transaction_date) = 2022</p>
<p>GROUP BY category, product</p>
<p>ORDER BY category, total_spend DESC</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; )</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</p>
<p>SELECT category, product, total_spend FROM ranking</p>
<p>WHERE rank &lt; 3</p>

## <p><u><a href="https://datalemur.com/questions/top-fans-rank">Top 5 Artists</a></u></p>
<p>WITH ranking AS (</p>
<p>SELECT&nbsp;&nbsp; a.artist_name,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; DENSE_RANK() OVER(ORDER BY COUNT(s.song_id) DESC) AS artist_rank</p>
<p>FROM artists AS a</p>
<p>INNER JOIN songs AS s ON a.artist_id = s.artist_id</p>
<p>INNER JOIN global_song_rank AS g ON s.song_id = g.song_id</p>
<p>WHERE g.rank &lt;= 10</p>
<p>GROUP BY a.artist_name)</p>
<p>&nbsp;</p>
<p>SELECT&nbsp; artist_name,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; artist_rank</p>
<p>FROM ranking</p>
<p>WHERE artist_rank &lt;= 5</p>

## <p><u><a href="https://datalemur.com/questions/signup-confirmation-rate">Signup Activation Rate</a></u></p>
<p>SELECT&nbsp; ROUND(SUM(CASE WHEN signup_action = 'Confirmed' THEN 1 ELSE 0 END)::DECIMAL</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; /COUNT(signup_action)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ,2)</p>
<p>FROM emails AS e</p>
<p>INNER JOIN texts AS t ON e.email_id = t.email_id</p>

## <p><u><a href="https://datalemur.com/questions/supercloud-customer">Supercloud Customer</a></u></p>
<p>&nbsp;</p>
<p>SELECT&nbsp; c.customer_id</p>
<p>FROM customer_contracts as c</p>
<p>INNER JOIN products AS p ON c.product_id = p.product_id</p>
<p>GROUP BY c.customer_id</p>
<p>HAVING COUNT(DISTINCT(p.product_category)) = (SELECT COUNT(DISTINCT(product_category))FROM products)</p>

## <p><u><a href="https://datalemur.com/questions/odd-even-measurements">Odd and Even Measurements</a></u></p>
<p>WITH ranking AS (SELECT&nbsp; measurement_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; measurement_value,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; measurement_time,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ROW_NUMBER() OVER(PARTITION BY(DATE_PART('day',measurement_time)) ORDER BY measurement_time)::INTEGER AS rank</p>
<p>FROM measurements</p>
<p>GROUP BY measurement_id, measurement_value, measurement_time</p>
<p>ORDER BY measurement_time)</p>
<p>&nbsp;</p>
<p>SELECT&nbsp; DATE_TRUNC('day',measurement_time) AS measurement_day,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; SUM(CASE WHEN rank%2 &lt;&gt; 0 THEN measurement_value ELSE 0 END) AS odd_sum,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; SUM(CASE WHEN rank%2 = 0 THEN measurement_value ELSE 0 END) AS even_sum</p>
<p>FROM ranking</p>
<p>GROUP BY measurement_day</p>
<p>ORDER BY measurement_day</p>

## <p><u><a href="https://datalemur.com/questions/histogram-users-purchases">Histogram of Users and Purchases</a></u></p>
<p>WITH ranking AS (SELECT&nbsp; MAX(transaction_date) AS transaction_date,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; user_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY transaction_date DESC) AS rank</p>
<p>FROM user_transactions</p>
<p>GROUP BY transaction_date, user_id</p>
<p>ORDER BY transaction_date)</p>
<p>&nbsp;</p>
<p>SELECT&nbsp; transaction_date,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; user_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; COUNT(DISTINCT(product_id)) AS purchase_count</p>
<p>FROM user_transactions</p>
<p>WHERE CONCAT(transaction_date,user_id) IN (SELECT CONCAT(transaction_date,user_id) FROM ranking WHERE rank = 1)</p>
<p>GROUP BY transaction_date, user_id</p>
<p>ORDER BY transaction_date</p>

## <p><u><a href="https://datalemur.com/questions/alibaba-compressed-mode">Compressed Mode</a></u></p>
<p>SELECT&nbsp; item_count</p>
<p>FROM items_per_order</p>
<p>WHERE order_occurrences = ( SELECT MAX(order_occurrences)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; FROM items_per_order)</p>
<p>ORDER BY item_count</p>

## <p><u><a href="https://datalemur.com/questions/card-launch-success">Card Launch Success</a></u></p>
<p>WITH ranking AS&nbsp; (SELECT&nbsp;&nbsp; card_name,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; issue_month,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; issue_year,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; issued_amount,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; RANK() OVER(PARTITION BY card_name ORDER BY CONCAT(issue_year,issue_month)) AS rank</p>
<p>FROM monthly_cards_issued)</p>
<p>&nbsp;</p>
<p>SELECT&nbsp;&nbsp;&nbsp; card_name,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; issued_amount</p>
<p>FROM ranking</p>
<p>WHERE rank = 1</p>
<p>ORDER BY issued_amount DESC</p>

## <p><u><a href="https://datalemur.com/questions/international-call-percentage">International Call Percentage</a></u></p>
<p>WITH aux AS (</p>
<p>SELECT c.country_id AS caller_country,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; p.receiver_id AS receiver,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; r.country_id AS receiver_country</p>
<p>FROM phone_calls AS p</p>
<p>INNER JOIN phone_info AS c ON p.caller_id = c.caller_id</p>
<p>INNER JOIN phone_info AS r ON p.receiver_id = r.caller_id)</p>
<p>&nbsp;</p>
<p>SELECT&nbsp; ROUND(100.0</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *SUM(CASE WHEN caller_country &lt;&gt; receiver_country THEN 1 ELSE 0 END)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; /COUNT(*)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ,1)</p>
<p>FROM aux</p>

## <p><u><a href="https://datalemur.com/questions/user-retention">Active User Retention</a></u></p>
<p>WITH m6 AS (SELECT *</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; FROM user_actions</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WHERE DATE_PART('month', event_date) IN (6) AND DATE_PART('year', event_date) = 2022),</p>
<p>&nbsp;&nbsp;&nbsp; m7 AS (SELECT&nbsp; *</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; FROM user_actions</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WHERE DATE_PART('month', event_date) IN (7) AND DATE_PART('year', event_date) = 2022)</p>
<p>&nbsp;</p>
<p>SELECT DISTINCT&nbsp;&nbsp; DATE_PART('month', m7.event_date) AS mth,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; COUNT(DISTINCT(m7.user_id)) AS mau</p>
<p>FROM m6</p>
<p>INNER JOIN m7 ON m6.user_id = m7.user_id</p>
<p>GROUP BY mth</p>

## <p><u><a href="https://datalemur.com/questions/yoy-growth-rate">Y-on-Y Growth Rate</a></u></p>
<p>WITH sum AS (SELECT&nbsp;&nbsp; DATE_PART('year', transaction_date) AS year,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; product_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; SUM(spend) AS curr_year_spend</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; FROM user_transactions</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; GROUP BY year, product_id</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ORDER BY year)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</p>
<p>SELECT&nbsp; year,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; product_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; curr_year_spend,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; LAG(curr_year_spend) OVER(PARTITION BY product_id ORDER BY year) AS prev_year_spend,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ROUND(&nbsp; 100.0</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *(curr_year_spend - LAG(curr_year_spend) OVER(PARTITION BY product_id ORDER BY year))</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; /(LAG(curr_year_spend) OVER(PARTITION BY product_id ORDER BY year))</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ,2) AS yoy</p>
<p>FROM sum</p>
<p>ORDER BY product_id, year</p>

## <p><u><a href="https://datalemur.com/questions/prime-warehouse-storage">Maximize Prime Item Inventory</a></u></p>
<p>WITH sum AS (</p>
<p>SELECT</p>
<p>&nbsp; SUM(square_footage) FILTER (WHERE item_type = 'prime_eligible') AS prime_sq_ft,</p>
<p>&nbsp; COUNT(item_id) FILTER (WHERE item_type = 'prime_eligible') AS prime_item_count,</p>
<p>&nbsp; SUM(square_footage) FILTER (WHERE item_type = 'not_prime') AS not_prime_sq_ft,</p>
<p>&nbsp; COUNT(item_id) FILTER (WHERE item_type = 'not_prime') AS not_prime_item_count</p>
<p>FROM inventory</p>
<p>),</p>
<p>prime AS (</p>
<p>SELECT</p>
<p>&nbsp; FLOOR(500000/prime_sq_ft)*prime_sq_ft AS max_prime_area</p>
<p>FROM sum</p>
<p>)</p>
<p>&nbsp;</p>
<p>SELECT</p>
<p>&nbsp; 'prime_eligible' AS item_type,</p>
<p>&nbsp; FLOOR(500000/prime_sq_ft)*prime_item_count AS item_count</p>
<p>FROM sum</p>
<p>&nbsp;</p>
<p>UNION ALL</p>
<p>&nbsp;</p>
<p>SELECT</p>
<p>&nbsp; 'not_prime' AS item_type,</p>
<p>&nbsp; FLOOR((500000-(SELECT max_prime_area FROM prime))</p>
<p>&nbsp;&nbsp;&nbsp; / not_prime_sq_ft) * not_prime_item_count AS item_count</p>
<p>FROM sum;</p>

## <p><u><a href="https://datalemur.com/questions/median-search-freq">Median Google Search Frequency</a></u></p>
<p>WITH serie AS (</p>
<p>SELECT searches</p>
<p>FROM search_frequency</p>
<p>GROUP BY searches,</p>
<p>GENERATE_SERIES(1, num_users))</p>
<p>&nbsp;</p>
<p>SELECT</p>
<p>&nbsp; ROUND(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY searches)::DECIMAL, 1) AS&nbsp; median</p>
<p>FROM serie;</p>

## <p><u><a href="https://datalemur.com/questions/updated-status">Advertiser Status</a></u></p>
<p>SELECT&nbsp; CASE WHEN a.user_id IS NOT NULL THEN a.user_id</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ELSE d.user_id END AS user_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; CASE</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WHEN a.status = 'NEW' AND d.paid IS NOT NULL THEN 'EXISTING'</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WHEN a.status = 'NEW' AND d.paid IS NULL THEN 'CHURN'</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WHEN a.status = 'EXISTING' AND d.paid IS NOT NULL THEN 'EXISTING'</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WHEN a.status = 'EXISTING' AND d.paid IS NULL THEN 'CHURN'</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WHEN a.status = 'CHURN' AND d.paid IS NOT NULL THEN 'RESURRECT'</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WHEN a.status = 'CHURN' AND d.paid IS NULL THEN 'CHURN'</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WHEN a.status = 'RESURRECT' AND d.paid IS NOT NULL THEN 'EXISTING'</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WHEN a.status = 'RESURRECT' AND d.paid IS NULL THEN 'CHURN'</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ELSE 'NEW' END AS new_status</p>
<p>FROM advertiser AS a</p>
<p>FULL JOIN daily_pay AS d ON a.user_id = d.user_id</p>
<p>ORDER BY user_id</p>

## <p><u><a href="https://datalemur.com/questions/pizzas-topping-cost">3-Topping Pizzas</a></u></p>
<p>WITH ingredients AS (SELECT&nbsp; ROW_NUMBER() OVER(ORDER BY topping_name) AS row,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; topping_name,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ingredient_cost</p>
<p>FROM pizza_toppings</p>
<p>ORDER BY topping_name)</p>
<p>&nbsp;</p>
<p>SELECT&nbsp; CONCAT(i1.topping_name,',',i2.topping_name,',',i3.topping_name) AS pizza,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; i1.ingredient_cost+i2.ingredient_cost+i3.ingredient_cost AS total_cost</p>
<p>FROM ingredients AS i1</p>
<p>INNER JOIN ingredients AS i2 ON i1.row &lt; i2.row</p>
<p>INNER JOIN ingredients AS i3 ON i2.row &lt; i3.row</p>
<p>ORDER BY total_cost DESC, i1.row, i2.row, i3.row</p>

## <p><u><a href="https://datalemur.com/questions/repeated-payments">Repeated Payments</a></u></p>
<p>WITH diffs AS (SELECT&nbsp; merchant_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; credit_card_id,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; amount,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; CONCAT(merchant_id,credit_card_id,amount),</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; transaction_timestamp,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; LAG(transaction_timestamp) OVER(PARTITION BY CONCAT(merchant_id,credit_card_id,amount) ORDER BY transaction_timestamp),</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; DATE_PART('hours',transaction_timestamp - LAG(transaction_timestamp) OVER(PARTITION BY CONCAT(merchant_id,credit_card_id,amount) ORDER BY transaction_timestamp))AS hours,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; DATE_PART('minutes',transaction_timestamp - LAG(transaction_timestamp) OVER(PARTITION BY CONCAT(merchant_id,credit_card_id,amount) ORDER BY transaction_timestamp)) AS minutes</p>
<p>&nbsp;</p>
<p>FROM transactions</p>
<p>ORDER BY concat)</p>
<p>&nbsp;</p>
<p>SELECT COUNT(*) AS payment_count</p>
<p>FROM diffs</p>
<p>WHERE hours = 0 AND minutes &lt;= 10</p>
