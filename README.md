# Natural-Language SQL Chatbot

A chatbot that lets you ask questions about a database in plain English and get answers back, along with a chart. No SQL knowledge needed. You type something like "which vessel type costs the most to insure per year" and it works out the query, runs it against the database, and gives you a clear written answer.

The key thing about this version is that it is not tied to any one dataset. It reads the structure of whatever SQLite database you give it and configures itself, so the same tool works on maritime data today and could work on finance, logistics, or any other operational data tomorrow.

This project was built and tested on sample data I generated for development. It does not use live or confidential commercial figures.

## What it does

You ask a question in plain English. Behind the scenes the chatbot does three things:

1. Converts your question into a SQL query using a large language model.
2. Runs that query against the database and gets the real numbers back.
3. Sends the results back to the model, which writes a short plain-English answer.

It also draws a chart from the result. It picks the chart type based on the question, a line graph when the answer is a trend over time and a bar chart for straight comparisons. Every answer can be traced back to the exact query that produced it, and you can see that query in the interface, so nothing is a black box.

The important design choice here is that the model never invents numbers. It only writes the query and phrases the answer. The actual figures always come from the database running real SQL. That keeps the answers grounded in the data.

## Works on any SQLite database

You do not have to describe your tables to it by hand. When you upload a database, the app reads the database's own structure, the tables, the columns, and the relationships between them, and builds its understanding automatically. That means you can point it at a completely different dataset and it adapts, no code rewrite needed.

For best accuracy on a specific database you can optionally add a few example question and query pairs for that data, but it will work out of the box without them.

## Example questions you can ask

On the maritime sample data:

- Average total monthly wage by rank
- Which vessel type costs the most to insure per year
- Claims settlement ratio by cover type
- Total wage bill by pay month (this one draws a line chart)
- How many crew of each nationality are there

The same style of question works on any other dataset once you upload it.

## How to run it

The whole thing runs in Google Colab, so you do not need to install anything locally.

1. Open the notebook in Google Colab.
2. Set the runtime to a GPU (Runtime, Change runtime type, T4 GPU). The model runs on the GPU.
3. Run the first cell to install the libraries.
4. Run the upload cell and upload any SQLite database file when prompted.
5. Run the model cell to load the language model.
6. Run the engine cell. It reads your database structure automatically and prints the detected schema.
7. Run the final cell to launch the chatbot. It gives you a link you can open in any browser.

Type a question, or click one of the example prompts, and you will get an answer, the query that was used, a chart, and the full result table.

If the larger model runs out of GPU memory on a free Colab session, switch to the smaller model option noted in the model cell, restart the runtime, and run the cells again.

## How it works under the hood

The chatbot uses a technique called few-shot prompting. The model is given a description of the database structure, which is read automatically from the database itself, and optionally a handful of example question and query pairs. It reads these fresh each time to understand how to write good queries for that specific data. Nothing is retrained. The model was already trained by its creators, and this project steers it with a well-designed prompt.

There is a safety check built in. Before any query runs, it is inspected to confirm it only reads data and does not modify anything. The chatbot is a read-only analyst by design.

## Why this is useful

Teams in commercial, chartering, and operations roles often sit on large amounts of data but need someone who knows SQL to pull answers out of it. A tool like this puts those answers one plain-English question away. It turns a back-and-forth that used to take time into something that takes seconds, and it opens the data up to people who do not write SQL. Because it adapts to any database, the same tool can serve very different teams.

## Notes

This is a working prototype built to prove the concept and get the architecture right. It runs on sample data. Pointing it at a new database is as simple as uploading a different file.
