# SQL Profiler Normalizer

A tiny static web tool that converts SQL Server Profiler `sp_executesql` output into a readable `EXEC ...` statement.

It runs entirely in the browser and is designed for GitHub Pages:
- no backend
- no database connection
- no data leaves the user's machine

## Live demo

**Website:** https://qwert11.github.io/sql-profiler-normalizer/

## Why this tool exists

SQL Server Profiler often shows stored procedure calls in a parameterized form like this:

```sql
exec sp_executesql N'EXEC SampleProcedure @Id = @P1, @Name = @P2',
N'@P1 int,@P2 nvarchar(50)',
42,
N'John Doe'
```

That format is technically correct, but it is hard to read during debugging.

This tool converts it into something much more readable:

```sql
EXEC SampleProcedure
  @Id = 42,
  @Name = N'John Doe'
```

The goal is simple: paste raw Profiler text, get back a human-friendly SQL call.

## What it does

- Accepts raw SQL Server Profiler text
- Detects `exec sp_executesql ...`
- Extracts the inner stored procedure call
- Replaces `@P1`, `@P2`, and similar placeholders with real values
- Optionally applies readable multi-line formatting
- Lets the user copy the normalized result with one click
- Runs fully client-side in a single HTML file

## Interface behavior

- The page opens with a short built-in demo example
- The output area already shows the normalized result on first load
- Clicking inside the input editor clears the demo text automatically
- `Beauty format` is enabled by default
- `Ctrl+Enter` runs normalization

## Current scope

This project is currently being checked against a real workflow using:

- Delphi 13
- FireDAC
- `DriverID=MSSQL`

## Example

### Input from Profiler

```sql
exec sp_executesql N'EXEC User_save @Id = @P1, @Name = @P2',
N'@P1 int,@P2 nvarchar(50)',
100,
N'Alex'
```

### Normalized output

```sql
EXEC User_save
  @Id = 100,
  @Name = N'Alex'
```

## Files

- `index.html` - complete application in a single file
- `README.md` - project description and usage

## Run locally

Open `index.html` in any modern browser.

## Publish on GitHub Pages

1. Create a GitHub repository
2. Upload `index.html` and `README.md` into the repository root
3. Open **Settings -> Pages**
4. Choose **Deploy from a branch**
5. Select branch `main` and folder `/(root)`
6. Click **Save**

The site will be published at:

https://YOUR_USERNAME.github.io/YOUR_REPOSITORY_NAME/

## Suggested GitHub topics

sql, sql-server, profiler, sp-executesql, formatter, developer-tools, github-pages

## Roadmap

- Improve support for edge cases in `sp_executesql`
- Better handling of nested calls
- Optional syntax highlighting
- Auto-normalize on paste

## Notes

This is a focused utility, not a full SQL parser.
