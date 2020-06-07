# MySQL Scripts

### [Top Competitors](https://www.hackerrank.com/challenges/full-score/problem) challenge from Hacker Rank

**Methods: Inner Join and Subqueries**

**Task:** Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.

**Tables:** 
*Hackers*, *Difficulty*, *Challenges* and *Submissions*

<div>
<img align="left" src="https://s3.amazonaws.com/hr-challenge-images/19504/1458526776-67667350b4-ScreenShot2016-03-21at7.45.59AM.png" >

<img align="left" src="https://s3.amazonaws.com/hr-challenge-images/19504/1458526915-57eb75d9a2-ScreenShot2016-03-21at7.46.09AM.png" >

<img align="left" src="https://s3.amazonaws.com/hr-challenge-images/19504/1458527032-f9ca650442-ScreenShot2016-03-21at7.46.17AM.png" >

<img align="center" src="https://s3.amazonaws.com/hr-challenge-images/19504/1458527077-298f8e922a-ScreenShot2016-03-21at7.46.29AM.png" >
</div>


``` MySQL
SELECT h.hacker_id, h.name
FROM Hackers h
    INNER JOIN
        (SELECT s.hacker_id AS hack_id, COUNT(s.hacker_id) AS counter
         FROM Submissions s 
             INNER JOIN (SELECT c.challenge_id AS id, c.difficulty_level, d.score AS max_score 
                        FROM Challenges c INNER JOIN Difficulty d 
                        ON c.difficulty_level = d.difficulty_level) AS p
             ON s.challenge_id = p.id
         WHERE s.score = p.max_score
         GROUP BY 1) AS t
    ON h.hacker_id = t.hack_id
WHERE t.counter > 1
ORDER BY t.counter DESC, h.hacker_id ASC
```
