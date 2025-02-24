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

</details>

)

## Question 2

<details>
  <summary>SQL Query</summary>

```

```

</details>

<details>
  <summary>Expected SQL Output</summary>

</details>

)

## Question 3

<details>
  <summary>SQL Query</summary>

```

```

</details>

<details>
  <summary>Expected SQL Output</summary>

</details>

)

## Question 4

<details>
  <summary>SQL Query</summary>

```

```

</details>

<details>
  <summary>Expected SQL Output</summary>

</details>

)

## Question 5

<details>
  <summary>SQL Query</summary>

```
  
```

</details>

<details>
  <summary>Expected SQL Output</summary>

</details>
