# intermediate-sql-queries-afternoon

For today we will be practicing inserting and querying data using SQL.

Here is a website that will let us write queries to interact with some data. http://jxs.me/chinook-web/

On the left are the Tables with their fields. The right is where we will be writing our queries. The bottom is where we will see our results.

Any new tables or records that we add into the database will be removed after you refresh the page. So if you're wondering where you data went, that may be why.

Use www.sqlteaching.com or sqlbolt.com as resources for the missing keywords you'll need.

## Practice joins

### Use at least one join for all of the following

__Examples__
```
Select [Column names] from [Table] [abbv] join [Table2] [abbv2] on abbv.prop=abbv2.prop where [Conditions]

Select a.Name, b.Name from someTable a join anotherTable b on a.someid=b.someid
Select a.Name, b.Name from someTable a join anotherTable b on a.someid=b.someid where b.email='e@mail.com'
```

* Get all invoices where the unit price on the invoice line is greater than $0.99

    SELECT *
    FROM invoice as i
    JOIN invoiceline as il
    ON i.invoiceid = il.invoiceid
    where unitprice > 0.99

* Get all invoices and show me their invoice date, customer first and last names, and total

    SELECT invoice date, firstname, lastname,  total
    FROM invoice as i
    JOIN customer as c
    ON i.customerid = c.customerid

* Get all customers and show me their first name, last name, and support rep first name and last name (support reps are on the Employees table)

    SELECT e.firstname, e.lastname, c.firstname, c.lastname
    From customer as c
    JOIN employer as e
    ON c.customerid = e.employeeid

* Get all Albums and show me the album title and the artist name

    SELECT title, name
    FROM album a
    JOIN artist ar
    ON a.artistid=ar.artistid

* Get all Playlist Tracks where the playlist name is Music

    SELECT *
    FROM playlist p
    JOIN playlisttrack pt
    ON p.playlistid = pt.playlistId
    where name = 'Music'

* Get all Tracknames for playlistId 5

    SELECT t.name
    FROM Track t
    JOIN playlisttrack pt
    ON t.trackid = pt.trackid
    JOIN playlist p
    ON p.trackid = pt.trackid
    where p.playlistid = 5

* Now we want all tracknames and the playlist name that they're on (You'll have to use 2 joins)

    SELECT t.name , p.name
    FROM Track t
    JOIN playlisttrack pt
    ON t.trackid = pt.trackid
    JOIN playlist p
    ON p.playlistid = pt.playlistid
    where p.playlistid = 5

* Get all Tracks that are alternative and show me the track name and the album name (2 joins)

    SELECT t.name, g.name, a.albumid
    FROM track t  
    JOIN genre g
    ON t.genreid = g.genreid
    JOIN album a
    ON a.albumid = t.albumid
    WHERE g.name is 'Alternative'

#### Black Diamond :

* Get all tracks on the playlist(s) called Music and show their name, genre name, album name, and artist name (at least 5 joins)

    SELECT t.name as Track, g.name as Genre, a.title as Album, ar.name as Artist
    FROM track t
    JOIN genre g
    ON t.genreid = g.genreid
    JOIN playlisttrack pt
    ON t.trackid = pt.trackid
    JOIN playlist p
    ON pt.playlistid = p.playlistid
    JOIN album a
    ON t.albumid = a.albumid
    JOIN artist ar
    ON a.artistid = ar.artistid

## Practice nested queries

### Use no joins for the following queries.  Use only nested queries.

__Examples__
```
Select [Column names] from [Table] where columnId in (select columnId from [Table2] where [Condition])

Select Name, email from athlete where athleteId in (select personId from pieEaters where flavor='Apple')
```

* Get all invoices where the unit price on the invoice line is greater than $0.99

    SELECT *
    FROM invoice
    WHERE invoiceid IN
    (SELECT invoiceid FROM invoiceline
    WHERE unitprice > .99)

* Get all Playlist Tracks where the playlist name is Music

    SELECT *
    FROM playlisttrack
    WHERE playlistid IN
    (SELECT playlistid FROM playlist
    WHERE name = 'Music')

