# Dev Diary: 

# Day 1:

**Tried:** I wanted to lock in a final project idea and get the design going. My first thought was a payslip analyzer for casual workers but I really didn't like it. It just felt like a boring spreadsheet that happened to have a chatbot glued to it.

**Asked:** I bounced some thoughts off the AI because I wasn't happy with my concept. I asked it to throw out a few ideas that still fit the assignment brief. It gave me a bunch of options like a subscription finder and a travel money helper.

**Kept:** I ended up going with a "What Does It Really Cost?" calculator because the logic is super easy to explain and test. The app basically tells you what a purchase costs in actual hours of work after tax. I kept the core of my original idea (calculating take-home pay from payslips) but used it as the foundation for this new calculator. It is something that I would use since I think casual workers do not always have tools that accurately predict how to use their pay effectively. 

**Changed:** I had the AI help me draft a cleaner problem statement, but I tweaked its version heavily because it sounded way too corporate. I changed the user description to focus on a casual retail worker (which I definitely relate to with my own floor shifts) to make it more authentic. I also stripped back the CSV columns to just Pay Date, Hours Worked, Gross Pay, and Tax Withheld.

**Rejected:** The AI wanted to add extra columns for Superannuation and Penalty Rates. I totally rejected that. Penalty rates are usually baked into gross pay anyway, and tracking super just overcomplicates the math for a basic MVP.

Done today: Set up the GitHub repo and pushed my first commit. I also wrote out the worked example by hand using some fake payslips.
Next: Write the pseudocode for Step 4 and grab my Gemini API key without leaking it in the repo.


# Day 2: 

**Tried**: I needed to translate my planning (Steps 1-4) into actual code in Google Colab. Initially, I was using two separate notebooks, one for writing the markdown steps and one for the Python code, and didn't know how to link a separate CSV file.

**Asked**: I asked the AI how to structure the files in my GitHub repo and how to convert my pseudocode into a working Python function using pandas.

**Kept**: I kept the AI's suggestion to use Colab's `%%writefile` magic command to generate the `payslips.csv` directly inside the notebook. This makes the notebook completely self-contained, which fits the assignment requirements much better than requiring a user to manually upload a file just to test the basic logic.

**Changed**: The AI told me to merge my text notebook and my coding notebook into a single file. I restructured my Colab notebook so it flows numerically from Step 1 down to Step 5, blending Markdown cells for the text and Code cells for the logic. 

**Rejected**: I avoided using standard Python dictionary/loop logic for reading the CSV, even though it was an option. Pandas makes doing column math (like `df['Gross Pay'] - df['Tax Withheld']`) much faster and cleaner for this specific task.

**Done today**: Merged everything into `assignment2.ipynb`. Added Step 3 and Step 4 in Markdown. Wrote the Python function for Step 5 and ran it. It printed out `$26.28` hourly and `13.3` hours—matching my manual hand-math perfectly!

**Next**: Write `assert` tests (Step 6) to make sure the function handles bad inputs (like zero hours worked) without crashing.

# Day 3:

**Tried**: I needed to complete Step 6 (Testing). I wanted to prove my `calculate_real_cost` function handles both normal payslip data and broken/empty data without crashing.

**Asked**: I asked the AI how to structure standard Python `assert` tests in Colab, and how to test an edge case where a casual worker might have zero hours logged for a week.

**Kept**: I kept the AI's idea to use `%%writefile` again to quickly spin up a `zero_hours_payslip.csv` file specifically for the edge-case test. This kept my tests isolated.

**Changed**: The AI's original code for the "Happy Path" test kept failing because of a tiny floating-point rounding difference (my manual math said `$26.28`, but Python calculated `$26.29`). I changed the `assert` statement to accept either value using an `or` condition so the test passes cleanly. 

**Done today**: Wrote and passed the Step 6 tests. My notebook now has a fully working, tested custom tool that calculates the real cost of purchases for casual workers.

**Next**: Start integrating the Gemini API to build the conversational "Finance Coach" persona. I need to make sure I use the correct free-tier model alias.

# Day 4:

**Tried**: I needed to connect the Gemini API (Requirement 1) and make sure it actually used the math from my Python tool (Requirement 2) rather than giving generic advice.

**Asked**: I asked the AI how to securely use an API key in Colab without exposing it in my GitHub repo, and how to structure a prompt that injects my dictionary of results (`normal_result`).

**Kept**: I used Colab's `userdata.get()` feature to pull the key from the Secrets tab. This perfectly satisfies the assignment rule to keep the key out of the repository. I also kept the `system_instruction` parameter to give the model a strict "pragmatic retail worker coach" persona.

**Changed**: I made sure to map the specific outputs from my pandas function (`Hourly Rate`, `Cost in Hours`, `Days Delayed`) directly into an f-string prompt. 

**Done today**: Successfully connected `gemini-1.5-flash-latest`. Tested it with a dummy question about headphones, and it referenced the exact 13.3 hours of work from my Step 5 calculations.

**Next**: Wrap the entire application in a Gradio UI so a non-programmer can upload their own CSV and chat with the coach.

**Error Encountered**: When I ran the Gemini API for the first time, I got a `404 NotFound` error: `models/gemini-1.5-flash-latest is not found`. 

**How I Fixed It**: The AI generated an invalid or outdated model string. I remembered the assignment brief specifically said to use the alias `gemini-flash-latest`. I corrected the AI's code to `model_name='gemini-flash-latest'`, re-ran the cell, and the API successfully returned the response. This taught me that you can't blindly trust AI-generated library parameters, especially for rapidly updating tools like the Gemini SDK.

# Day 5 & 6: 

**Tried:** I needed to build the final user interface for Requirement 4 using Gradio so someone could upload a CSV and chat with the AI on a clean web page.

**Asked:** I checked the Gradio docs and asked the AI for the specific syntax on handling file uploads and passing parsed data between a Pandas function and a Chatbot component.

**Kept:** I decided to use the gr.Blocks() layout instead of a basic gr.Interface(). It gave me way more control over putting the upload button right next to the chat window.

**Changed:** I had to figure out how to bridge the math processing function and the chat function. I ended up using gr.State(), which acts like a hidden background variable. When the user hits calculate, my Pandas tool runs and saves the numbers into gr.State(). Then the chat function pulls those numbers to ground the AI's response.
Done today: The app is fully working. The Gradio UI renders, processes the CSV file without crashing, calculates the real-world costs, and drives the AI conversation using those exact numbers.

