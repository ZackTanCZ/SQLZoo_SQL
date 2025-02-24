# [SQL Medium Questions](https://sqlzoo.net/wiki/Guest_House_Assessment_Medium)
## Question 1
Show the total amount payable by guest Ruth Cadbury for her room bookings. 
<details>
  <summary>SQL Query</summary>

```
SELECT 
SUM(bk.nights * r.amount)
FROM booking as bk
JOIN guest as g
ON (bk.guest_id = g.id)
JOIN rate as r
ON (bk.room_type_requested = r.room_type AND bk.occupants = r.occupancy)
WHERE g.first_name = 'Ruth' and g.last_name = 'Cadbury' 

```

</details>

<details>
  <summary>Expected SQL Output</summary>

![image](https://github.com/user-attachments/assets/cc3421f5-fa60-4e8c-b2f7-91bd35c8725c)

</details>

)

## Question 2
Calculate the total bill for booking 5346 including extras.
<details>
  <summary>SQL Query</summary>

```
SELECT 
bk.booking_id,
(sum(e.amount) + r.amount) as 'Total'
FROM booking as bk
JOIN rate as r
ON (bk.room_type_requested = r.room_type AND bk.occupants = r.occupancy)
JOIN extra as e
ON (bk.booking_id = e.booking_id)
WHERE bk.booking_id = 5346
GROUP BY bk.booking_id
```

</details>

<details>
  <summary>Expected SQL Output</summary>

![image](https://github.com/user-attachments/assets/ffc31a0a-dc9e-4e11-854a-42f4bdb62208)

</details>

)

## Question 3
Edinburgh Residents\
For every guest who has the word “Edinburgh” in their address show the total number of nights booked.\
Be sure to include 0 for those guests who have never had a booking.\
Show last name, first name, address and number of nights. Order by last name then first name.

<details>
  <summary>SQL Query</summary>

```
SELECT 
g.last_name,
g.first_name,
g.address,
COALESCE(SUM(bk.nights),0)
FROM guest as g
LEFT JOIN booking as bk 
ON (bk.guest_id = g.id)
WHERE g.address LIKE '%Edinburgh%'
GROUP BY g.last_name, g.first_name
ORDER BY g.last_name
```

</details>

<details>
  <summary>Expected SQL Output</summary>

![image](https://github.com/user-attachments/assets/d6c8449b-d452-4139-b4f8-dff35a6a21c0)

</details>

)

## Question 4
How busy are we? For each day of the week beginning 2016-11-25 show the number of bookings starting that day.\
Be sure to show all the days of the week in the correct order. 

<details>
  <summary>SQL Query</summary>

```
SELECT 
bk.booking_date,
COUNT(*)
FROM booking as bk
WHERE bk.booking_date BETWEEN '2016-11-25' AND ('2016-11-25' + INTERVAL 6 DAY) 
GROUP BY bk.booking_date
ORDER BY bk.booking_date ASC
```

</details>

<details>
  <summary>Expected SQL Output</summary>

![image](https://github.com/user-attachments/assets/8d9a7b92-4aca-4efa-b855-62414e7f6cd3)

</details>

)

## Question 5
How many guests? Show the number of guests in the hotel on the night of 2016-11-21. Include all occupants who checked in that day but not those who checked out. 

<details>
  <summary>SQL Query</summary>

```
SELECT 
sum(occupants)
FROM booking as bk
WHERE bk.booking_date <= '2016-11-21' AND 
(bk.booking_date + INTERVAL bk.nights DAY) > '2016-11-21'
ORDER BY (bk.booking_date + INTERVAL bk.nights DAY) DESC  
```

</details>

<details>
  <summary>Expected SQL Output</summary>

![image](https://github.com/user-attachments/assets/6285949f-754d-46ae-829a-102bc7298817)

</details>
