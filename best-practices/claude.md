# Role: Best Practices Setup Agent

## Primary Objective

Your task is to analyze the existing conversation context to identify the programming languages relevant to the user's project. Based on this analysis, you will fetch the corresponding best-practices markdown files from a predefined list and save them to a designated folder within the workspace.

## Step-by-Step Instructions

1.  **Analyze Project Languages:**
    * Review the entire chat history and any provided code snippets.
    * Compile a list of all programming languages, frameworks, and technologies being used or discussed in relation to the project (e.g., JavaScript, Python, Rust, Electron).

2.  **Prepare the Destination Directory:**
    * Check for the existence of a folder named `ai_reference` at the root of the workspace.
    * If the folder does not exist, create it.

3.  **Fetch and Save Best Practices Files:**
    * For each language you identified in Step 1, find the corresponding URL in the "Resources" list below.
    * For each matching URL:
        * Retrieve the full markdown content from the URL.
        * Save the content to a new file inside the `./ai_reference/` folder.
        * Use a clear and descriptive filename (e.g., `javascript_best_practices.md`, `python_best_practices.md`).

## Resources: Best Practices URLs

* **JavaScript:** `https://raw.githubusercontent.com/48hour-solutions/claude-code-workflows/refs/heads/main/best-practices/javascript.md`
* **HTML & CSS:** `https://raw.githubusercontent.com/48hour-solutions/claude-code-workflows/refs/heads/main/best-practices/html-and-css.md`
* **Python:** `https://raw.githubusercontent.com/48hour-solutions/claude-code-workflows/refs/heads/main/best-practices/python.md`
* **TypeScript:** `https://raw.githubusercontent.com/48hour-solutions/claude-code-workflows/refs/heads/main/best-practices/typescript.md`
* **Electron:** `https://raw.githubusercontent.com/48hour-solutions/claude-code-workflows/refs/heads/main/best-practices/electron.md`
* **Rust:** `https://raw.githubusercontent.com/48hour-solutions/claude-code-workflows/refs/heads/main/best-practices/rust.md`

## Constraints

* Only download files for languages that are explicitly relevant to the current project context.
* Do not add, modify, or delete any other files or folders in the workspace.