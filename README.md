

# 🧠 AI Project Builder (Trinity + Puter.js)

[ [developer.puter](https://developer.puter.com/blog/arcee-ai-trinity-large-in-puter-js/)
[ [developer.puter](https://developer.puter.com/blog/arcee-ai-trinity-large-in-puter-js/)
[ [arcee](https://www.arcee.ai)
[

AI-powered project generator that turns a short description into a full project tree (HTML/CSS/JS/README, etc.) and writes files directly into your **Puter** cloud using `puter.fs` and **Trinity Large** via `puter.ai.chat`. [developer.puter](https://developer.puter.com/tutorials/free-unlimited-arcee-ai-api/)

> “Describe an app, watch Trinity Large design the structure and create files under your Puter account.”

***

## ✨ Features

- **AI project planning** – uses `arcee-ai/trinity-large-preview:free` through `puter.ai.chat` to generate a JSON plan for files and folders. [developer.puter](https://developer.puter.com/tutorials/free-unlimited-arcee-ai-api/)
- **Automatic file creation** – writes everything into your Puter space with `puter.fs.mkdir` and `puter.fs.write` (`createMissingParents: true`). [developer.puter](https://developer.puter.com/tutorials/free-unlimited-arcee-ai-api/)
- **Mobile-first UI** – single-page, responsive layout tuned for phones and small laptops.
- **Dark, neon-inspired design** – gradients, soft borders, and subtle motion on focus/press.
- **One-click example** – built‑in Todo app prompt to test the flow instantly.

***

## Preview

_Add a screenshot or GIF of the app here (e.g.](https://aiprojectbuilder-549196.puter.site/))_

```markdown
![AI Project Builder screenshot](docs/screenshot.png)
```

***

## 🛠 Tech Stack

- **Frontend:** HTML5, CSS3, vanilla JS (single-page app)
- **Cloud & FS:** Puter.js v2 (`puter.fs.mkdir`, `puter.fs.write`) [developer.puter](https://developer.puter.com/tutorials/free-unlimited-arcee-ai-api/)
- **AI:** Arcee AI Trinity Large Preview (`arcee-ai/trinity-large-preview:free`) via `puter.ai.chat` [developer.puter](https://developer.puter.com/blog/arcee-ai-trinity-large-in-puter-js/)
- **Runtime:** Any modern browser with Puter.js loaded using:
  ```html
  <script src="https://js.puter.com/v2/"></script>
  ``` [developer.puter](https://developer.puter.com/blog/arcee-ai-trinity-large-in-puter-js/)

***

## 🚀 Quick Start

<details>
<summary><strong>Option 1 – Run directly on Puter</strong></summary>

1. Sign in to [Puter](https://puter.com/) in your browser. [developer.puter](https://developer.puter.com/tutorials/free-unlimited-arcee-ai-api/)
2. Create a new file in your workspace, e.g. `apps/ai-project-builder.html`.
3. Paste the full HTML from this repository’s main file into it.
4. Open the file in Puter (Preview or double‑click).
5. Describe a project, set a project root (e.g. `projects/todo-app`), and click **“Ask Trinity & build”**.
6. Open your Puter file manager to inspect and edit the generated project.

</details>

<details>
<summary><strong>Option 2 – Clone and run locally</strong></summary>

1. Clone the repo:
   ```bash
   git clone https://github.com/YOUR-USERNAME/ai-project-builder.git
   cd ai-project-builder
   ```
2. Open `index.html` (or the main `.html` file) in a modern browser.
3. Ensure you are logged into Puter in the same browser profile.
4. Use the UI as above to generate and inspect projects.

</details>

***

## 🔍 How It Works

<details>
<summary><strong>AI planning flow</strong></summary>

- The app constructs a **system prompt** instructing Trinity to output **only JSON** with:
  - `projectRoot`: string
  - `files`: array of `{ path, type: "file" | "dir", content? }`.
- It calls:
  ```js
  const res = await puter.ai.chat(messages, {
    model: 'arcee-ai/trinity-large-preview:free',
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

