Group Members Duties

Prateek: TypeScript interface for table modification and CRUD operations.

Khushboo: Database schema design, creation of tables, and initial data insertion.

Manjot: Sql queries for Requirements and Db testing

```Tables and Attributes ``` 

## Create a Typescript interface that will allow modification to a table.
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

    try {
        await client.query(query, values);
        console.log('Book updated successfully');
    } catch (err) {
	if (err instanceof Error) {
		console.error('Error updating book', err.stack);
	} else {
		console.error('Unexpected error', err);
	}
    } finally {
        await client.end();
    }
}
```