* Get all Tracknames for playlistId 5

    SELECT name
    FROM track
    WHERE trackid IN
    (SELECT trackid FROM playlisttrack
    WHERE playlistid = 5)

* Get all tracks where the genre is comedy

    SELECT *
    FROM track
    WHERE genreid IN
    (SELECT genreid FROM genre
    WHERE name = 'Comedy')

* Get all tracks where the album is Fireball

    SELECT *
    FROM track
    WHERE albumid IN
    (SELECT albumid FROM album
    WHERE title = 'Fireball')

* Get all tracks for the artist queen Queen (2 nested subqueries)

    SELECT *
    FROM track
    WHERE albumid IN
    (
    SELECT albumid FROM album
    WHERE artistid IN
    (
    SELECT artistid FROM artist
    WHERE name = 'Queen'
    ))

## Practice updating Rows

__Examples__
```
Update [Table] set [column1]=[value1], [column2]=[value2] where [Condition]

Update Athletes set sport='Picklball' where sport='pockleball'
```

* Find all customers with fax numbers and set those numbers to null

    UPDATE customer
    SET fax = null
    WHERE fax IS NOT null

* Find all customers with no company (null) and set their company to self

    UPDATE customer
    SET company = 'self'
    WHERE company IS null;

* Find the customer `Julia Barnett` and change her last name to `Thompson`

    UPDATE customer
    SET lastname = 'Thompson'
    WHERE firstname like 'Julia'
    AND lastname like 'Barnett'

* Find the customer with this email `luisrojas@yahoo.cl` and change his support rep to rep 4

    UPDATE customer
    SET SupportRepId = 4
    WHERE email like 'luisrojas@yahoo.cl'

* Find all tracks that are of the genre `Metal` and that have no composer and set the composer to be 'The darkness around us'

    UPDATE track
    SET composer = 'The darkness around us'
    WHERE genreid = 3
    AND composer is null

Once you're done with all of those refresh your page to blow away your changes to the database!

## Group by

* Find a count of how many tracks there are per genre

    SELECT count(*)
    FROM track
    group by genreid

* Find a count of all Tracks where the Genre is pop

    SELECT count(*)
    FROM track
    WHERE genreid = 9
    GROUP BY genreid

* Find a list of all artist and how many albums they have

    SELECT ar.name, count(*)
    FROM album
    JOIN artist ar
    ON album.artistid = ar.artistid
    (*)//not needed

## Use Distinct

* From the tracks table find a unique list of all composers

    SELECT distinct composer FROM TRACK

* From the Invoice table find a unique list of all Billing postal codes

    SELECT distinct billingpostalcode FROM invoice

* From the Customer table find a unique list of all companies

    SELECT distinct company FROM customer

## Delete Rows

Always do a select before a delete to make sure you get back exactly what you want and only what you want to delete!

* Remove all pop tracks from the tracks table

    DELETE
    FROM track
    WHERE genreid = 9

* Remove all tracks by `Santana`

    DELETE FROM track
    


* Remove all of the rest of the tracks, yes all of them.

    DELETE

Now refresh your browser to reset all your data



## eCommerce simulation

Let's simulate an e-commerce site.  We're going to need users, products, and orders.

Users need a name and an email.
Products need a name and a price
Orders need a ref to product.
All 3 need primary keys.

Add some data to fill up each table (write down your schema since you won't see it on the side).  You'll need to insert products before you can link them.

Add 2 users, multiple products and multiple orders.

Run some queries against your data:

* Get all products for the first order
* Get all orders
* Get the total cost of an order (sum the price of all products on an order)

## Add foreign key to existing table

Orders have products, but someone needs to place the order.

Add a ref from Orders to Users.  
Add some users.
Update the Orders table to link the a user to each order.

Run some queries against your data:

* Get all orders for a user
* Get how many orders each user has
* __Black Diamond:__ Get the total spend on all orders for each user
