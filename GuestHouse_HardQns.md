
# [Hard Questions](https://sqlzoo.net/wiki/Guest_House_Assessment_Hard)
## Question 1 - Coincidence
Have two guests with the same surname ever stayed in the hotel on the evening?\
Show the last name and both first names.\
Do not include duplicates. 

<details>  
  <summary>SQL Query</summary>

```
WITH CheckOutTbl AS (
SELECT g.last_name as 'lName', g.first_name as 'fName',  bk.booking_id, bk.booking_date as 'CheckIn', bk.nights,
(bk.booking_date + INTERVAL bk.nights DAY) as 'CheckOut'
FROM booking as bk
JOIN guest as g
ON (g.id = bk.guest_id)
ORDER BY g.last_name
),
JoinedTbl AS (
SELECT tbl1.lName as 'lName', tbl1.fName as 'fName1', tbl1.CheckIn as 'ChkIn1', tbl1.Checkout as 'ChkOut1',
tbl2.fName as 'fName2', tbl2.CheckIn as 'ChkIn2', tbl2.Checkout as 'ChkOut2'
FROM CheckOutTbl as tbl1, CheckOutTbl as tbl2
WHERE (tbl1.fName <> tbl2.fName) AND (tbl1.lName = tbl2.lName)
ORDER BY tbl1.fName DESC, tbl1.lName DESC
),
matchtbl AS (
Select 
jt.lName,
jt.fName1,
DATE_FORMAT(jt.ChkIn1, "%d/%m/%Y") as 'ChkIn1',
DATE_FORMAT(jt.ChkOut1, "%d/%m/%Y") as 'ChkOut1',
jt.fName2,
DATE_FORMAT(jt.ChkIn2, "%d/%m/%Y") as 'ChkIn2',
DATE_FORMAT(jt.ChkOut2, "%d/%m/%Y") as 'ChkOut2',
(IF((jt.ChkIn1 < jt.ChkOut2) AND (jt.ChkOut1 > jt.ChkIn2),1,0)) as 'match'
FROM JoinedTbl as jt
ORDER BY jt.fName1 DESC
)
SELECT
mt.lName as 'Last Name',
mt.fName1 as 'Cust 1 First Name',
mt.fName2 as 'Cust 2 First Name'
FROM matchtbl as mt
WHERE mt.match = 1
GROUP BY mt.lName
ORDER BY mt.lName ASC
```

</details>

<details>  
  <summary>Optimised SQL Query</summary>



</details>


<details>  
  <summary>Expected SQL Output</summary>

![image](https://github.com/user-attachments/assets/75ec1e7e-92b2-4881-a728-6f3c21351578)

</details>

## Question 2 - Check out per floor. 
The first digit of the room number indicates the floor – e.g. room 201 is on the 2nd floor.\
For each day of the week beginning 2016-11-14 show how many rooms are being vacated that day by floor number.\
Show all days in the correct order.

<details>  
  <summary>SQL Query</summary>

```
SELECT
DATE_FORMAT(bk.booking_date,"%Y-%m-%d") as 'BookIn Date',
bk.booking_id,
bk.room_no,
SUM(CASE WHEN bk.room_no < 200 THEN 1 ELSE 0 END) as '1st',
SUM(CASE WHEN bk.room_no BETWEEN 200 AND 299 THEN 1 ELSE 0 END) as '2nd',
SUM(CASE WHEN bk.room_no >= 300 THEN 1 ELSE 0 END) as '3rd'
FROM booking as bk
WHERE bk.booking_date BETWEEN '2016-11-14' 
AND ('2016-11-14' + INTERVAL 6 DAY)
GROUP BY bk.booking_date
ORDER BY bk.booking_date ASC, room_no ASC
```

</details>

<details>  
  <summary>Expected SQL Output</summary>

![image](https://github.com/user-attachments/assets/1fdec9d1-4afa-4b3c-a773-286be6cc0a8e)


</details>

## Question 3 - Free rooms?
List the rooms that are **free** on the day **25th Nov 2016**. 

<details>  
  <summary>SQL Query</summary>



</details>

<details>  
  <summary>SQL Query</summary>



</details>



## Question 4 - Single room for three nights required.
A customer wants a single room for three consecutive nights.\
Find the first available date in December 2016.

<details>  
  <summary>SQL Query</summary>



</details>

<details>  
  <summary>SQL Query</summary>



</details>



## Question 5 - Gross income by week.
Money is collected from guests when they leave.\
For each Thursday in November and December 2016, show the total amount of money collected from the previous Friday to that day, inclusive. 

<details>  
  <summary>SQL Query</summary>



</details>

<details>  
  <summary>SQL Query</summary>



</details>
