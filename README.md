# Music-Analyst-Project
Analyse the Music Concert Data Set By finding the below requirements

select * from album
select * from artist
select * from customer
select * from employee
select * from genre
select * from invoice
select * from playlist
select * from playlist_track
select * from track
select * from media_type
select * from invoice_line

* data set

Q. Who is the senior most employee based on job?

select * from employee

select * from employee where levels = (select MAX(levels) from employee)

Q. Which countries have the most invoice

select * from invoice

select 
billing_country,
count(invoice_id) as Total
from invoice group by billing_country order by Total desc

--To get the top 5 highest invoice generated

select 
top 5
billing_country,
count(invoice_id) as Total
from invoice group by billing_country order by Total desc

Q. What are the top 3 values of total invoice ?

select * from invoice

select top 3 
format(total, 'C0', 'INR-IN') as Amount
from invoice order by total desc

or

select top 3 *
from invoice
order by total desc


Q. Which city has the best customers? We could like to throw a promotional Music Festival in the city we made the most money. 
Write a query that returns one city that has the highest sum of invoice totals.  
Return both the city name & sum of all invoice totals

select * from invoice

select
top 1
billing_city,
sum(total) as Total
from invoice group by billing_city order by Total desc

Q. Who is the best customer? The customer who has spent the most money decleared will be decleared the best customer ?
write a query that returns the person who has spent the most money.

select * from customer
select * from invoice

select 
concat(A.first_name, ' ', A.last_name) as Full_name,
Sum(total) as Total_Spent
from customer as A
left join
invoice as B
on
A.customer_id=B.customer_id
group by concat(A.first_name, ' ', A.last_name) order by Total_Spent desc

--To get the top 5 customer who spent the most


select top 5
concat(A.first_name, ' ', A.last_name) as Full_name,
Sum(total) as Total_Spent
from customer as A
left join
invoice as B
on
A.customer_id=B.customer_id
group by concat(A.first_name, ' ', A.last_name) order by Total_Spent desc

--To get the top 1 customer who spent the most

select top 1
A.customer_id,
concat(A.first_name, ' ', A.last_name) as Full_name,
Sum(total) as Total_Spent
from customer as A
left join
invoice as B
on
A.customer_id=B.customer_id
group by concat(A.first_name, ' ', A.last_name), A.customer_id
 order by Total_Spent desc

Q. Write a query to return the email, first_name, last_name & genre of all rock music listeners. Return your list ordered alphabetically 
by email started with A

select * from customer
select * from invoice
select * from invoice_line
select * from track
select * from genre


select
CONCAT(A.First_name, ' ', A.last_name) as Full_name,
A.email,
E.name
from customer as A
left join
invoice as B
on
A.customer_id=B.customer_id
left join
invoice_line as C
on
B.invoice_id=C.invoice_id
left join
track as D
on
C.track_id=D.track_id
left join
genre as E
on
D.genre_id=E.genre_id
where E.name = 'Rock' group by A.email, 
CONCAT(A.First_name, ' ', A.last_name), E.name

Q.Let invite the artist who have written the most rock music in our dataset. Write a query that return the artist name and total track count
of the top 10 brands

select * from track
select * from artist
select * from album
select * from genre

select top 10
D.artist_id,
D.name,
count(B.track_id) as DUMPs
from genre as A
left join
track as B
on
A.genre_id=B.genre_id
left join
album as C
on
B.album_id=C.album_id
left join
artist as D
on
C.artist_id= D.artist_id
where A.name= 'Rock'
group by D.name , D.artist_id order by DUMPs desc

Q.Return all the track names that have a song length longer then the average song length. Return the name and milliseconds for each track. 
Order by the song length with the longest song listed first.

select * from track

select name,
milliseconds
from track 
where milliseconds > (select avg(milliseconds) from track) 
order by milliseconds desc

Q. Find how much amount spent by each customer on artists. Write a Query to find customer name, artist name and total spent.
select * from customer
select * from invoice
select * from invoice_line
select * from track
select * from album
select * from artist

select 
concat(A.First_name, ' ', A.last_name) as Full_name,
F.name,
sum(total) as Total_Spent
from customer as A
left join
invoice as B
on
A.customer_id=B.customer_id
left join
invoice_line as C
on
B.invoice_id=C.invoice_id
left join
track as D
on
C.track_id=D.track_id
left join
album as E
on
D.album_id=E.album_id
left join
artist as F
on
E.artist_id=F.artist_id
group by concat(A.First_name, ' ', A.last_name), F.name order by Total_Spent desc

Q. Which genre has the highest sales/invoice?

select * from customer
select * from invoice
select * from invoice_line
select * from track
select * from genre

select
E.name,
count(B.invoice_ID) as PA
from customer as A
left join
invoice as B
on
A.customer_id=B.customer_id
left join
invoice_line as C
on
B.invoice_id=C.invoice_id
left join
track as D
on
C.track_id=D.track_id
left join
genre as E
on
D.genre_id=E.genre_id
group by E.name order by PA desc

