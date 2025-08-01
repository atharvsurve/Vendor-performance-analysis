Inventory & Vendor Data Overview

This dataset tracks product inventory, purchases, sales, and vendor relationships for the year 2024. Below is a summary of the key tables and their purposes:
begin_inventory & end_inventory
Purpose: Capture the inventory state at the start (Jan 1) and end (Dec 31) of 2024.
Key Columns: InventoryId, Store, City, Brand, onHand, Price
Note: Excluded from vendor-specific analysis.

purchases
Purpose: Detailed log of all purchase transactions from vendors.
Key Columns:
Product Info: InventoryId, Brand, Description
Vendor Info: VendorNumber, VendorName
Transaction Info: PONumber, PODate, ReceivingDate, PayDate
Cost Info: PurchasePrice, Quantity, Dollars

purchase_prices
Purpose: Master list linking vendors to their standard product prices.
Key Columns: Brand, Description, Price (retail), PurchasePrice (cost), VendorNumber, VendorName

sales
Purpose: Records every product sale to customers.
Key Columns:
Product Info: InventoryId, Brand, Description
Sales Metrics: SalesQuantity, SalesDollars, SalesPrice, SalesDate
Vendor Attribution: VendorNo, VendorName

vendor_invoice
Purpose: Summarizes vendor invoices per purchase order.
Key Columns: VendorNumber, VendorName, InvoiceDate, PONumber, PayDate, Quantity, Dollars, Freight (shipping cost)

Objective
To generate a summary table that answers:
How much was purchased from each vendor?
How much was sold, and at what margin?
What freight costs were incurred?
What is the overall profitability?

Problem
The original tables are too large for repeated joins:
sales: ~12.8 million rows
purchases: ~2.3 million rows

Direct joins are inefficient and slow for analysis.

Approach
An ETL-style strategy is used:
Filter → Aggregate → Join → Analyze

Steps
Filter and Explore Relationships
By isolating one vendor (VendorNumber = 4466), identified key relationships:
VendorNumber is the common key
Brand identifies products uniquely
Freight data exists only in vendor_invoice
Sales data links via VendorNo and Brand

Build Aggregated Tables
Three separate summaries are created:
Freight Summary: Total freight cost per vendor
Purchase Summary: Total quantity and spend per product (joined with retail pricing)
Sales Summary: Total quantity sold, revenue, and taxes per product and vendor

Join into a Final Summary Table
The three summaries are merged using VendorNumber and Brand, resulting in a single table (vendor_sales_summary) that holds:

Purchase and sales quantities and amounts
Freight cost
Profit and margin metrics

Outcome
The final summary enables efficient analysis and visualization of:
Vendor-level profitability
Sales-to-purchase ratios
High-cost, low-return products

This structured dataset avoids repeated large joins and supports further reporting and decision-making.

The newly created vendor_sales_summary DataFrame is cleaned and enhanced.

Feature Engineering: New, more insightful columns are created to help with analysis:
GrossProfit: The difference between sales revenue and purchase cost.
ProfitMargin: The gross profit as a percentage of sales revenue.
StockTurnover: The ratio of units sold to units purchased.
SalesToPurchaseRatio: The ratio of sales dollars to purchase dollars.