---
description: 'Guidelines for generating SQL statements and stored procedures'
applyTo: '**/*.sql'
---

# SQL Server Scripting Guidelines

## General Principles
- Use stored procedures to encapsulate business logic, reduce network traffic, and improve security
- Grant users EXECUTE permissions on procedures instead of direct table access
- Use parameters to pass data into procedures and prevent SQL injection
- Use comments to document the purpose, parameters, and return values of each procedure
- Use consistent indentation and uppercase SQL keywords for readability.
- Make creation and modification scripts idempotent, so they can be run multiple times with identical outcome and without errors or unintended effects.
- For procedure, table, view, etc. create scripts that use NOT EXISTS to  create a stub implementation if needed, followed by a MODIFY statement, to make the script idempotent.

## Naming Conventions
- Use descriptive, PascalCase names that clearly indicate the procedure's purpose (e.g., GetCustomerOrders, UpdateProductPrice)
- Do not use the 'sp_' prefix for user-defined stored procedures; it is reserved for system procedures
- Avoid reserved words, special characters, and square brackets in object names; use only Latin letters and numbers

## Parameter Handling
- Prefix parameters with '@' and use descriptive, camelCase names
- Place required parameters first, followed by optional parameters with default values
- Always specify a length for text-based data types (e.g., varchar(50))
- Validate parameter values at the start of the procedure
- Use OUTPUT parameters to return status or additional values when needed

## Structure and Best Practices
- Include a header comment block with description, parameter list, return information, author, and version
- Use SET NOCOUNT ON at the start of procedures that modify data to reduce unnecessary network traffic
- Use TRY...CATCH blocks for error handling and return standardized error codes or messages
- Return result sets with a consistent column order and data types
- Use temporary tables (prefix with '#' for local, '##' for global) only when necessary; for small data sets, prefer table variables
- Avoid dynamic SQL unless absolutely necessary; if used, always parameterize inputs and use sp_executesql
- Select only the columns needed (avoid SELECT *)
- Use FETCH/OFFSET for pagination
- Strongly avoid using cursors; if needed, ensure they are closed and deallocated properly

## Security
- Always use parameterized queries to prevent SQL injection
- Avoid embedding credentials or sensitive information in scripts
- Use the EXECUTE AS clause to control permissions when needed
- Grant the least privilege required for the procedure to function

## Performance
- Use appropriate indexes on columns frequently used in WHERE, JOIN, or ORDER BY clauses
- Avoid long-running transactions and keep transactions as short as possible
- Recompile procedures if significant changes to underlying tables or data occur and performance degrades
- Use batch processing for large data operations
- Avoid using scalar UDFs in WHERE, JOIN, or SELECT lists unless schema-bound

## Maintenance
- Reuse code by encapsulating repetitive logic in procedures or functions (UDFs)
- Update procedures as needed to reflect changes in business logic or database schema, minimizing changes to client applications

## Additional Notes
- Temporary procedures (names starting with # or ##) are stored in tempdb and are session-scoped
- Extended stored procedures (xp_) are deprecated and should not be used in new development
