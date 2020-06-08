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


```SQL
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
### [Placements](https://www.hackerrank.com/challenges/placements/problem) challenge from Hacker Rank

**Methods: Inner Join and Subqueries**

**Task:** Write a query to output the names of those students whose best friends got offered a higher salary than them. Names must be ordered by the salary amount offered to the best friends. It is guaranteed that no two students got same salary offer.

**Example Tables:**

<div>
<img align="left" src="https://s3.amazonaws.com/hr-challenge-images/12895/1443820079-9bd1e231b1-2_1.png" height="200" >
<img align="left" src="https://i.gyazo.com/ab95d2dcd49f5ed96f3c33389a3f8e46.png" height="200" >
<img align="center" src="https://i.gyazo.com/6e35e2eaadd60f307c521bbf03b38b97.png" height="200" >
</div>


```SQL
SELECT name
FROM (SELECT s.id AS student_id, s.name, salary
      FROM Students s INNER JOIN packages p ON s.id = p.id) AS g
      INNER JOIN
     (SELECT f.id AS student_id, p.id AS friend_id, salary
      FROM friends f INNER JOIN packages p ON f.friend_id = p.id) AS h
      ON g.student_id = h.student_id
WHERE h.salary > g.salary
ORDER BY h.salary
```
