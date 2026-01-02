# Project Title
Expense Tracker MCP Server

## Project Description
This project is a lightweight MCP (Model Context Protocol) server built using FastMCP to manage and track personal expenses. It allows adding expenses, listing expenses within a date range, summarizing expenses by category, and exposing category data as a resource using a local SQLite database.

## Project Vision
The vision of this project is to provide a simple, structured, and extensible MCP server that can be integrated with AI assistants or client applications to track expenses efficiently and generate meaningful financial insights through structured tools.

## Technology Stack
- Programming Language: Python  
- MCP Framework: FastMCP  
- Database: SQLite  
- Data Storage: JSON (for categories)  

## Project Structure
- `main.py` – MCP server implementation with tools and resources  
- `expenses.db` – SQLite database for storing expenses  
- `categories.json` – JSON file containing expense categories  

## MCP Tools
### add_expense
Adds a new expense entry to the database with date, amount, category, optional subcategory, and note.

### list_expenses
Retrieves all expense records within a specified date range.

### summarize
Provides a summary of total expenses grouped by category within a given date range. Optionally filters by a specific category.

## MCP Resources
### expense://categories
Exposes expense categories as a JSON resource, allowing dynamic updates without restarting the server.

## Key Features
- MCP-compliant server using FastMCP
- SQLite-based persistent storage
- Structured tools for expense management
- Resource endpoint for categories
- Simple and extensible architecture
- Suitable for AI agent and assistant integration

## How to Run
1. Install dependencies:
   ```bash
   pip install fastmcp



