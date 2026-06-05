---
name: playwright-failure-analysis
description: Locates, parses, and diagnoses Playwright test failures from the JUnit XML report. Use when the user asks why tests failed, debugging CI runs, or summarizing Playwright errors.
---

# Playwright Failure Analysis & Resolution

**Objective:** Automatically locate, parse, and diagnose Playwright test failures by reading the JUnit XML report using your standard file access tools, and efficiently report the results based on the volume of failures.

**Trigger Conditions:**
- The user asks "Why did the test suite fail?", "Analyze my test failures", or "Summarize the latest test run."
- You run a test script using your terminal tool and receive a non-zero exit code.

## Step-by-Step Execution Workflow

1. **Locate the Report:** 
   Use your file reading tool to open the Playwright JUnit report. The default path is `playwright-report/results.xml`. If it is not there, check `playwright.config.ts` for the `junit` output file path.

2. **Extract Failure Data:** 
   Read the contents of the XML file. Ignore all standard `<testcase>` nodes that do not have child nodes (these are passing tests). Focus strictly on `<testcase>` nodes that contain a `<failure>` tag. 
   - Extract the `name` attribute (the test name).
   - Extract the `file` attribute (the spec file path).
   - Extract the text content inside the `<failure>` tag (the stack trace and error message).

3. **Analyze and Synthesize (Scale Rule):** 
   Count the total number of failures. Do NOT dump the raw XML back to the user.
   
   - **If there are 5 or fewer failures:** 
     Print the detailed analysis, root causes, and proposed code fixes directly in the chat window.
     
   - **If there are MORE than 5 failures:** 
     Do NOT print the detailed analysis in the chat. Instead:
     1. Use your file creation/writing tool to create a well-formatted markdown file named `playwright-failure-summary.md` in the root directory.
     2. In this file, group the errors by root cause (e.g., "Timeout Errors", "Assertion Failures on Login", "Network Issues").
     3. For each group in the file, list the affected tests/files and propose the overarching fix.
     4. In the chat window, ONLY output a brief executive summary (e.g., *"Found 50 failures across 3 main categories. I have generated a detailed report for you at `playwright-failure-summary.md`."*).

4. **Investigate and Propose a Fix:** 
   Look at the specific file name and line number provided in the `<failure>` stack trace. Use your file reading tool to open that specific `.spec.ts` file. Inspect the surrounding code and include the exact line edit required to fix the test in your final output (whether in chat or in the generated markdown file).
