# The Casual Worker's Reality Check

A personal finance assistant built for casual retail and hospitality workers. Standard budgeting apps assume a predictable salary, which breaks down when your roster and hours fluctuate every week.

This tool reads past payslips to calculate your real take-home hourly pay and average weekly earnings. When you enter an item you want to buy, it breaks down what that purchase actually costs you in shift hours, weekly income drain, and everyday trade-offs before passing those numbers to an AI financial coach.

---

## How to Run

1. Open `assignment2.ipynb` in Google Colab.
2. Open the **Secrets (Key)** tab in the left sidebar.
3. Add a new secret named `GEMINIKEY` containing your Google Gemini API key and turn on **Notebook access**.
4. Run all cells (**Runtime > Run all**).
5. Scroll to the bottom cell to use the Gradio interface.

*Note: You don't need your own CSV file to test the app. If you leave the upload box empty and click **Calculate Reality**, it will automatically load the built-in sample dataset.*

---

## Features & Sample Workflow

### Inputs

* **Payslips (CSV)**: Requires columns for `Pay Date`, `Hours Worked`, `Gross Pay`, and `Tax Withheld`.
* **Item Name & Price ($)**: The purchase you are considering.
* **Current Savings ($)**: Your current bank balance.

### Outputs

* **Calculated Take-Home Rate**: Net hourly wage and average weekly net pay.
* **Cost in Floor Hours**: Exact shift hours needed to pay for the item after tax.
* **Income & Savings Impact**: Shows what percentage of your weekly pay the item takes, or how much it drains from your savings if it exceeds your weekly pay.
* **Recommendation Verdict**: Triggers a `✅ SAFE TO PURCHASE` or `🚨 NOT RECOMMENDED TO PURCHASE` status based on shift length and weekly income thresholds.
* **Reality Check Metrics**: Converts the item's cost into everyday equivalents ($5.50 coffees, $80 fuel tanks, $150 grocery runs) if the purchase is flagged as risky.
* **Grounded AI Chat**: An AI coach that answers questions using your specific calculated figures, with 1-click sample prompts provided below the chat.

---

## Example Output

**Uploaded Data (`payslips.csv`):**

* 35 total hours worked, $1,050 gross, $130 tax withheld ($920 net)
* *Calculated Rate*: $26.29/hr net | $460.00/wk average net

**User Input:**

* Item: Sony Headphones ($350)
* Savings: $500

**Calculated Results:**

* **Take-home rate**: $26.29/hr ($460.00/wk average)
* **Cost in floor work**: 13.3 hours
* **Weekly Impact**: Takes 76.1% of average weekly pay ($109.99 left over)
* **Verdict**: 🚨 `NOT RECOMMENDED TO PURCHASE` (exceeds an 8-hour shift equivalent)
* **Reality Check**: Equal to 63 coffees, 4.4 tanks of petrol, or 2.3 weeks of groceries

---

## Limitations & Roadmap

### Known Limitations

* **CSV Format Only**: Requires data in a formatted `.csv` file. Real payslips are usually PDFs, which currently need to be converted manually before uploading.
* **Session Storage Only**: Data is stored in browser memory via `gr.State()`. Refreshing the page resets the session, requiring you to re-run the calculation.
* **API Availability**: The AI coach relies on Google's free-tier REST API, which can occasionally hit peak demand limits (`429` errors). The app catches these errors without crashing the main calculator.
* **Payslip Update:** You would need to update the payslip from within the actual code or the CSV (made by program) that you are uploading to the Gradio each time instead of within the program itself which might not be friendly for non-programmers.
* **Hard Coded Variables**: The program assumes an 8 hour shifts' worth of money is too much, which might not be the same for everyone. The reality check (not AI chatbot) doesn't take into account if something is a necessity or not or if purchasing something will be beneficial in the long run. The comparisons for coffee, petrol and groceries are also hard coded which not be viable in the future. 

### Future Improvements
* **PDF Payslip Parser**: Add OCR/document parsing so users can upload raw PDF payslips or photos directly from their employer.
* **Long-Term Shift Tracking**: Add persistent accounts to track rent and fluctuating income patterns across several months.
* **More Tools:** I would add more tools that are more useful for Casual workers. It would track their income and savings but also take into account recurring expenses like potential rent, groceries, entertainment, and maintenance. After asking app if something is viable to purchase, it will also take into account weekly expenses FIRST and THEN tell you wether the purchase is recommended or not.
