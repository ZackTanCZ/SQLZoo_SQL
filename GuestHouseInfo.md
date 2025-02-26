# [GuestHouse](https://sqlzoo.net/wiki/Guest_House)

## Table Name (Column 1, Columm 2,...)

>[!NOTE]
>Primary key are **bolded**

booking (**booking_id**, booking_date, room_no, guest_id, occupants, room_type_requested, nights, arrival_time)\
guest (**id**, first_name, last_name, address)\
rate (**room_type, occupancy**, amount)\
room (**id**, room_type, max_occupancy)\
room_type (**id**, description)\
extra (**extra_id**, booking_id, description, amount)

>[!NOTE]
>'booking_date' column is _DATE_ datatype

## 



