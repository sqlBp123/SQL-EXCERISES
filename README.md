130. Historians decided to make a summary of battles and arrange it in two super columns. Each super column consists of three columns containing the battle serial number, name, and date.
First, the left super column is filled out in ascending order of serial numbers, then the right one. Serial numbers are assigned sequentially, with the battles being sorted by date, then by name.
In order to save paper the historians distribute the data from the Battles table equally between the two super columns (adding an extra battle to the left one if the total of battles is odd).
Display the result of the historians’ work as a six-column table, filling empty cells with NULL values.



solution:
with a 
as
(
select Ntile(2) over(order by date,name) ro,row_number()over(order by date ) row, name,date from battles
)

select rn_1,name_1,date_1,rn_2,name_2,date_2 
from (select   
case when ro =1 then a.row  end as rn_1 ,
case when ro =1 then name end as name_1,
case when ro =1 then date end as date_1
from a) as b join
(select  
case when ro =2 then a.row else null end as  rn_2,
case when ro =2 then name end as name_2,
case when ro =2 then date end as date_2 
from a) as c
on b.rn_1 is not null and c.rn_2 is not null and rn_1 + 3 = rn_2
