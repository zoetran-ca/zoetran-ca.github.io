---
# the default layout is 'page'
icon: fas fa-book-circle
tags: [automation, python]
---
# Automating Household Finances: How I Built a Low-Overhead Budget Tracker

Let’s be honest: nobody wants to spend their weekends staring at spreadsheets. Since our baby arrived, my 'free time' has become non-existent. I built this because I wanted a way to stay on top of our finances without it feeling like a second job. Every minute I save on data entry is another minute I get to spend with my family.

This project was born from the need for **shared visibility** and **minimal effort**. I didn't want to be the sole "gatekeeper" of our financial data; I wanted my spouse to have the same bird's-eye view without me having to give a formal briefing every time. Now, we’re on the same page at a glance, and neither of us has to ask, 'Where did the money go this week?' 

## The Philosophy: High-Level Numbers, Low-Level Effort

While designing a complex, interactive dashboards is totally doable, we made a deliberate choice to keep things simple. We don't need a high-frequency trading platform for our groceries; we just need high-level numbers that tell us if we are on track. 

### Under the Hood: The Tech Stack

Under the hood, I made some choices to keep this as 'low-maintenance' as possible. I set up an incremental update system so the code only looks at new receipts, and I used Parquet files because they’re way faster and cleaner than standard spreadsheets. The best part? The full refresh logic. If I change a rule today, it automatically cleans up our entire spending history instantly.

What's more? I put AI into good use. While defining some essential categories like Housing, Bills, Grocery is manageable, mapping some random transactions like "SQ *BAKERY 123" to "Groceries" is tedious. And there are so many of them out there. I decided to put AI into good use. I called in Gemini API Basemodel to handle the "Uncategorized" outliers. If the rule-based engine fails, the LLM suggests a category based on the merchant name and description. Results are returned to me for approval or editing before they continue down into the reporting route. Sweet!

### The Bank Statements mess

One of the hurdles was the sheer variety of data. Between us, we have six different accounts across three different banks.If you’ve ever looked at bank statements from different institutions, you know the struggle: one bank calls it "Merchant," another calls it "Description," and a third hides it in a "Memo" field. They all have different CSV layouts and naming conventions. To solve this, I built Account-Specific Transformers. Each account has its own custom function that maps its unique quirks into one single, unified format. This "homogenization" process ensures that no matter where the money comes from, it ends up in a clean, standard table with consistent headers like date, amount, and description.  

### Handling the Nuances: The "Processing" Trap

One challenge I encountered was dealing with transactions that are still "In Processing." If you're like me, I was surprised to learn that many bank statements include these pending items. If I didn't look close enough, I could've easily double or triple counted a transaction, which makes no sense for reporting. My pipeline includes a logic layer to scrub and normalize these transactions, ensuring that only final transactions are included. 

---

## The Outcome: An Intuitive Excel Report

The final product isn't a complex website; it's a clean, formatted Excel file with three key views:

1.  **Monthly Category Spending**: A table showing our budget vs. actual spending, including a "% spending ratio" and a trend indicator (Overspent vs. Underspent).
2.  **Top 5 Non-Recurring Expenditures**: A list of the largest one-off purchases for the month. This helps us quickly identify why a particular month might have felt "expensive."
3.  **Yearly Financial Summary**: A high-level view of our Net Income, Net Expenditures, and Savings. If a year is still in progress, the system dynamically labels it as "YTD."

---

## Future Horizons: What's Next?

This is just the beginning. While a clean Excel sheet is perfect for us right now, I could easily see this moving into a real database or even a private local webpage using Streamlit. Imagine a custom dashboard just for our family—the possibilities are endless!

Sundays are finally back on our calendar! I’m a big believer in automating the boring stuff to make room for the good stuff—if this project sparks any ideas for your own household, don't hesitate to reach out.
