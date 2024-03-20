# SQLZoo-Solved-Problems
This repository contains the solutions to SQLZoo Quiz

Link to the page - https://sqlzoo.net/wiki/Self_join

[Details of the database](https://sqlzoo.net/wiki/Edinburgh_Buses.) 
We have two tables:

    stops(id, name)
    route(num, company, pos, stop)


## 1. How many stops are in the database.
```sql
select count(distinct(stop)) from route
```

## 2. Find the id value for the stop 'Craiglockhart'
```sql
select id from stops
where name= 'Craiglockhart'
```

## 3. Give the id and the name for the stops on the '4' 'LRT' service.
```sql
select distinct id, name from stops as a inner join route as b on a.id=b.stop
where num='4' and company='LRT'
```

## 4. The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these stops have a count of 2. Add a HAVING clause to restrict the output to these two routes.
```sql
SELECT company, num, COUNT(*)
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
having count(*)>1
```

## 5. Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.
```sql
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num) join stops as c on b.stop=c.id
WHERE a.stop=53 and c.name='London Road'
```

## 6. The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'
```sql
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops stopa ON (a.stop=stopa.id)
  JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' and stopb.name= 'London Road'
```

## 7. Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')
```sql
select distinct a.company, a.num from
route as a join route as b on a.company=b.company and a.num=b.num
join stops stopa on a.stop=stopa.id join stops stopb on b.stop=stopb.id
where stopa.name='Haymarket' and stopb.name='Leith'
```

## 8. Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'
```sql
select a.company,a.num from
route a join route b on a.company=b.company and a.num=b.num
join stops stopa on a.stop=stopa.id join stops stopb on b.stop=stopb.id
where stopa.name='Craiglockhart' and stopb.name='Tollcross'
```

## 9. Give a distinct list of the stops which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.
```sql
select distinct stopb.name, a.company,a.num from
route a join route b on a.company=b.company and a.num=b.num
join stops stopa on a.stop=stopa.id join stops stopb on b.stop=stopb.id
where stopa.name='Craiglockhart'
```

## 10. Find the routes involving two buses that can go from Craiglockhart to Lochend.
Show the bus no. and company for the first bus, the name of the stop for the transfer,
and the bus no. and company for the second bus.

Hint
Self-join twice to find buses that visit Craiglockhart and Lochend, then join those on matching stops.
```sql
select x.num,x.company,x.name,y.num,y.company from

(select a.company,a.num,stopb.name as name from
route a join route b on a.company=b.company and a.num=b.num
join stops stopa on a.stop=stopa.id join stops stopb on b.stop=stopb.id
where stopa.name='Craiglockhart')as x join 

(select a.company,a.num,stopa.name as start from
route a join route b on a.company=b.company and a.num=b.num
join stops stopa on a.stop=stopa.id join stops stopb on b.stop=stopb.id
where stopb.name='Lochend') as y on  x.name=y.start
ORDER BY x.num, x.company, x.name, y.num, y.company
```
This one was a tricky one!!











