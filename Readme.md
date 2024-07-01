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
    **NOTE**: PostgreSQL does not permit direct access to the PSQL Tool for querying; therefore, it is necessary to first create a database before executing queries. If creating the database programmatically is required, the following query can be used for database creation.

    ```sql
    DROP DATABASE IF EXISTS book_store;
    CREATE DATABASE book_store WITH TEMPLATE = template0 ENCODING = 'UTF8' LOCALE_PROVIDER = libc LOCALE = 'C';

* ## Sample Database creation SQL Query 
    
    ```sql
    CREATE TYPE public.book_format AS ENUM (
        'Physical',
        'E-book',
        'Audiobook'
    );

    CREATE TYPE public.user_type AS ENUM (
        'Customer',
        'Author',
        'Publisher'
    );

    CREATE TABLE public.books (
        id integer NOT NULL,
        title character varying(255) NOT NULL,
        price money,
        author_id integer,
        publisher_id integer,
        publication_date date,
        user_rating double precision,
        book_format public.book_format NOT NULL,
        genre_id integer NOT NULL
    );

    ALTER TABLE public.books ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
        SEQUENCE NAME public.books_id_seq
        START WITH 1
        INCREMENT BY 1
        NO MINVALUE
        NO MAXVALUE
        CACHE 1
    );

    CREATE TABLE public.genre (
        id integer NOT NULL,
        title character varying NOT NULL
    );

    ALTER TABLE public.genre ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
        SEQUENCE NAME public.genre_id_seq
        START WITH 1
        INCREMENT BY 1
        NO MINVALUE
        NO MAXVALUE
        CACHE 1
    );


    CREATE TABLE public.orders (
        id integer NOT NULL,
        customer_id integer NOT NULL,
        book_id integer NOT NULL,
        order_date date NOT NULL,
        price money NOT NULL,
        quantity numeric(10,0) NOT NULL,
        total_amount money NOT NULL
    );

    ALTER TABLE public.orders ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
        SEQUENCE NAME public.orders_id_seq
        START WITH 1
        INCREMENT BY 1
        NO MINVALUE
        NO MAXVALUE
        CACHE 1
    );

    CREATE TABLE public.reviews (
        id integer NOT NULL,
        customer_id integer NOT NULL,
        book_id integer NOT NULL,
        comment text NOT NULL,
        rating double precision NOT NULL,
        posted_date date NOT NULL
    );

    ALTER TABLE public.reviews ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
        SEQUENCE NAME public.reviews_id_seq
        START WITH 1
        INCREMENT BY 1
        NO MINVALUE
        NO MAXVALUE
        CACHE 1
    );

    CREATE TABLE public.users (
        id integer NOT NULL,
        first_name character varying(255) NOT NULL,
        last_name character varying(255),
        email character varying(255),
        password character varying(255),
        type public.user_type,
        created_date date
    );


    ALTER TABLE public.users ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
        SEQUENCE NAME public.users_id_seq
        START WITH 1
        INCREMENT BY 1
        NO MINVALUE
        NO MAXVALUE
        CACHE 1
    );

    INSERT INTO public.books OVERRIDING SYSTEM VALUE VALUES (1, 'The Women', '$19.99', 1, 14, '2024-06-19', 3.7, 'Physical', 1) ON CONFLICT DO NOTHING;
    INSERT INTO public.books OVERRIDING SYSTEM VALUE VALUES (3, 'After Death', '$10.44', 3, 15, '2020-01-29', 4.1, 'Audiobook', 4) ON CONFLICT DO NOTHING;
    INSERT INTO public.books OVERRIDING SYSTEM VALUE VALUES (4, 'A Place Called Freedom', '$25.00', 4, 15, '2001-08-29', 3, 'E-book', 4) ON CONFLICT DO NOTHING;
    INSERT INTO public.books OVERRIDING SYSTEM VALUE VALUES (5, 'The House Across the Lake', '$12.00', 5, 14, '2020-10-16', 2.1, 'Physical', 2) ON CONFLICT DO NOTHING;
    INSERT INTO public.books OVERRIDING SYSTEM VALUE VALUES (6, 'The Haunting of Hill House', '$14.00', 6, 14, '2001-04-10', 4.3, 'Physical', 2) ON CONFLICT DO NOTHING;
    INSERT INTO public.books OVERRIDING SYSTEM VALUE VALUES (7, 'History of India', '$50.30', 10, 14, '2022-07-10', 4.9, 'Audiobook', 5) ON CONFLICT DO NOTHING;
    INSERT INTO public.books OVERRIDING SYSTEM VALUE VALUES (8, 'Hamlet', '$43.00', 9, 15, '2005-02-10', 4.4, 'E-book', 5) ON CONFLICT DO NOTHING;
    INSERT INTO public.books OVERRIDING SYSTEM VALUE VALUES (2, 'House of Flame and Shadow', '$30.00', 2, 15, '2022-08-20', 4.5, 'E-book', 1) ON CONFLICT DO NOTHING;
    INSERT INTO public.books OVERRIDING SYSTEM VALUE VALUES (9, 'This is test', '$12.00', 1, 14, '2024-01-19', 2, 'Physical', 1) ON CONFLICT DO NOTHING;

    INSERT INTO public.genre OVERRIDING SYSTEM VALUE VALUES (1, 'Action') ON CONFLICT DO NOTHING;
    INSERT INTO public.genre OVERRIDING SYSTEM VALUE VALUES (2, 'Horror') ON CONFLICT DO NOTHING;
    INSERT INTO public.genre OVERRIDING SYSTEM VALUE VALUES (3, 'Mystery') ON CONFLICT DO NOTHING;
    INSERT INTO public.genre OVERRIDING SYSTEM VALUE VALUES (4, 'Thriller') ON CONFLICT DO NOTHING;
    INSERT INTO public.genre OVERRIDING SYSTEM VALUE VALUES (5, 'History') ON CONFLICT DO NOTHING;


    INSERT INTO public.orders OVERRIDING SYSTEM VALUE VALUES (8, 11, 3, '2024-01-19', '$29.00', 1, '$29.00') ON CONFLICT DO NOTHING;
    INSERT INTO public.orders OVERRIDING SYSTEM VALUE VALUES (6, 11, 7, '2024-03-03', '$20.75', 3, '$62.25') ON CONFLICT DO NOTHING;
    INSERT INTO public.orders OVERRIDING SYSTEM VALUE VALUES (5, 12, 2, '2023-05-05', '$19.00', 1, '$19.00') ON CONFLICT DO NOTHING;
    INSERT INTO public.orders OVERRIDING SYSTEM VALUE VALUES (7, 10, 2, '2023-04-20', '$35.00', 1, '$35.00') ON CONFLICT DO NOTHING;
    INSERT INTO public.orders OVERRIDING SYSTEM VALUE VALUES (9, 12, 4, '2023-04-01', '$15.50', 2, '$31.00') ON CONFLICT DO NOTHING;

    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (1, 11, 2, 'It''s a must read. Amazing book!!!', 5, '2024-05-05') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (9, 11, 2, 'This BOOK is unexplainable, it should be experienced each and every sentence', 4.5, '2024-02-28') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (10, 11, 1, 'Average book.', 2.5, '2024-02-13') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (11, 11, 4, 'Lorem Ipsum Lorem Ipsum', 2, '2024-02-20') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (12, 11, 8, 'Test review from the user', 3, '2024-02-15') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (13, 11, 1, 'Overall good book. Relationships are like mirrors and shows our inner world, the are helping in knowing our inner world . Great chapters for every common man . Worth reading and referenceable book', 2, '2024-01-14') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (14, 12, 7, 'Test Test Test!!!!', 3.5, '2024-03-10') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (15, 13, 5, 'A test review for this book.', 1, '2024-04-04') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (16, 11, 2, 'I like the way author presented the key points.', 3, '2024-01-19') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (17, 11, 4, 'This book is a good one. Unexpected endings.', 2.5, '2023-01-05') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (7, 11, 6, 'Overall good book. I like this author a lot but this one is not the best one from the author.', 3.5, '2024-01-15') ON CONFLICT DO NOTHING;
    INSERT INTO public.reviews OVERRIDING SYSTEM VALUE VALUES (8, 11, 5, 'It is a nice read but I did not like the way it ended.', 3.5, '2024-03-03') ON CONFLICT DO NOTHING;


    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (1, 'Kristin', 'Hannah', 'kristin@book.com', NULL, 'Author', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (2, 'Sarah', 'Maas', 'sarah@book.com', NULL, 'Author', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (3, 'Dean', 'Koontz', 'dean@book.com', NULL, 'Author', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (4, 'Ken', 'Follett', 'ken@book.com', NULL, 'Author', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (5, 'Riley', 'Sager', NULL, NULL, 'Author', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (6, 'Shirley', 'Jackson', NULL, NULL, 'Author', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (9, 'William', 'Shakespeare', NULL, NULL, 'Author', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (10, 'Billy', 'Wellman', NULL, NULL, 'Author', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (11, 'Khushboo', 'R', 'khush@book.com', NULL, 'Customer', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (12, 'Pratik', 'C', 'pratik@book.com', NULL, 'Customer', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (13, 'Customer', '1', 'customer@book.com', NULL, 'Customer', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (14, 'Publisher', 'Test', NULL, NULL, 'Publisher', NULL) ON CONFLICT DO NOTHING;
    INSERT INTO public.users OVERRIDING SYSTEM VALUE VALUES (15, 'Joe', 'Test', NULL, NULL, 'Publisher', NULL) ON CONFLICT DO NOTHING;


    ALTER TABLE ONLY public.books
        ADD CONSTRAINT books_pkey PRIMARY KEY (id);

    ALTER TABLE ONLY public.genre
        ADD CONSTRAINT genre_pkey PRIMARY KEY (id);


    ALTER TABLE ONLY public.orders
        ADD CONSTRAINT orders_pkey PRIMARY KEY (id);

    ALTER TABLE ONLY public.reviews
        ADD CONSTRAINT reviews_pkey PRIMARY KEY (id);

    ALTER TABLE ONLY public.users
        ADD CONSTRAINT users_pkey PRIMARY KEY (id);

    ALTER TABLE ONLY public.books
        ADD CONSTRAINT author_id_fkey FOREIGN KEY (author_id) REFERENCES public.users(id) ON DELETE CASCADE NOT VALID;


    ALTER TABLE ONLY public.reviews
        ADD CONSTRAINT book_id_fkey FOREIGN KEY (book_id) REFERENCES public.books(id) NOT VALID;

    ALTER TABLE ONLY public.orders
        ADD CONSTRAINT book_id_fkey FOREIGN KEY (book_id) REFERENCES public.books(id);

    ALTER TABLE ONLY public.reviews
        ADD CONSTRAINT customer_id_fkey FOREIGN KEY (customer_id) REFERENCES public.users(id);

    ALTER TABLE ONLY public.orders
        ADD CONSTRAINT customer_id_fkey FOREIGN KEY (customer_id) REFERENCES public.users(id);

    ALTER TABLE ONLY public.books
        ADD CONSTRAINT genre_id_fkey FOREIGN KEY (genre_id) REFERENCES public.genre(id) NOT VALID;

    ALTER TABLE ONLY public.books
        ADD CONSTRAINT publisher_id_fkey FOREIGN KEY (publisher_id) REFERENCES public.users(id) NOT VALID;

    REVOKE USAGE ON SCHEMA public FROM PUBLIC;
    GRANT ALL ON SCHEMA public TO PUBLIC;

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
