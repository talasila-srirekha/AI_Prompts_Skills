🚀 How to Use It
----------------------
1. Run your Playwright test suite.

2. If tests fail and generate a playwright-report/results.xml file, open the GitHub Copilot Chat in your IDE.

3. Ensure Copilot is in Agent Mode (if applicable to your IDE version).

4. Prompt Copilot with any of the following natural language triggers:

	"Why did my test suite fail?"

	"Analyze my test failures."

	"Summarize the latest Playwright run."

5. Copilot will automatically recognize the intent, trigger the skill, and begin analyzing your code.

⚙️ How It Works Under the Hood
---------------------------------
This skill leverages Copilot's progressive loading and autonomous tool execution:

1. Intent Matching (The YAML Index): Copilot silently reads the YAML frontmatter at the top of SKILL.md. When your prompt matches the skill's description, it loads the full workflow into active memory.

2. Autonomous Tool Execution: The agent uses its internal read_file tool to open and parse the playwright-report/results.xml file, specifically hunting for <failure> tags.

3. Smart Routing (The Scale Rule):

   - 1 to 5 Failures: The agent replies instantly in the chat window with an ultra-concise, token-optimized summary and exact code fixes.
   - 6+ Failures: To save chat space and memory, the agent silently reads file-template.md and generates a comprehensive playwright-failure-summary.md file in your root directory, grouping errors by root cause.

4. Code Investigation: For every failure, the agent opens the specific .spec.ts file referenced in the error trace to read the surrounding code context before proposing a 1-to-2 line code fix.

🔋 Token Optimization
---------------------------
This skill is strictly configured to minimize LLM token consumption. It achieves this by:

1. Limiting diagnostic explanations to a 1 to 2 sentences.

2. Forcing the agent to output only the modified lines of code, rather than rewriting entire test blocks.
