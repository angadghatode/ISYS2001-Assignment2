# Dev Diary: 


# Day 1: 

**Tried**: I wanted to choose a final project idea and start the design. My first idea was a payslip analyser for casual workers, but I didn't like it. It felt like a spreadsheet with a chatbot attached.

**Asked**: I told the AI I wasn't happy with my idea and asked for other ideas that still meet the assignment requirements. It gave me six options, including a subscription finder, a savings goal simulator and a travel money helper.

**Kept**: I chose "What Does It Really Cost?" because it's easy to explain and test. The app tells you what a purchase costs in hours of work after tax. I kept part of my original idea, working out take-home hourly pay from payslips, as the base of the new one.

**Changed**: I asked the AI to rewrite my problem statement and inputs/outputs around the new idea. I tweaked the AI's version because it sounded too corporate—I changed the user description to "A casual retail or hospitality worker (like a uni student)" to make it sound more authentic to my situation, and I simplified the CSV columns to just Pay Date, Hours Worked, Gross Pay, Tax Withheld.

**Rejected**: The AI suggested including "Superannuation" and "Penalty Rates" as separate columns. I rejected this because casual workers sometimes have complex super rules, and penalty rates are already baked into the Gross Pay. Tracking them separately overcomplicates the math for a simple MVP.

**Done today**: Created the GitHub repo and made my first commit. Wrote the worked example by hand with fake payslips.

**Next**: Write the pseudocode (Step 4) and get my Gemini API key, keeping it out of the repo.


# Day 2: 

**Tried**: I needed to translate my planning (Steps 1-4) into actual code in Google Colab. Initially, I was using two separate notebooks, one for writing the markdown steps and one for the Python code, and didn't know how to link a separate CSV file.

**Asked**: I asked the AI how to structure the files in my GitHub repo and how to convert my pseudocode into a working Python function using pandas.

**Kept**: I kept the AI's suggestion to use Colab's `%%writefile` magic command to generate the `payslips.csv` directly inside the notebook. This makes the notebook completely self-contained, which fits the assignment requirements much better than requiring a user to manually upload a file just to test the basic logic.

**Changed**: The AI told me to merge my text notebook and my coding notebook into a single file. I restructured my Colab notebook so it flows numerically from Step 1 down to Step 5, blending Markdown cells for the text and Code cells for the logic. 

**Rejected**: I avoided using standard Python dictionary/loop logic for reading the CSV, even though it was an option. Pandas makes doing column math (like `df['Gross Pay'] - df['Tax Withheld']`) much faster and cleaner for this specific task.

**Done today**: Merged everything into `assignment2.ipynb`. Added Step 3 and Step 4 in Markdown. Wrote the Python function for Step 5 and ran it. It printed out `$26.28` hourly and `13.3` hours—matching my manual hand-math perfectly!

**Next**: Write `assert` tests (Step 6) to make sure the function handles bad inputs (like zero hours worked) without crashing.
