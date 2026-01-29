(https://developer.puter.com/blog/arcee-ai-trinity-large-in-puter-js/)

```markdown
# AI Project Builder (Puter.js + Trinity / OpenRouter)

A simple web app that lets you describe a project in natural language and have an AI model generate a project plan and write files into your Puter storage (HTML, CSS, JS, README, etc.).

## Features

- Runs fully in the browser using `https://js.puter.com/v2/`.
- Uses `puter.ai.chat` with your choice of free models (Trinity Large plus OpenRouter `:free` models).
- Creates folders and files via `puter.fs.mkdir` and `puter.fs.write` with `createMissingParents: true`.
- Shows a live log of the AI response and a list of generated files.

## Getting Started

1. Create a new HTML file on Puter (or in any static host).
2. Paste the contents of `index.html` from this repo.
3. Open the file in a browser while logged into your Puter account.

No API keys or backend are required.

## Usage

1. Enter a project root path (for example: `projects/todo-app`).
2. Describe the app you want (tech stack, pages, files, etc.).
3. Pick a model from the dropdown (Trinity Large or any free OpenRouter model).
4. Click **“Ask AI & build”**.
5. Open the generated files in your Puter file manager or editor to tweak and deploy.

## Tech Stack

- HTML, CSS, vanilla JavaScript
- [Puter.js](https://developer.puter.com/) for:
  - `puter.ai.chat` (AI models)
  - `puter.fs` (file and directory operations)
- Arcee AI Trinity Large and free OpenRouter models exposed through Puter

## Notes

- The app asks the model to return a JSON plan describing `projectRoot` and a list of files/directories, then writes them into your Puter space.
- If a model returns invalid JSON, the app will show an error in the log; try a different model or simplify the prompt.
```    model: 'arcee-ai/trinity-large-preview:free',
    stream: false
  });
  ``` [developer.puter](https://developer.puter.com/blog/arcee-ai-trinity-large-in-puter-js/)
- It extracts the first JSON object from the response and parses it into a `plan`.

</details>

<details>
<summary><strong>File creation flow</strong></summary>

- For each `files[]` entry:
  - If `type === "dir"`, it runs:
    ```js
    await puter.fs.mkdir(fullPath, { createMissingParents: true });
    ``` [developer.puter](https://developer.puter.com/tutorials/free-unlimited-arcee-ai-api/)
  - If `type === "file"`, it runs:
    ```js
    await puter.fs.write(fullPath, fileDef.content || '', {
      createMissingParents: true
    });
    ``` [developer.puter](https://developer.puter.com/tutorials/free-unlimited-arcee-ai-api/)
- A small log console shows what was created or if any errors occurred.
- A right-hand list renders created paths and statuses (`created`, `written`, `error`).

</details>

***

## 📦 Example Prompt

Clicking **“Fill example”** loads a Todo app spec like:

```text
Build a simple mobile-friendly Todo web app:

- Use HTML, CSS, and vanilla JS
- One main page with header and a card-style list
- Add tasks, mark complete, delete
- Persist to localStorage
- Use a dark theme with a purple accent
- Put code into: index.html, style.css, app.js

Also include a short README.md explaining how to run it.
```

This becomes a JSON project plan that might yield:

```text
projects/todo-app/
├── index.html
├── style.css
├── app.js
└── README.md
```

***

## 📂 Repository Layout

You can structure this repo as:

```text
/
├── index.html          # AI Project Builder app (the provided HTML file)
├── README.md           # This file
└── docs/
    └── screenshot.png  # Optional UI screenshot
```

***

## 🧪 Limitations & Notes

- Relies on Trinity returning **valid JSON**; minimal extraction logic trims extra text but malformed JSON will surface as an error.
- Uses the **free preview** model `arcee-ai/trinity-large-preview:free`, which may change behavior as Arcee updates the checkpoint. [arcee](https://www.arcee.ai/blog/trinity-large)
- Files are written **inside the user’s Puter account** and scoped to the app sandbox. [developer.puter](https://developer.puter.com/tutorials/free-unlimited-arcee-ai-api/)

***

## 🧾 License

Released under the **MIT License**. See `LICENSE` for details.

