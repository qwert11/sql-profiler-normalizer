# SQL Profiler Normalizer

A tiny static web tool for turning SQL Server Profiler `sp_executesql` output into a readable `EXEC ...` statement.

The project is designed for GitHub Pages, so it runs fully in the browser with no backend, no database connection, and no data leaving the user's machine.

## What it does

- Accepts raw SQL Server Profiler text
- Detects `exec sp_executesql ...`
- Extracts the inner stored procedure call
- Replaces `@P1`, `@P2`, and similar placeholders with real values
- Optionally applies light formatting for better readability
- Lets the user copy the normalized result with one click

## Interface behavior

- The page opens with a short demo example already loaded
- The output area immediately shows the normalized result, so the tool is understandable at first glance
- Clicking inside the input editor clears the demo text automatically
- `Beauty format` is enabled by default
- `Ctrl+Enter` runs normalization

## Current scope

This tool is currently being checked against a real workflow with:

- Delphi 13
- FireDAC
- `DriverID=MSSQL`

That means the current parser is aimed first at the SQL shape commonly produced by this stack in SQL Server Profiler.

## Example

Input:

```sql
exec sp_executesql N'EXEC Customer_save
  @ID = @P1,
  @Name = @P2',N'@P1 int,@P2 nvarchar(50)',42,N'John Carter'
```

Output:

```sql
EXEC Customer_save
  @ID = 42,
  @Name = N'John Carter'
```

## Files

- `index.html` - complete app in a single file
- `README.md` - project description and usage notes

## Run locally

Just open `index.html` in a browser.

## Publish on GitHub Pages

1. Create a repository
2. Upload `index.html` and `README.md` to the repository root
3. Open **Settings -> Pages**
4. Choose **Deploy from a branch**
5. Select branch `main` and folder `/(root)`
6. Save

The site will be published at:

```text
https://YOUR_USERNAME.github.io/YOUR_REPOSITORY_NAME/
```

## Notes

This is intentionally a focused utility, not a universal SQL parser.

If a future Profiler case uses a different `sp_executesql` layout, the parser can be extended with additional rules while keeping the UI simple.


## Latest update

Improved beauty formatting now places each parameter on its own line after normalization, closer to SSMS-style readable EXEC output.
