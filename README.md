# Retail analysis

**Online Retail** is a transaction-line dataset for a UK online gift shop: **541,909 rows × 8 columns**, covering **1 Dec 2010 – 9 Dec 2011**.

Each row is a **product line on an invoice**, not a full order.

## What’s in it


| Column        | What it is                                                    |
| ------------- | ------------------------------------------------------------- |
| `InvoiceNo`   | Invoice id (`5…` sales, `C…` cancellations, `A…` adjustments) |
| `StockCode`   | Product (or fee) code                                         |
| `Description` | Item name                                                     |
| `Quantity`    | Units on the line (can be negative)                           |
| `InvoiceDate` | Timestamp                                                     |
| `UnitPrice`   | Price per unit in GBP                                         |
| `CustomerID`  | Customer number (often missing)                               |
| `Country`     | Ship-to country                                               |



| Scale                              | Value                     |
| ---------------------------------- | ------------------------- |
| Unique invoices                    | 25,900                    |
| Unique stock codes                 | 4,070                     |
| Unique customers (where id exists) | 4,372                     |
| Countries                          | 38                        |
| UK share of lines                  | ~91% (495,478)            |
| Line revenue (`qty × price`)       | ~£9.75m including returns |
| Positive-only line revenue         | ~£10.67m                  |


A typical sold line is small (median qty **3**, median price **£2.08**). Totals are skewed by a few extreme lines.

## Quality in one pass

About **25% of lines have no** `CustomerID`. There are **5,268 exact duplicate rows**, **~10.6k negative quantities** (mostly cancellations), **2,515 zero prices**, and **2 bad-debt adjustment rows** with a large negative price. Descriptions are messy (missing on some zero-price rows, extra spaces, multiple names per stock code). Codes like `POST` are postage, not products.

**Bottom line:** it is useful for sales, returns, and customer analysis after you filter to real sales (`Quantity > 0`, `UnitPrice > 0`), drop duplicates, and treat missing customers as guests.

**This is a UK gift retailer with a Christmas ramp, a few wholesale-like export markets, and a catalog where volume stars and revenue stars are not the same products.** Figures are from the cleaned sales set (~£8.89m, 18,532 invoices, known customers only).

### Sales over time

Revenue is not flat. It dips in **Feb–Apr 2011**, then climbs into autumn and peaks in **Nov (£1.16m, 2,657 orders)**. **Sep–Nov** is the real growth block (revenue jumps from ~£0.64m in Aug to £0.95m / £1.04m / £1.16m).

**Dec 2011 looks weak (£0.52m)** only because the file **ends 9 Dec** — compare to full **Dec 2010 (£0.57m)**. Do not treat December 2011 as a collapse.

Order count and revenue move together; AOV is jumpy (Jan £576, Nov £435, partial Dec £665), so the story is **more orders into Q4**, not a steady rise in basket size.

### Geography

**UK is ~82% of revenue** (£7.29m, 16,646 invoices, AOV **£438**). The other **36 countries** are a small slice of orders but a **much higher AOV**:


| Country        | Revenue | Share | Invoices | AOV        |
| -------------- | ------- | ----- | -------- | ---------- |
| United Kingdom | £7.29m  | 82.0% | 16,646   | £438       |
| Netherlands    | £0.29m  | 3.2%  | 94       | **£3,037** |
| EIRE           | £0.27m  | 3.0%  | 260      | £1,020     |
| Germany        | £0.23m  | 2.6%  | 457      | £500       |
| France         | £0.21m  | 2.4%  | 389      | £537       |
| Australia      | £0.14m  | 1.6%  | 57       | **£2,429** |


Netherlands, Australia, Singapore, Japan look like **wholesale / bulk** (few invoices, huge baskets). Germany/France look more like **repeat retail**. Growth abroad is about **account quality**, not matching UK order volume.

### Products

Top **15 SKUs are only ~12% of revenue** — the catalog is a long tail, not a few heroes.

**Revenue leaders** mix true bestsellers with one-off bulk and fees:

- **PAPER CRAFT , LITTLE BIRDIE** — £168k from **one invoice / 80,995 units**. Treat as an outlier, not a core range SKU.
- **REGENCY CAKESTAND 3 TIER** — £142k across **~1,700 invoices** (real hero).
- **WHITE HANGING HEART T-LIGHT HOLDER** / **JUMBO BAG RED RETROSPOT** — high revenue *and* high repeat orders.
- **POSTAGE** (£78k) and **Manual** (£53k) are **not merchandise**.

**Quantity leaders** include cheap fillers (**WW2 gliders**, cake cases, tissues) that move units but little money. Stock and promo should split **margin heroes** vs **traffic fillers**.

### Average order value

Mean AOV is **£480**, median **£303** — a typical order is closer to £300; the mean is pulled up by export/wholesale invoices. Average **~278 units per invoice** also reflects gift packs and bulk lines, not 278 unique products.

Monthly AOV stays roughly **£400–£575** except the incomplete December. Country AOV is the bigger split: **UK ~£440 vs Netherlands/Australia/Singapore ~£2.4k–£3.0k**.

