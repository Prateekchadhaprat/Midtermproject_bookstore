Group Members Duties

Prateek: TypeScript interface for table modification and CRUD operations.

Khushboo: Database schema design, creation of tables, and initial data insertion.

Manjot: Sql queries for Requirements and Db testing

```Tables and Attributes ``` 




## Clearly Identify 1 complete set of DDL/DML For one of the tables *Books* you should be able to perform CRUD on all the values of the table

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

## Create a Typescript interface that will allow modification to a table.
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
