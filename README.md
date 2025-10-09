A powerful Model Context Protocol (MCP) Server for expense tracking that seamlessly integrates with AI assistants like Claude. Track expenses, analyze spending patterns, and manage your finances through natural language conversations!
🌟 Features

✅ Add Expenses - Record transactions with categories and notes
📊 List Expenses - Retrieve expenses within date ranges
📈 Spending Analytics - Generate category-wise summaries
🏷️ Category Management - Organize with categories & subcategories
💾 Persistent Storage - SQLite database for reliable data storage
🔄 Hot Reload - Update categories without server restart
🤖 AI-Friendly - Direct integration with MCP-compatible assistants

📋 Table of Contents

What is MCP?
Installation
Usage
API Reference
Database Schema
Configuration
Examples
Architecture
Contributing
License

🤔 What is MCP?
Model Context Protocol (MCP) is an open standard by Anthropic that enables AI assistants to securely connect with external data sources and tools. It acts as a bridge between AI models and your applications, allowing them to:

Access real-time data
Execute commands
Interact with databases
Perform complex operations

Learn more: Anthropic MCP Documentation
🚀 Installation
Prerequisites

Python 3.8 or higher
pip (Python package manager)

Step 1: Clone the Repository
bashgit clone (https://github.com/abhay032004/expense-tracker-mcp.git)
cd expense-tracker-mcp
Step 2: Install Dependencies
bashpip install fastmcp
Step 3: Create Categories File
Create a categories.json file in the project directory:
json{
  "categories": [
    {
      "name": "Food & Dining",
      "subcategories": ["Groceries", "Restaurants", "Coffee Shops"]
    },
    {
      "name": "Transportation",
      "subcategories": ["Fuel", "Public Transport", "Taxi/Uber"]
    },
    {
      "name": "Shopping",
      "subcategories": ["Clothing", "Electronics", "Home Goods"]
    },
    {
      "name": "Entertainment",
      "subcategories": ["Movies", "Games", "Subscriptions"]
    },
    {
      "name": "Bills & Utilities",
      "subcategories": ["Electricity", "Water", "Internet", "Phone"]
    },
    {
      "name": "Healthcare",
      "subcategories": ["Medicine", "Doctor", "Insurance"]
    },
    {
      "name": "Other",
      "subcategories": []
    }
  ]
}
Step 4: Run the Server
bashpython expense_tracker.py
📖 Usage
With Claude Desktop

Install Claude Desktop application
Configure MCP server in Claude settings
Start chatting with natural language commands!

Example Conversations:
You: Add ₹500 for groceries today
Claude: ✅ Added expense: ₹500 for Groceries on 2025-10-09

You: Show me all expenses from October 1 to October 9
Claude: Here are your expenses:
- 2025-10-01: ₹500 - Groceries
- 2025-10-05: ₹200 - Coffee Shops
...

You: How much did I spend on food this month?
Claude: You spent ₹2,500 on Food & Dining this month.
Standalone Usage
pythonfrom fastmcp import FastMCP

# Initialize client
client = FastMCP("ExpenseTracker")

# Add expense
result = client.add_expense(
    date="2025-10-09",
    amount=500,
    category="Food & Dining",
    subcategory="Groceries",
    note="Weekly shopping"
)

# List expenses
expenses = client.list_expenses(
    start_date="2025-10-01",
    end_date="2025-10-09"
)

# Get summary
summary = client.summarize(
    start_date="2025-10-01",
    end_date="2025-10-09"
)
📚 API Reference
Tools
add_expense(date, amount, category, subcategory="", note="")
Add a new expense entry to the database.
Parameters:

date (string, required): Date in YYYY-MM-DD format
amount (float, required): Expense amount
category (string, required): Expense category
subcategory (string, optional): Subcategory for better organization
note (string, optional): Additional notes

Returns:
json{
  "status": "ok",
  "id": 123
}
Example:
pythonadd_expense(
    date="2025-10-09",
    amount=1500.50,
    category="Shopping",
    subcategory="Electronics",
    note="New headphones"
)

list_expenses(start_date, end_date)
List all expenses within an inclusive date range.
Parameters:

start_date (string, required): Start date in YYYY-MM-DD format
end_date (string, required): End date in YYYY-MM-DD format

Returns:
json[
  {
    "id": 1,
    "date": "2025-10-09",
    "amount": 500.0,
    "category": "Food & Dining",
    "subcategory": "Groceries",
    "note": "Weekly shopping"
  }
]
Example:
pythonexpenses = list_expenses(
    start_date="2025-10-01",
    end_date="2025-10-31"
)

summarize(start_date, end_date, category=None)
Generate spending summary grouped by category.
Parameters:

start_date (string, required): Start date in YYYY-MM-DD format
end_date (string, required): End date in YYYY-MM-DD format
category (string, optional): Filter by specific category

Returns:
json[
  {
    "category": "Food & Dining",
    "total_amount": 5000.0
  },
  {
    "category": "Transportation",
    "total_amount": 2000.0
  }
]
Example:
python# All categories
summary = summarize(
    start_date="2025-10-01",
    end_date="2025-10-31"
)

# Specific category
food_summary = summarize(
    start_date="2025-10-01",
    end_date="2025-10-31",
    category="Food & Dining"
)
Resources
expense://categories
Access category definitions in JSON format.
MIME Type: application/json
Returns: Contents of categories.json file
🗄️ Database Schema
expenses Table
ColumnTypeDescriptionidINTEGERPrimary key (auto-increment)dateTEXTExpense date (YYYY-MM-DD)amountREALExpense amountcategoryTEXTExpense categorysubcategoryTEXTSubcategory (optional)noteTEXTAdditional notes (optional)
SQL Schema:
sqlCREATE TABLE IF NOT EXISTS expenses(
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    date TEXT NOT NULL,
    amount REAL NOT NULL,
    category TEXT NOT NULL,
    subcategory TEXT DEFAULT '',
    note TEXT DEFAULT ''
)
⚙️ Configuration
File Structure
expense-tracker-mcp/
├── expense_tracker.py      # Main MCP server file
├── categories.json         # Category definitions
├── expenses.db            # SQLite database (auto-created)
├── README.md              # This file
└── requirements.txt       # Python dependencies
Environment Variables
No environment variables required. All configuration is file-based.
Database Location
By default, the database is created in the same directory as the script:
pythonDB_PATH = os.path.join(os.path.dirname(__file__), "expenses.db")
To change location, modify the DB_PATH variable in expense_tracker.py.
💡 Examples
Scenario 1: Daily Expense Tracking
python# Morning coffee
add_expense("2025-10-09", 150, "Food & Dining", "Coffee Shops", "Starbucks")

# Lunch
add_expense("2025-10-09", 350, "Food & Dining", "Restaurants", "Office cafeteria")

# Evening groceries
add_expense("2025-10-09", 2500, "Food & Dining", "Groceries", "Monthly shopping")
Scenario 2: Monthly Analysis
python# Get all October expenses
october_expenses = list_expenses("2025-10-01", "2025-10-31")

# Get spending summary
summary = summarize("2025-10-01", "2025-10-31")

# Get food expenses only
food_summary = summarize("2025-10-01", "2025-10-31", "Food & Dining")
Scenario 3: Budget Monitoring
python# Check transportation spending
transport = summarize("2025-10-01", "2025-10-09", "Transportation")
total = transport[0]["total_amount"]

if total > 5000:
    print(f"Warning: Transportation budget exceeded! Spent: ₹{total}")
🏗️ Architecture
┌─────────────────┐
│   AI Assistant  │
│    (Claude)     │
└────────┬────────┘
         │ MCP Protocol
         │
┌────────▼────────┐
│  FastMCP Server │
│                 │
│  ┌───────────┐  │
│  │  Tools    │  │
│  ├───────────┤  │
│  │ Resources │  │
│  └───────────┘  │
└────────┬────────┘
         │
┌────────▼────────┐
│ SQLite Database │
│  expenses.db    │
└─────────────────┘
Components

MCP Server Layer - FastMCP framework handling protocol communication
Tool Layer - Three core functions (add, list, summarize)
Resource Layer - Category definitions endpoint
Data Layer - SQLite database for persistence

🔒 Security Considerations

✅ Parameterized SQL queries prevent SQL injection
✅ Local database storage (no external connections)
✅ Context managers ensure proper resource cleanup
⚠️ No authentication (single-user design)
⚠️ No encryption (suitable for local use)

For production use, consider adding:

User authentication
Database encryption
Input validation
Rate limiting
Audit logging

🐛 Troubleshooting
Database Locked Error
Problem: sqlite3.OperationalError: database is locked
Solution: Ensure no other process is accessing the database. Close any SQLite browser tools.
Import Error
Problem: ModuleNotFoundError: No module named 'fastmcp'
Solution: Install FastMCP:
bashpip install fastmcp
Categories Not Loading
Problem: Categories not appearing in AI assistant
Solution: Ensure categories.json exists in the same directory as the script.
🚀 Future Enhancements

 Multi-user support with authentication
 Receipt image OCR integration
 Budget alerts and notifications
 Data visualization dashboard
 Export to Excel/PDF
 Multi-currency support
 Recurring expense templates
 Mobile app integration
 Cloud sync capabilities
 Advanced analytics with charts

🤝 Contributing
Contributions are welcome! Please follow these steps:

Fork the repository
Create a feature branch (git checkout -b feature/AmazingFeature)
Commit your changes (git commit -m 'Add some AmazingFeature')
Push to the branch (git push origin feature/AmazingFeature)
Open a Pull Request

Code Style

Follow PEP 8 guidelines
Add docstrings to functions
Include type hints where possible
Write meaningful commit messages

📄 License
This project is licensed under the MIT License - see the LICENSE file for details.
👨‍💻 Author
Abhay Raj

GitHub: @abhay032004
LinkedIn: Abhay Raj
Email: abhay.raj.job@gmail.com

🙏 Acknowledgments

Anthropic for the MCP protocol
FastMCP for the amazing framework
SQLite for reliable database engine
The open-source community





⭐ If you find this project useful, please give it a star! ⭐
Made with ❤️ and Python

