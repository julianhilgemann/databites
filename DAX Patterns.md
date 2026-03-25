
### 1. The "Retail Standard": Year-over-Year (YoY) Growth

*   **The Question:** "How are we performing compared to the same time last year?"
*   **Why HSE asks:** Every Monday morning meeting starts with this number.
*   **The Logic:**
    1.  Calculate Current Sales.
    2.  Calculate Last Year's Sales (shift the dates back 1 year).
    3.  Find the difference and divide.

**The Code:**
```dax
YoY Growth % = 
VAR CurrentSales = [Total Sales]
VAR LastYearSales = CALCULATE( [Total Sales], SAMEPERIODLASTYEAR( 'Dim_Date'[Date] ) )
RETURN
    DIVIDE( CurrentSales - LastYearSales, LastYearSales, 0 )
```
*   **Key Concept:** [[Time Intelligence]] functions (`SAMEPERIODLASTYEAR`) rely on a proper Date Table marked as a "Date Table."

---

### 2. The "Product King": Pareto Analysis (The 80/20 Rule)

*   **The Question:** "Which top products account for 80% of our revenue?"
*   **Why HSE asks:** They have limited Live TV minutes. They only want to show products that sell.
*   **The Logic:**
    1.  Get Total Sales for *all* products (ignore filters).
    2.  Calculate the cumulative (running) total of sales up to the current product.
    3.  Divide Cumulative by Total to get the %.

**The Code (Simplified for Whiteboard):**
```dax
Cumulative Sales % = 
VAR TotalSalesAll = CALCULATE( [Total Sales], ALL( 'Dim_Product' ) )
VAR CurrentRunningTotal = 
    CALCULATE( 
        [Total Sales], 
        FILTER( 
            ALL( 'Dim_Product' ), 
            [Total Sales] >= [Total Sales] // Rank logic: Sum everyone bigger than me
        )
    )
RETURN
    DIVIDE( CurrentRunningTotal, TotalSalesAll )
```
*   **Key Concept:** Using `ALL` to break out of the filter [[Context|context]] to see the "Whole," then using `FILTER` to rebuild the running total.

---

### 3. The "Gen Z Hunter": New vs. Returning Customers
*   **The Question:** "How many of today's buyers are brand new?"
*   **Why HSE asks:** They are pivotting to social commerce; they need to know if the App is attracting *new* people or just the same old TV audience.
*   **The Logic:**
    1.  Look at the customers who bought in the current selected period.
    2.  Check their "First Order Date."
    3.  If "First Order Date" is inside the current period $\rightarrow$ New. Otherwise $\rightarrow$ Returning.

**The Code:**
```dax
New Customer Count = 
COUNTROWS(
    FILTER(
        VALUES( 'Fact_Sales'[Customer_ID] ), // Get list of customers who bought now
        CALCULATE( MIN( 'Fact_Sales'[OrderDate] ) ) = MIN( 'Dim_Date'[Date] ) 
        // Logic: Is their first ever purchase date EQUAL to the current visible date?
    )
)
```
*   **Key Concept:** Virtual [[Tables]] (`VALUES`) and iterating to check history.

---

### 4. The "Live TV" Metric: Moving Average (Rolling Average)
*   **The Question:** "The daily sales chart is too spiky. Can you smooth it out to show the trend?"
*   **Why HSE asks:** Live TV sales spike when a show is on and drop to zero after. They need a 7-day or 30-day trend to see real health.
*   **The Logic:**
    1.  Identify the last date selected.
    2.  Grab a range of dates (e.g., last 30 days) relative to that date.
    3.  Average the sales over that range.

**The Code:**
```dax
Rolling 30 Day Avg = 
CALCULATE(
    AVERAGEX( 'Dim_Date', [Total Sales] ), // Average the daily sales
    DATESINPERIOD( 
        'Dim_Date'[Date], 
        MAX( 'Dim_Date'[Date] ), // Starting from the last visible date
        -30,                     // Go back 30 intervals
        DAY                      // Interval is Day
    )
)
```
*   **Key Concept:** `DATESINPERIOD` creates a dynamic window of time.

---

### 5. The "Inventory" Trick: Semi-Additive Measures
*   **The Question:** "What was our Inventory value at the end of the month?"
*   **Why HSE asks:** You cannot sum Inventory over time. (If I have 10 units on Monday and 10 units on Tuesday, I don't have 20 units total. I still have 10).
*   **The Logic:**
    1.  You can sum inventory across *Stores* or *Products*.
    2.  But across *Time*, you must take the **Last Available Value**.

**The Code:**
```dax
Closing Inventory Value = 
CALCULATE(
    SUM( 'Fact_Inventory'[Quantity] ),
    LASTDATE( 'Dim_Date'[Date] ) // Force the filter to only the very last day
)
```
*   **Key Concept:** This proves you understand data modeling physics. "Inventory is semi-additive; it sums across space but not across time."

---

### How to use this list to lower anxiety:

You don't need to memorize the code character-for-character. Just memorize the **Logic Steps**.

If they ask: *"How would you calculate New Customer Acquisition?"*

**You say:**
*"I would iterate through the list of customers active in the current period using `VALUES`, and then use `CALCULATE` to check if their `MIN(OrderDate)` (their first purchase) falls within the current date [[Context|context]]. If it does, they are New."*

That sentence alone gets you the job. Code can be Googled. Logic cannot.