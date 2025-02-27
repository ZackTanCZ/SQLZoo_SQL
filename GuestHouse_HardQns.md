
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

![image](https://github.com/user-attachments/assets/b71e6e13-cb44-432e-8bc5-3cc2830370bd)


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

![image](https://github.com/user-attachments/assets/d8acdfe3-37e7-4b7e-8215-eebd17d089ce)


</details>

## Question 3 - Free rooms?
List the rooms that are **free** on the day **25th Nov 2016**. 

<details>  
  <summary>SQL Query</summary>

```
WITH NoRoom AS (
SELECT
bk.room_no as 'room',
bk.booking_id,
DATE_FORMAT(bk.booking_date,"%d/%m/%Y") as 'ChkIn',
bk.nights,
DATE_FORMAT((bk.booking_date + INTERVAL bk.nights DAY),"%d/%m/%Y") as 'ChkOut'
FROM booking as bk
WHERE ((bk.booking_date + INTERVAL bk.nights DAY) > '2016-11-25' AND
bk.booking_date <= '2016-11-25')
ORDER BY room ASC
)

SELECT
DISTINCT(bk.room_no) as 'room'
FROM booking  as bk
WHERE bk.room_no NOT IN  (SELECT room FROM NoRoom)
```

</details>

<details>  
  <summary>Expected SQL Output</summary>

![image](https://github.com/user-attachments/assets/0a076a93-3c8c-42e1-9cc3-a1c591ff1ca8)

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
  <summary>SQL Query [WIP]</summary>

```
WITH ExtraChargesTbl AS (
SELECT 
bk.room_no as 'rmno',
(bk.booking_date + 
INTERVAL bk.nights DAY) as 'ChkOut',
SUM(e.amount) as 'ExtraCharges'
FROM booking as bk
JOIN extra as e
ON (bk.booking_id = e.booking_id)
WHERE (bk.booking_date + 
INTERVAL bk.nights DAY) BETWEEN '2016-11-01' AND ('2016-11-01' + INTERVAL 2 MONTH)
GROUP BY bk.room_no, (bk.booking_date + INTERVAL bk.nights DAY)  
ORDER BY (bk.booking_date + INTERVAL bk.nights DAY) DESC
),
RoomChargesTbl AS (
SELECT 
bk.room_no as 'rmno',
bk.nights as 'nights',
bk.booking_date as 'ChkIn',
(bk.booking_date + 
INTERVAL bk.nights DAY) as 'ChkOut',
r.amount as 'RoomCharge'
FROM booking as bk
JOIN rate as r
ON (bk.room_type_requested = r.room_type AND bk.occupants = r.occupancy)
WHERE (bk.booking_date + 
INTERVAL bk.nights DAY) BETWEEN '2016-11-01' AND ('2016-11-01' + INTERVAL 2 MONTH)
ORDER BY (bk.booking_date + INTERVAL bk.nights DAY) DESC
),
RoomBookings AS (
SELECT
rc.rmno as 'Room No.',
rc.Chkin as 'ChkInDate',
rc.ChkOut as 'ChkOutDate', 
rc.nights as 'nights',
rc.RoomCharge as 'Room Charge',
COALESCE(ec.ExtraCharges,0) as 'Extra Charges',
((rc.nights * rc.RoomCharge) + COALESCE(ec.ExtraCharges,0)) as 'TotalCharge'
FROM RoomChargesTbl as rc
LEFT JOIN ExtraChargesTbl as ec
ON (rc.rmno = ec.rmno AND rc.ChkOut = ec.ChkOut)
ORDER BY rc.ChkOut ASC
),
ThursTbl AS (
SELECT 
DISTINCT(DAYOFWEEK(rb.ChkOutDate)) as 'thursday_date',
rb.ChkOutDate as 'ThursOfWeek'
FROM RoomBookings as rb
WHERE DAYOFWEEK(rb.ChkOutDate) = 5
)


SELECT 
tt.ThursOfWeek,
/*rb.ChkOutDate,*/
SUM(rb.TotalCharge)
FROM RoomBookings as rb
LEFT JOIN ThursTbl as tt
ON (rb.ChkOutDate BETWEEN (tt.ThursOfWeek - INTERVAL 6 WEEK) AND tt.ThursOfWeek)
GROUP BY tt.ThursOfWeek
```
This question is pretty hard but this is the approach\
1. Compute a table and find every distinct **THURSDAY** within Nov 2016, Dec 2016 (Table A)\
2. Compute table with the final rooms charges (nights * rates + extra rates) & Checkout dates (Table B)\
3. Join both tables with the business logic - Catergorise each **CheckOutDate** into their week (which begins with the previous friday & present thursday)\
4. Group all the transaction into their week and sum the final room charges.\

The above SQL query is correct (Syntax wise) but the logic is WIP.
</details>

<details>  
  <summary>Expected SQL Output</summary>



</details>
