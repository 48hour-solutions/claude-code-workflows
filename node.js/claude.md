Setup Instructions for Claude

You are setting up a Node.js JavaScript/TypeScript workflow for Claude Code, follow these steps exactly:

1. Check for an existing package.json, if not found , initialize the barebones for a new node.js project, and set up the package.json
2. Create a folder in root of the project directory named `scripts`
3. Get the script contents from each of these urls:
	- Update me
	- Update me too
	- And me
4. Save each script to the scripts folder you just created (count_lines.ps1, extract_fileoverview.ps1, etc.)
5. Update the package.json to add calls to the scripts, add these exactly without modification (the user is expected to be using Windows)
# Auto Documentation Scripts

"docs:check": "powershell -ExecutionPolicy Bypass -File scripts/check_fileoverview.ps1",
"docs:combine": "powershell -ExecutionPolicy Bypass -File scripts/extract_fileoverview.ps1",
"docs:clean": "rimraf \"fileoverview-collection.json\""

# Other Scripts 
"lint:check-disabled": "powershell -ExecutionPolicy Bypass -File scripts/scan_eslint_disable.ps1",
"linecount": "powershell -ExecutionPolicy Bypass -File scripts/count_lines.ps1"

6. Check for an existing linter config in package.json If found, remove it to prepare to setup eslint.
Setup the latest version of eslint (check with `npm view eslint version`) and set up a barebones configuration.
Then set up the eslint.config.js (or .mjs, depending on the existing code or the user's goal for the project).
Follow guidelines from here:
https://eslint.org/docs/latest/use/getting-started
https://eslint.org/docs/latest/use/configure/

You will need to determine at this step if the primary language for the project is JavaScript or TypeScript, as it will affect the setup process.
If you are unsure from the existing project code, or if it's an empty folder (new project), stop and ask the user for any clarification(s) needed.
NEVER make assumptions , as it could lead to an incorrect setup.

8. Check for an exiting .claude folder, if not found, create it.
9. Copy the contents of each agent file here:
	- Update me
	- Update me too
	- And me
To .claude/agents (create the agents folder if it does not exist)
10. Copy the contents of each command file here:
	- Update me
	- Update me too
	- And me
To .claude/commands (create the commands folder if it does not exist)
11. Check for an existing CLAUDE.md file, if one does exist, you may skip this step.
Based on the project requirements and information , you'll need to generate a comprehensive CLAUDE.md file. If you are unsure of ANY information,
stop and ask the user for any clarification(s) needed.

- Include comprehensive best practice sections for the languages being used (if the user is using JavaScript, TypeScript, or both). Don't generate this from your training data,
search the web for the latest and most up to date information.
- Include STRICT coding safety guidelines for whenever code changes are made, this MUST be done:
	- If this is a TypeScript project, you MUST run type checking after coding changes to ENSURE no errors.
	- ALWAYS run linting after code changes to ensure the BEST code quality
- Remind yourself to ALWAYS use the latest versions for packages, to avoid annoying & time consuming refactors to newer versions in the future.
ALWAYS check the latest versions with `npm view package-name version` , and then get relevant information from the web and your tools
- Include comprehensive information about the mcp tools you are provided (you do NOT need to include 'built-in' tools like edit, find, web search , etc)
	- Using sequential-thinking for complex reasoning and debugging, in tandem with gemini-mcp
	- Using context7 to get library documentation & examples (falling back to web search when needed)
	- Using code-context-provider-mcp to get a full overview of the entire project, specific folders, and more
	