# Retail analysis

**Online Retail** is a transaction-line dataset for a UK online gift shop: **541,909 rows × 8 columns**, covering **1 Dec 2010 – 9 Dec 2011**.

Each row is a **product line on an invoice**, not a full order.

## What’s in it

| Column | What it is |
|---|---|
| `InvoiceNo` | Invoice id (`5…` sales, `C…` cancellations, `A…` adjustments) |
| `StockCode` | Product (or fee) code |
| `Description` | Item name |
| `Quantity` | Units on the line (can be negative) |
| `InvoiceDate` | Timestamp |
| `UnitPrice` | Price per unit in GBP |
| `CustomerID` | Customer number (often missing) |
| `Country` | Ship-to country |

| Scale | Value |
|---|---|
| Unique invoices | 25,900 |
| Unique stock codes | 4,070 |
| Unique customers (where id exists) | 4,372 |
| Countries | 38 |
| UK share of lines | ~91% (495,478) |
| Line revenue (`qty × price`) | ~£9.75m including returns |
| Positive-only line revenue | ~£10.67m |

A typical sold line is small (median qty **3**, median price **£2.08**). Totals are skewed by a few extreme lines.

## Quality in one pass

About **25% of lines have no `CustomerID`**. There are **5,268 exact duplicate rows**, **~10.6k negative quantities** (mostly cancellations), **2,515 zero prices**, and **2 bad-debt adjustment rows** with a large negative price. Descriptions are messy (missing on some zero-price rows, extra spaces, multiple names per stock code). Codes like `POST` are postage, not products.

**Bottom line:** it is useful for sales, returns, and customer analysis after you filter to real sales (`Quantity > 0`, `UnitPrice > 0`), drop duplicates, and treat missing customers as guests.
