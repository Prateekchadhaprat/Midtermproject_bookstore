###### Group Members Duties

**Prateek 8967796: TypeScript interface for table modification and CRUD operations.**

**Khushboo: Database schema design, creation of tables, and initial data insertion.**

**Manjot: SQL queries for Requirements and DB Testing.**

###### Github URL
[MidtermprojectDB](https://github.com/Prateekchadhaprat/Midtermproject_bookstore/)

* ## Clearly title and identify the tables required, the data you are capturing, and what type each of the attribute is
###### Tables and Attributes 

Users

| Field Name  | Type | Key Type 
| --------- | ------- | ------- | 
| Id |  integer | PRIMARY KEY
| name | character varying(255) | -
| email | character varying(255) | -
| password | character varying(255) | -
| type | ENUM ( Customer','Author','Publisher') | -
| created_at | date | -

Genre

| Field Name  | Type | Key Type 
| --------- | ------- | ------- | 
| Id |  integer | PRIMARY KEY
| title | character varying(255) | -

Books

| Field Name  | Type | Key Type 
| --------- | ------- | ------- | 
| Id |  integer | PRIMARY KEY
| title | character varying(255) | -
| price | money  | -
| author_id | integer | FOREIGN KEY
| publisher_id | integer | FOREIGN KEY
| publication_date | date | -
| user_rating  | double precision | -
| book_format | ENUM ( Physical','E-book','Audiobook') | -
| genre_id | integer | FOREIGN KEY

Orders

| Field Name  | Type | Key Type 
| --------- | ------- | ------- | 
| Id |  integer | PRIMARY KEY
| customer_id | integer | FOREIGN KEY
| book_id | integer | FOREIGN KEY
| order_date | date | -
| price | money | -
| quantity | numeric(10,0) | -
| total_amount  | money | -

Reviews

| Field Name  | Type | Key Type
| --------- | ------- | ------- | 
| Id |  integer | PRIMARY KEY
| customer_id | integer | FOREIGN KEY
| book_id | integer | FOREIGN KEY
| comment | text | -
| rating | double precision | -
| posted_date | date | -

* ## Submit a code block containing only valid sql syntax which will create your sample data base.


* ## Clearly Identify 1 complete set of DDL/DML For one of the tables *Books* you should be able to perform CRUD on all the values of the table

###### Create
``CREATE TABLE public.books (
    id integer NOT NULL,
    title character varying(255) NOT NULL,
    price money,
    author_id integer,
    publisher_id integer,
    publication_date date,
    user_rating double precision,
    book_format public.book_format NOT NULL,
    genre_id integer NOT NULL
);``

###### Insert
```INSERT INTO public.books OVERRIDING SYSTEM VALUE VALUES (1, 'The Women', '$19.99', 1, 14, '2024-06-19', 3.7, 'Physical', 1) ON CONFLICT DO NOTHING;```

###### Read
```SELECT * FROM Books WHERE author_id = 1;```

###### Update
```UPDATE Books SET price money = '19.99' WHERE id = 1;```
###### Delete
```DELETE FROM Books WHERE id = 2;```

* ## Create SQL queries for the above listed requirements. They should be single queries. Not a series of steps
* Power writers (authors) with more than X books in the same genre published within the last X years
  ```sql
  SELECT u.name
  FROM Users u
  JOIN Books b ON u.id = b.author_id
  WHERE b.genre_id = 4 AND b.publication_date > CURRENT_DATE - INTERVAL '5 years'
  GROUP BY u.id
  HAVING COUNT(b.id) > 8;

* Loyal Customers who has spent more than X dollars in the last year
  ```sql
  SELECT name, email
  FROM Users
  WHERE total_spent > 5 AND user_type = 'Customer' AND join_date > CURRENT_DATE - INTERVAL '1 year';

* Well Reviewed books that has a better user rating than average
  ```sql
  SELECT title, average_rating
  FROM Books
  WHERE average_rating > (SELECT AVG(average_rating) FROM Books);

* The most popular genre by sales
  ```sql
  SELECT g.name
  FROM Genre g
  JOIN Books b
  ON g.id = b.genre_id
  JOIN Orders o
  ON b.id = o.book_id
  GROUP BY g.id
  ORDER BY SUM(o.quantity * b.price)
  DESC LIMIT 1;

* The 10 most recent posted reviews by Customers 
  ```sql
  SELECT u.name, b.title, r.rating, r.comment, r.review_date
  FROM Reviews r
  JOIN Users u
  ON r.customer_id = u.id
  JOIN Books b
  ON r.book_id = b.id
  ORDER BY r.review_date
  DESC LIMIT 10;

* ## Create a Typescript interface that will allow modification to a table.
###### Update Book
```import { Client } from 'pg';

export interface Book {
    book_id: number;
    title: string;
    genre: string;
    publish_date: Date;
    author_id: number;
    publisher_id: number;
    format: string;
    price: number;
}

export async function updateBook(book: Book): Promise<void> {
    const client = new Client({
        user: 'postgres',           
        host: '172.18.0.3',        
        database: 'book_store', 
        password: 'password',  
        port: 5432,                 
    });

    await client.connect();

    const query = `
        UPDATE Books
        SET title = $1, genre = $2, publish_date = $3, author_id = $4, publisher_id = $5, format = $6, price = $7
        WHERE book_id = $8
    `;
    const values = [book.title, book.genre, book.publish_date, book.author_id, book.publisher_id, book.format, book.price, book.book_id];

    
```
###### Insert Book
```import { Client } from 'pg';

export interface Book {
    book_id?: number;
    title: string;
    genre: string;
    publish_date: Date;
    author_id: number;
    publisher_id: number;
    format: string;
    price: number;
}

export async function insertBook(book: Book): Promise<void> {
    const client = new Client({
        user: 'postgres',
        host: '172.18.0.3',
        database: 'book_store',
        password: 'password',
        port: 5432,
    });

    await client.connect();

    const query = `
        INSERT INTO Books (title, genre, publish_date, author_id, publisher_id, format, price)
        VALUES ($1, $2, $3, $4, $5, $6, $7)
    `;
    const values = [book.title, book.genre, book.publish_date, book.author_id, book.publisher_id, book.format, book.price];

    
}
```

###### Select Book
```import { Client } from 'pg';

export interface Book {
    book_id: number;
    title: string;
    genre: string;
    publish_date: Date;
    author_id: number;
    publisher_id: number;
    format: string;
    price: number;
}

export async function selectBook(bookId: number): Promise<Book | null> {
    const client = new Client({
        user: 'postgres',
        host: '172.18.0.3',
        database: 'book_store',
        password: 'password',
        port: 5432,
    });

    await client.connect();

    const query = `
        SELECT book_id, title, genre, publish_date, author_id, publisher_id, format, price
        FROM Books
        WHERE book_id = $1
    `;
    const values = [bookId];

    
```
###### Delete Book
```import { Client } from 'pg';

export async function deleteBook(bookId: number): Promise<void> {
    const client = new Client({
        user: 'postgres',
        host: '172.18.0.3',
        database: 'book_store',
        password: 'password',
        port: 5432,
    });

    await client.connect();

    const query = `
        DELETE FROM Books
        WHERE book_id = $1
    `;
    const values = [bookId];

    
```
