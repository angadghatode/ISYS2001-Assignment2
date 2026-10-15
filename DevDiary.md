# Dev Diary:

# Day 1:

**Tried**: I wanted to lock in a final project idea and get the design going. My first thought was a payslip analyzer for casual workers but I really didn't like it. It just felt like a boring spreadsheet that happened to have a chatbot glued to it.

**Asked**: I bounced some thoughts off the AI because I wasn't happy with my concept. I asked it to throw out a few ideas that still fit the assignment brief. It gave me a bunch of options like a subscription finder and a travel money helper.

**Kept**: I ended up going with "What Does It Really Cost?" because the logic is super easy to explain and test. The app basically tells you what a purchase costs in actual hours of work after tax. I kept the core of my original idea (calculating take-home pay from payslips) but used it as the foundation for this new calculator.

**Changed**: I had the AI help me draft a cleaner problem statement, but I tweaked its version heavily because it sounded way too corporate. I changed the user description to focus on a casual retail worker (which I definitely relate to with my own floor shifts) to make it more authentic. I also stripped back the CSV columns to just Pay Date, Hours Worked, Gross Pay, and Tax Withheld.

**Rejected**: The AI wanted to add extra columns for Superannuation and Penalty Rates. I totally rejected that. Penalty rates are usually baked into gross pay anyway, and tracking super just overcomplicates the math for a basic MVP.

**Done today**: Set up the GitHub repo and pushed my first commit. I also wrote out the worked example by hand using some fake payslips.

**Next**: Write the pseudocode for Step 4 and grab my Gemini API key without leaking it in the repo.

# Day 2:

**Tried**: I needed to turn my planning from Steps 1-4 into actual Python code in Google Colab. At first I was writing markdown in one notebook and code in another, and I was pretty stuck on how to link a separate CSV file to test my math.

**Asked**: I asked the AI for advice on the best way to structure the files for GitHub and the cleanest pandas syntax to translate my pseudocode.

**Kept**: I took the AI's tip to use Colab's `%%writefile` command to generate `payslips.csv` right inside the notebook. It makes the whole file self-contained so the marker doesn't have to manually upload stuff just to test the basic logic.

**Changed**: The AI suggested I merge my text and code notebooks into one. I reorganized my Colab file so it flows straight from Step 1 down to Step 5, mixing Markdown cells for my text with Code cells for the actual logic.

**Rejected**: The AI offered standard Python dictionary and loop logic to read the CSV. I ignored that and used Pandas instead. Doing column math in Pandas is way faster and cleaner for this kind of data.

**Done today**: Merged everything into `assignment2.ipynb`. Added Steps 3 and 4 in Markdown. I wrote the Python function for Step 5 and when I ran it, it spit out $26.28 hourly and 13.3 hours, which matched my hand-written math perfectly.

**Next**: Write the `assert` tests (Step 6) to make sure the code doesn't crash if someone uploads bad data.

# Day 3:

**Tried**: I wanted to knock out Step 6 for testing. I needed to prove my `calculate_real_cost` function could handle normal payslip data and completely empty data without breaking.

**Asked**: I asked the AI for a quick refresher on structuring standard Python `assert` tests in Colab and how to best simulate an edge case for a casual worker with zero hours.

**Kept**: I kept the idea to use `%%writefile` again to quickly make a `zero_hours_payslip.csv` file. It was a smart way to keep the edge-case test totally isolated from the main data.

**Changed**: My code for the happy path test kept failing because of a tiny rounding difference. My manual math was $26.28 but Python kept calculating $26.29. I updated the `assert` statement with an `or` condition so it accepts either value and passes cleanly.

**Done today**: Wrote and passed the Step 6 tests. The notebook now has a fully working and tested custom tool.

**Next**: Start hooking up the Gemini API to build the "Finance Coach" persona. I need to double check I'm using the right free-tier model name.

# Day 4:

**Tried**: I needed to tackle Requirement 1 (connecting the Gemini API) and Requirement 2 (making sure the AI actually uses my Python math instead of giving generic advice).

**Asked**: I asked the AI for the syntax to securely load an API key in Colab without exposing it, and how to format a prompt that feeds my `normal_result` dictionary into the chat.

**Kept**: I used Colab's `userdata.get()` feature to pull the key from the Secrets tab to meet the security requirement. I also kept the `system_instruction` parameter to force the model into a strict "pragmatic retail worker coach" persona.

**Changed**: I set up an f-string prompt to directly map the outputs from my pandas function (like the Hourly Rate and Cost in Hours) straight into the text the AI reads.

**Done today**: Connected `gemini-1.5-flash-latest` successfully. I tested it with a dummy question about headphones and it accurately referenced the 13.3 hours of work from my Step 5 code.

**Next**: Wrap everything in a Gradio UI so users can upload their own CSVs and chat with the coach.

**Error Encountered**: When I ran the Gemini API the first time, it threw a `404 NotFound` error saying `models/gemini-1.5-flash-latest is not found`.

**How I Fixed It**: The AI had generated an outdated model string. I went back to the assignment brief which specifically said to use the alias `gemini-flash-latest`. I fixed the string in my code, re-ran the cell, and the API worked perfectly. Good lesson in not blindly trusting AI with library parameters.

# Day 5 & 6: 

**Tried**: I needed to build the final user interface for Requirement 4 using Gradio so someone could upload a CSV and chat with the AI on a clean web page.

**Asked**: I checked the Gradio docs and asked the AI for the specific syntax on handling file uploads and passing parsed data between a Pandas function and a Chatbot component.

**Kept**: I decided to use the `gr.Blocks()` layout instead of a basic `gr.Interface()`. It gave me way more control over putting the upload button right next to the chat window.

**Changed**: I had to figure out how to bridge the math processing function and the chat function. I ended up using `gr.State()`, which acts like a hidden background variable. When the user hits calculate, my Pandas tool runs and saves the numbers into `gr.State()`. Then the chat function pulls those numbers to ground the AI's response.

**Done today**: The app is fully working. The Gradio UI renders, processes the CSV file without crashing, calculates the real-world costs, and drives the AI conversation using those exact numbers.

