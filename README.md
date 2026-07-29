# Maritime Analytics Chatbot

A natural-language chatbot that lets you ask questions about maritime operations data in plain English and get answers back, along with a chart. No SQL knowledge needed. You type something like "which vessel type costs the most to insure per year" and it works out the query, runs it against the database, and gives you a clear written answer.

This project was built and tested on sample data I generated for development. It does not use live or confidential commercial figures.

## What it does

You ask a question in plain English. Behind the scenes the chatbot does three things:

1. Converts your question into a SQL query using a large language model.
2. Runs that query against the database and gets the real numbers back.
3. Sends the results back to the model, which writes a short plain-English answer.

It also draws a chart from the result so you get a visual alongside the text. Every answer can be traced back to the exact query that produced it, and you can see that query in the interface, so nothing is a black box.

The important design choice here is that the model never invents numbers. It only writes the query and phrases the answer. The actual figures always come from the database running real SQL. That keeps the answers grounded in the data.

## The data

The sample database covers a small oil-tanker fleet and has five tables:

- **vessels**: vessel details, type, flag, deadweight tonnage, year built
- **crew**: crew members, rank, department, nationality
- **crew_wages**: monthly wage breakdowns per crew member per vessel
- **insurance_policies**: cover type, insurer, premium, insured value per vessel
- **insurance_claims**: claims, amounts, settlements, status

The tables are linked so questions can span across them, for example joining crew wages to vessels, or claims to policies.

## Example questions you can ask

- Average total monthly wage by rank
- Which vessel type costs the most to insure per year
- Claims settlement ratio by cover type
- Total overtime pay by department
- Which insurer has the most open claims
- How many crew of each nationality are there

## How to run it

The whole thing runs in Google Colab, so you do not need to install anything locally.

1. Open the notebook in Google Colab.
2. If you are using the open-source model version, set the runtime to a GPU (Runtime, Change runtime type, T4 GPU).
3. Run the first cell to install the libraries.
4. Run the upload cell and upload the database file when prompted.
5. Run the model cell to load the language model.
6. Run the engine cell, which sets up the query logic.
7. Run the final cell to launch the chatbot. It gives you a link you can open in any browser.

Type a question, or click one of the example prompts, and you will get an answer, the query that was used, a chart, and the full result table.

## How it works under the hood

The chatbot uses a technique called few-shot prompting. The model is given a plain description of the database structure and a handful of example question and query pairs. It reads these fresh each time to understand how to write good queries for this specific data. Nothing is retrained. The model was already trained by its creators, and this project steers it with a well-designed prompt.

There is a safety check built in. Before any query runs, it is inspected to confirm it only reads data and does not modify anything. The chatbot is a read-only analyst by design.

## Why this is useful

Teams in commercial, chartering, and operations roles often sit on large amounts of data but need someone who knows SQL to pull answers out of it. A tool like this puts those answers one plain-English question away. It turns a back-and-forth that used to take time into something that takes seconds, and it opens the data up to people who do not write SQL.

## Notes

This is a working prototype built to prove the concept and get the architecture right. It runs on sample data. The same approach would extend to other databases by updating the schema description and example queries.
