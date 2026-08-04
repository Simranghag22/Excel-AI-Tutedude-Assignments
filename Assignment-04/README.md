# Assignment 4 — Power Query, Advanced Formulas & Macros

This assignment pushed me well past basic formulas — I got into Power Query, wrote a nested logic formula, worked around a missing Excel feature, and even recorded my first macro. It's the assignment I'm proudest of so far.

## What I Did

### Bringing the data together with Power Query
I started with two separate CSV files — `Employee_info.csv` and `Quarterly_Sales.csv` — that I needed to combine into one clean report. Instead of manually copy-pasting, I used **Data → Get Data → From Text/CSV** to import both into Power Query.

Once inside the Power Query Editor, I had to split the incoming column by delimiter, promote the first row to headers, and fix the data types on each column. Then I used **Merge Queries** to join `Employee_info` and `Quarterly_Sales` together on `EmployeeID`/`EmpID` (a Left Outer join, so I kept every employee even if their sales data was missing), and expanded the merged table to pull in `Q1_Sales` and `Q2_Sales`.

While I was still in Power Query, I added a **custom column** for `Total_Sales`, using `[Q1_Sales] + [Q2_Sales]` — it felt satisfying to get the calculation done right at the source instead of adding it later in the sheet.

### Rating employees with nested logic
Back in Excel, I built a **Rating** column using an `IFS` formula that checks total sales against a few thresholds:
```
=IFS(G2>180000,"Excellent",G2>150000,"Good",G2>100000,"Meets Expectations",TRUE,"Needs Improvement")
```
This was my first time chaining that many conditions together, and `IFS` made it a lot cleaner than nesting a bunch of `IF`s inside each other.

### Working around a missing XLOOKUP
For the bonus calculation, I originally wanted to use `XLOOKUP` to look up each employee's bonus percentage from a Target/Bonus table. My version of desktop Excel doesn't support `XLOOKUP` though, so I ran into a `#NAME?` error when I tried it. Rather than skip the concept, I actually rebuilt the exact formula in Google Sheets (which does support XLOOKUP) to confirm what it *should* look like, screenshotted that, and then implemented the working equivalent in my actual worksheet using `VLOOKUP` instead:
```
=IFERROR(G2*VLOOKUP(G2,$K$2:$L$4,2,TRUE),0)
```
I left notes directly in the sheet explaining this substitution, since I wanted it to be clear I understood the concept even though the tool I was using couldn't run it natively.

I also added a **Total Bonus Payout** cell that sums the Bonus_Amount column using a structured table reference, and threw in **Sparklines** next to each employee's Q1/Q2 sales so I could see their trend at a glance without building a full chart.

### Testing "what-if" scenarios with Goal Seek
I used **Goal Seek** to answer a question I was curious about: what Q2 sales number would someone need to hit a specific bonus target? I set it up to solve for the Q2_Sales value needed to make the Bonus_Amount equal a target figure — a nice hands-on look at how Excel can work backward from a goal.

### Automating formatting with a macro
Last, I recorded a macro called `ApplyReportStyle` using **Developer → Record Macro**, then inserted a rectangle shape, labeled it **"Format Report,"** and assigned the macro to it. Now clicking that one button applies my report formatting automatically — small thing, but it was my first real taste of Excel automation beyond formulas.

*(No official brief PDF was provided for this assignment — inferred title based on the task content above.)*

## 📁 Files
| File | Description |
|------|-------------|
| `Assignment_4.xlsm` | The macro-enabled workbook with the full workflow |
| `Employee_info.csv` | Source data — EmployeeID, FullName, Department |
| `Quarterly_Sales.csv` | Source data — EmpID, Q1_Sales, Q2_Sales |
| `Screenshots/Step-01` → `Step-35` | My step-by-step screenshots of the whole build |
| `Screenshots/Screenshot-of-Performane-Report.png` | The finished Performance_Report sheet |

## 🖼 Preview
**Final Performance Report**
![Performance Report](./Screenshots/Screenshot-of-Performane-Report.png)
