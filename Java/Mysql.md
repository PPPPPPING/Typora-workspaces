查询两门及其以上不及格的课程的同学的学号  姓名及其平均值

```sql
SELECT t1.sid AS学号,sname AS姓名,score AS平均分FROM
(SELECT sid,AVG(score) AS score from SC GROUP BY sid HAVING AVG(score) <60) t1
LEFT JOIN Student ON t1.sid = Student.sid
```
