# Case Study: 2022 Cricket Data Analysis Using SQL

This article is a Case Study which I did on 2022 ODI Match Results Data taken from ESPNcricinfo website.
Steps Of Analysis:

1. First of all, I took the dataset link from ESPNcricinfo website and did web scraping on it via MS Excel as shown below:

![web scraping ](https://user-images.githubusercontent.com/72240938/212479610-b5509764-f7a6-4382-a77a-3ea3ecfae54c.jpg)


2. Then, I removed one of the unnecessary column Scorecard & did a conditional formatting on Power BI, where I added an additional column i.e. Host Country as shown below:
There, using Google I listed down the Ground names with their country and made a column for it.

![host country power bi](https://user-images.githubusercontent.com/72240938/212479584-b60b0029-d4d5-45d0-983f-de1246190289.jpg)

Finally, after the changes made I downloaded the file as CSV file.

I came up with several questions with respect to the dataset and answered it with the help of SQL queries.

## Tool Used: MYSQL
 
### [Link To Dataset](https://drive.google.com/file/d/1Pxk98M7vlKiB1F4HUSFiMsw_app3m6ly/view?usp=share_link)

## Table Name: 2022_odimatchresults

Q1) Find total number of ODI matches played in the year 2022.

Ans:

Code:
SELECT COUNT(*) as TotalMatchesPlayed from `2022_odimatchresults`;

Output:
![q 1](https://user-images.githubusercontent.com/72240938/212479719-18aca378-ff31-44b2-808b-93d9ccc6ae41.jpg)

Q2) Find % of Matches won by host country among all matches.

Ans:

Code:
SELECT COUNT(*) FROM `2022_odimatchresults` where `Host Country`= Winner;

Output:

![q2](https://user-images.githubusercontent.com/72240938/212479769-dfc52fc0-8d48-459e-9364-ff5f8c509725.jpg)

So, Percentage of matches = 100*(58/161) = 36%

Q3) Filter 2022 ODI matches based on margin of victory by runs and wickets.

Ans:

Code:
-- Filter with respect to runs
SELECT * from `2022_odimatchresults` where Margin Like '%r%';
-- Filter with respect to wickets
SELECT * from `2022_odimatchresults` where Margin Like '%w%';

Q4) Find top 3 highest runs by margin victory.
Ans:
Code:
SELECT Winner, Margin_number from `2022_odimatchresults_runsmargin`
order by Margin_number desc
limit 3;

Output:

![q4](https://user-images.githubusercontent.com/72240938/212479825-c35b49d8-07af-49af-b597-0cdfe33bdecd.jpg)

Q5) Find top 3 countries who won the most ODIs in 2022.

Ans:

Code:
SELECT Winner, count(*) as TotalCount from `2022_odimatchresults`
group by Winner
order by TotalCount desc
limit 3;

Output:
![q5](https://user-images.githubusercontent.com/72240938/212479869-24cd8923-1ed7-40d5-9e7f-03be1f2888cb.jpg)

Q6) Find on which ground the most matches were played.

Ans:

Code:
SELECT Ground, count(*) as TotalMatchesPlayed from `2022_odimatchresults` 
group by Ground
order by TotalMatchesPlayed desc
limit 3;

Output:

![q6](https://user-images.githubusercontent.com/72240938/212479900-07b9602c-850e-4e8d-a4ef-45c1bf8ba8b6.jpg)

Q7)  Find whether India won the most of the matches while chasing or defending.
Ans:
Code:
SELECT count(*) from `2022_odimatchresults_runsmargin` where Winner = "India";
SELECT count(*) from `2022_odimatchresults_wicketsmargin` where Winner = "India";

Output:
Among the total no. of matches played by India in 2022, more no. of matches have been won by wickets margin.

Now, some of the complex questions with respect to the dataset which requires advanced SQL functions like CTE, UNION.

Q8) Find the country which has played the most ODI matches.

Ans:

Code:

with cte as(
SELECT `Team 1` from `2022_odimatchresults`
UNION ALL
SELECT `Team 2` from `2022_odimatchresults`
)
SELECT `Team 1`, count(*) as Total from cte
group by 1
order by Total desc
limit 1;

Output:

![q8_new](https://user-images.githubusercontent.com/72240938/212479957-d9d51dc3-d9b3-4fa6-b62c-23b22bffbf07.jpg)

Q9) Find the country which has played the most ODI matches at home.

Ans:

Code:
with cte as(
SELECT `Team 1` from `2022_odimatchresults`
where `Team 1`= `Host Country`
UNION ALL
SELECT `Team 2` from `2022_odimatchresults`
where `Team 2`= `Host Country`
)
SELECT `Team 1`, count(*) as TotalMatches from cte
group by 1
order by TotalMatches desc
limit 1;

Output:

![q9_new](https://user-images.githubusercontent.com/72240938/212479995-6338686d-865b-42d0-a7a2-652c7e3ef0b7.jpg)

Q10) Find the country which played the most ODI matches outside home conditions.

Ans:

Code:

with cte as(
SELECT `Team 1` from `2022_odimatchresults`
where `Team 1`<> `Host Country`
UNION ALL
SELECT `Team 2` from `2022_odimatchresults`
where `Team 2`<> `Host Country`
)
SELECT `Team 1`, count(*) as Total from cte
group by 1
order by Total desc
limit 1;

Output:
![q10](https://user-images.githubusercontent.com/72240938/212480029-6533e4c6-279e-4c54-8b50-60434bed6bd1.jpg)



