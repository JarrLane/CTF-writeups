Hello everyone, it has been a while since I have done writeups and now I am back, this weekend was bsides Orlando 2025, and it was an amazing event held at Full Sail University and
there was an amazing CTF called sunshineCTF hosted by the UCF team. 

Category: Web

Challenge: 

<img width="494" height="470" alt="image" src="https://github.com/user-attachments/assets/bcae019c-da81-4d3c-9d62-38efe12e88ec" />

Writeup: 

So we see from the description their flag product for their online store is unreleased, thats what we want, so lets look at the store.

<img width="881" height="515" alt="image" src="https://github.com/user-attachments/assets/c9c40a38-5056-47ef-8de4-2446b08fa87e" />

Only three products???!!!

<img width="1357" height="240" alt="image" src="https://github.com/user-attachments/assets/98614b61-368d-49eb-91bb-b30725b3152e" />

When we look at the URL, we see ```product?product_id=3``` We also notice the product information is formatted like a type of table, what could that mean? 
What happens if we change 3 to 4?

<img width="1357" height="240" alt="image" src="https://github.com/user-attachments/assets/04081a32-39eb-42e1-b197-f7a77d947224" />

We see an unreleased product, if we go 5, 6, 7, etc. could we find the flag? 

<img width="1357" height="240" alt="image" src="https://github.com/user-attachments/assets/a378e90e-8c46-4a90-80bf-9858f55317d1" />

Nope, what if we put something weird on purpose and see what the server does?

<img width="1363" height="233" alt="image" src="https://github.com/user-attachments/assets/8fe2e7ef-9891-424f-bd66-438af2c6e9c3" />

Whats so particular about that error message?

<img width="683" height="238" alt="image" src="https://github.com/user-attachments/assets/7a2ed69f-dfa0-4059-a406-e1ebc3908b33" />

Throwing the error in google shows us that this is an SQLite error, so what can we do with that? Lets try SQL Injection

Before we begin, we have a rough estimate of what our query looks like, since it passes in the id, we can assume it probably looks something like: 

```SELECT * FROM products WHERE product_id = (the parameter we pass)```

We also know that the flag is not in the products table.

We also know that we have to add on to the current query.

What can we do?

In SQLite, one way to have content from multiple tables show up in a single query result is to use ```UNION```. Union takes rows from multiple tables and returns them together, so 
lets see if that works with flag.

(At this point I assumed the flag table was called "flag")

When processed, my query will look like: ```SELECT * FROM products WHERE product_id = 12 UNION SELECT * FROM flag``` This quert is basically telling the server to return all rows where product ID = 12, and all rows from the flag.

<img width="1358" height="246" alt="image" src="https://github.com/user-attachments/assets/a114784d-bdd1-40c8-9270-4493f9a8db11" />

We see two things: there is a flag table, and that the amount of columns in the two tables do not match, so the UNION statement fails.

The UNION statement requires the two tables that are queried to have the same number of columns. but the flag table doesnt have four columns, so what do we do?

https://portswigger.net/web-security/sql-injection/union-attacks

^From reading this we see we can use NULL values to compensate for missing/unknown columns.

In this case lets try ```SELECT * FROM products WHERE product_id = 12 UNION SELECT flag, NULL, NULL, NULL from flag``` (Assuming they named the column flag)

<img width="1365" height="245" alt="image" src="https://github.com/user-attachments/assets/6c667e85-1286-43e6-ac3f-b72de344a8b8" /> 

Now We get the flag

Note, you will notice that in my payload I put an ID of 12, which is not a valid product id. I did this because from what I understand, if the first select statement doesn't work it returns the second, which is the flag.

Thank you for reading my writeup.



