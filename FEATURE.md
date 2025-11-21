# FEATURE.md – CashierAI

## 1. Overview

CashierAI is an AI-powered money management mobile app that helps users track daily expenses, loans, debts, and financial behavior. It includes a friendly, emotional AI assistant capable of generating insights, charts, and PDF reports.

## 2. Core Features

- AI-powered chat for queries, analytics, and recommendations
- Expense, income, loan, and debt tracking
- Personalized financial insights
- Multi-format outputs (text, charts, tables, PDFs)
- Premium-only features:
  - Receipt image recognition
  - Automated monthly PDF reports
  - Unlimited AI interactions
  - Advanced analytics & emotional AI modes

### 2.1 Tone / Personality Options

- Friendly
- Funny
- Slightly dramatic
- Upset (especially when user overspends)
- Professional (for analytics and reports)
- Ultra-roast (Premium)

## 3. Authentication & Onboarding

### 3.1 Login

- Phone number input
- OTP verification
- Auto-login session persistence
- Optional basic device-level security checks

### 3.2 First-Time Onboarding

Users answer:

- Name, age
- Preferred currency and language
- Monthly income
- Spending categories (select presets or add custom)
- Financial goals (e.g., “save BDT 50,000 in 6 months”)
- Optional: future ability to sync bank accounts

AI introduces itself:

- Explains how it will help (tracking, insights, alerts)
- Shows how to add transactions (manual + natural language)
- Invites the user to try a first query like: “Add my first expense”

## 4. Home Page

### 4.1 Quick Actions

- Add expense, income, loan given, debt taken
- Natural language interpretation, e.g.:
  - “Paid 650 for Uber yesterday”
    - Category: Transport
    - Amount: 650
    - Date: Yesterday

### 4.2 Ask Reports (Natural-Language Reports)

User can request:

- “Show me my expenses last week”
- “How much did I spend on food in October?”
- “Show debt and loan summary for 2025”
- “Create a pie chart of my expenses for this month”

Supported report themes:

- Daily/weekly/monthly summaries
- Category-based spending
- Loans & debt summaries
- Trends (e.g., “am I spending more?”)
- Output as charts, tables, summaries

### 4.3 Insights

- Overspending alerts
- Ratio analyses
- Budget warnings
- Saving suggestions

## 5. Transactions Module

The dedicated page for transaction management.

### 5.1 Transaction List

Filters:

- Date range
- Category
- Amount range
- Transaction type (expense / income / loan / debt)
- Tags
- Payment method (cash / bKash / card / etc.)

Sort options:

- Date (new → old, old → new)
- Amount (high → low, low → high)
- A–Z by category

### 5.2 CRUD Operations

- Add Transaction
- Update Transaction
- Delete Transaction
- Bulk Delete (optional)
- Duplicate Transaction
- Export Transactions (CSV / Premium PDF)

### 5.3 Smart AI Search

AI-powered query-based result filtering, e.g.:

- “Show me all purchases above 5000 this month”
- “Food expenses for October”
- “Expenses between 10–20 Jan”

### 5.4 Premium-Only: Receipt Image Recognition

Features:

- Upload camera photo or gallery image
- Auto-extract:
  - Amount
  - Category
  - Merchant
  - Time/date
- Auto-fill transaction form with ability to edit before saving

## 6. Reports & PDF Exports

### 6.1 AI-Generated Reports

User can ask:

- “Give me a PDF summary for last month”
- “Monthly expense report in PDF”
- “PDF of all transactions in March”

Reports include:

- Expense summaries
- Category distribution
- Income vs expenses
- Loans & debts overview
- Trends analysis
- Insights + personalized comments
- Charts rendered into PDF (Premium)

### 6.2 PDF Types (Premium)

- Monthly Financial Report
- Yearly Report
- Category-based report
- Loan/Debt PDF
- Full transaction export

All PDFs include signature CashierAI commentary, with tone based on the selected personality mode.

## 7. Tools & Automations

- Budget planner
- Savings tracker
- Debt manager
- Bill reminder
- Recurring transactions
- Premium: Automated PDF reports, trend insights, roast/praise mode

## 8. AI Capabilities

### Input

- Text
- Natural language
- Receipt images (Premium)

### Output

- Text
- Charts
- Tables
- Summaries
- PDFs

### Personality Modes

- Friendly
- Funny
- Slightly dramatic
- Upset (especially on overspending)
- Professional (analytics-focused)
- Ultra-roast (Premium)

### Personalized Insights & Examples

Emotional AI personality modes (funny, serious/professional, ultra-roast 🔥😆) adapt feedback and language.

Example outputs:

- “You spent 20% more on food this week 😡 why bro why??”
- “Great job! You saved more than last month 🥳”
- “If you reduce your snack expenses by 30%, you’ll save an extra 5,200 BDT this month.”
- “Bro, you spent 12,500 BDT on snacks this month 😭 why.”
- “🔥 Amazing! You saved 28% more this month.”
- “You’re at risk of overspending your food budget again 😡 behave!”
- “If you reduce outings by 20%, you’ll save BDT 3,200/month.”

## 9. Profile & Settings

- User profile
- Currency & theme
- Subscription plans
- Notifications
- Backup & restore
- CSV export/import

## 10. Plans & Access Levels

### 10.1 Free Plan

- Limited AI prompts per hour
- Text-only prompts (no voice or image commands)
- No report exports (no PDF exports; CSV only where allowed)
- No automatic scheduled reports
- No automations (bill reminders, recurring insights, etc.)

### 10.2 Premium Plan

- Text, voice, and image (receipt) prompts
- Extended prompts per hour
- Access to all Premium features (receipt OCR, PDF exports, advanced analytics, emotional modes, automations)

### 10.3 Enterprise / Family / Team Plan

- Team access with roles:
  - Admin
  - Cashier (read/write)
  - Observer (read-only; name can be adjusted per market)
- Extended hourly prompt limits per user
- For families, companies, and institutions
- Includes all Premium features, plus team-level controls and shared spaces

## 11. Future Add-ons

- Bank API sync
- Shared budgets
- Web dashboard
- Invoice scanning
