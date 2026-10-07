# Dev Diary: 

# Day 1: 

**Tried**: I wanted to choose a final project idea and start the design. My first idea was a payslip analyser for casual workers, but I didn't like it. It felt like a spreadsheet with a chatbot attached.

**Asked**: I told the AI I wasn't happy with my idea and asked for other ideas that still meet the assignment requirements. It gave me six options, including a subscription finder, a savings goal simulator and a travel money helper.

**Kept**: I chose "What Does It Really Cost?" because it's easy to explain and test. The app tells you what a purchase costs in hours of work after tax. I kept part of my original idea, working out take-home hourly pay from payslips, as the base of the new one.

**Changed**: I asked the AI to rewrite my problem statement and inputs/outputs around the new idea. I tweaked the AI's version because it sounded too corporate—I changed the user description to "A casual retail or hospitality worker (like a uni student)" to make it sound more authentic to my situation, and I simplified the CSV columns to just Pay Date, Hours Worked, Gross Pay, Tax Withheld.

**Rejected**: The AI suggested including "Superannuation" and "Penalty Rates" as separate columns. I rejected this because casual workers sometimes have complex super rules, and penalty rates are already baked into the Gross Pay. Tracking them separately overcomplicates the math for a simple MVP.

**Done today**: Created the GitHub repo and made my first commit. Wrote the worked example by hand with fake payslips.

**Next**: Write the pseudocode (Step 4) and get my Gemini API key, keeping it out of the repo.
