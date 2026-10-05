---
{"dg-publish":true,"permalink":"/publishing-slides/","title":"Publishing Slides","tags":["presentation","slides"],"noteIcon":"default","created":"2026-10-04T19:45:53.687-04:00","updated":"2026-10-04T21:14:01.449-04:00","dg-note-properties":{"title":"Publishing Slides","tags":["presentation","slides"]}}
---

If you love Obsidian as your digital brain, you already know the power of connecting your thoughts. But sometimes a flat wall of text doesn't cut it. When you need to explain an architecture, deliver a tutorial, or summarize key takeaways, **slides just work better**.

By combining **Slides Extended** with the **Digital Garden** plugin and a touch of **VS Code**, you can publish interactive, keyboard-navigable slide decks embedded directly inside your public notes on GitHub Pages.

Here is the complete, update-safe guide to setting up this workflow.

### What is Slides Extended?

[Slides Extended](https://www.google.com/search?q=https://github.com/MSzturc/obsidian-slides-extended) is an Obsidian community plugin that turns Markdown notes into presentations powered by **Reveal.js**.


Place a horizontal rule (`---`) between your sections, and Obsidian renders a full slide presentation with speaker notes, animations, side-by-side layouts, and custom themes.

  

#### Why Embed Slides in Your Digital Garden?

- **Dual-Format Consumption:** Visual learners can click through your slides, while deep-divers can scroll down to read the longform notes on the same page.

- **Interactive Polish:** It elevates your garden beyond static prose, giving your documentation or portfolio a dynamic feel.

- **No Third-Party Hosting:** Keep your presentation assets in your own GitHub repository without relying on external services like Google Slides or SlideShare.


### How the Pieces Fit Together

The Digital Garden plugin publishes Markdown notes to GitHub via the GitHub API, but it doesn't bundle complex multi-file HTML slide folders.


By cloning your site repository in **VS Code**, you can easily drop the exported slides into your repository, configure an update-safe passthrough rule in Eleventy, and push everything via VS Code's Source Control panel.


```
┌────────────────────────────────────────────────────────┐
│                   Your Local Machine                   │
│                                                        │
│  ┌──────────────────────┐    Export HTML               │
│  │    Obsidian Vault    │ ─────────────────┐           │
│  │ (Slide Note & Iframe)│                  │           │
│  └──────────┬───────────┘                  ▼           │
│             │                      ┌────────────────┐  │
│             │ DG Plugin            │ Exported Deck  │  │
│             │ (Note Only)          │ (index + dist) │  │
│             │                      └───────┬────────┘  │
│             │                              │ Drag & drop│
│             │                              ▼ in VS Code │
│             │                   ┌───────────────────┐  │
│             │                   │ Local Repo Clone  │  │
│             │                   │ (Opened in VS Code│  │
│             │                   └─────────┬─────────┘  │
└─────────────┼─────────────────────────────┼────────────┘
              │                             │ VS Code Sync
              │                             │ (git push)
              │                             ▼
┌─────────────┼──────────────────────────────────────────┐
│             ▼        GitHub                            │
│   ┌────────────────────────────────────────────────┐   │
│   │         Digital Garden Repository              │   │
│   │   - Notes: /src/site/notes/                    │   │
│   │   - Slides: /src/site/slides/<deck-name>/      │   │
│   │   - Passthrough: src/helpers/userSetup.js      │   │
│   └───────────────────────┬────────────────────────┘   │
│                           │                            │
│                           ▼ (Eleventy Build)           │
│   ┌────────────────────────────────────────────────┐   │
│   │                 GitHub Pages                   │   │
│   │  https://<username>.github.io/<repo>/slides/...│   │
│   └────────────────────────────────────────────────┘   │
└────────────────────────────────────────────────────────┘
```

### Step 1: Export Your Slides to HTML

1. Open your slide note in Obsidian.

2. Press `Cmd + P` (macOS) or `Ctrl + P` (Windows/Linux) to open the Command Palette.

3. Select:
```
Slides Extended: Export presentation to HTML
```  

4. Choose an export destination (such as your `Downloads` folder).

5. Name the export folder using clean URL slugging with no spaces (e.g., `system-architecture`).

This creates a folder containing `index.html` alongside Reveal.js support folders (`dist/`, `plugin/`, `css/`).

### Step 2: Clone Your Garden Repo in VS Code

Using VS Code handles authentication and folder management without needing terminal commands.


1. Open **VS Code**.

2. Press `Cmd + Shift + P` (or `Ctrl + Shift + P`) to open the Command Palette.

3. Type and run:

```
 Git: Clone
```


4. Paste the URL of your Digital Garden GitHub repository (e.g., `[https://github.com/](https://github.com/)<your-username>/<your-garden-repo>.git`) and press **Enter**.

5. Select a local directory to save the repository, then click **Open** when prompted.

6. If asked, sign in with your GitHub account so VS Code manages your credentials automatically.


### Step 3: Configure Passthrough in `userSetup.js`

By default, the Eleventy engine that powers Digital Garden only copies specific recognized assets into the final production build. Digital Garden provides **`src/helpers/userSetup.js`** as a permanent, update-safe hook to customize the build configuration without getting overwritten by template updates.

1. In VS Code's file explorer, open:

```
src/helpers/userSetup.js
```
    
2. Add `eleventyConfig.addPassthroughCopy("src/site/slides");` inside `userEleventySetup`:

```js
function userMarkdownSetup(md) {
  // The md parameter stands for the markdown-it instance used throughout the site generator.
  // Feel free to add any plugin you want here instead of /.eleventy.js
}
function userEleventySetup(eleventyConfig) {
  // The eleventyConfig parameter stands for the the config instantiated in /.eleventy.js.
  // Feel free to add any plugin you want here instead of /.eleventy.js
 // Copy all slide decks and their assets verbatim into dist/slides/
  eleventyConfig.addPassthroughCopy("src/site/slides");
}
exports.userMarkdownSetup = userMarkdownSetup;
exports.userEleventySetup = userEleventySetup;
```

3. Save the file (`Cmd + S` or `Ctrl + S`).

### Step 4: Copy the Slides and Push via VS Code

1. In VS Code's file tree, create a `slides` folder under `src/site/` if it doesn't already exist:

```
src/site/slides/
```

2. Drag and drop your exported presentation folder (e.g., `system-architecture`) from your file manager directly into `src/site/slides/` inside VS Code.
  
- Make sure the path looks like: `src/site/slides/system-architecture/index.html`.

3. Switch to the **Source Control** tab on the left sidebar (or press `Ctrl + Shift + G`).

4. Type a commit message (e.g., _"Add system-architecture slides and passthrough rule"_).

5. Click **Commit**, then click **Sync Changes** (or **Push**).


Once the GitHub Actions build finishes on your repository, the slides will be live at:

```
https://<your-username>.github.io/<repo-name>/slides/system-architecture/
```

### Step 5: Embed the Slides in an Obsidian Note

Back in Obsidian, create or open the note that will display your slides to garden visitors.

  

1. Add your frontmatter and a responsive `<iframe>`:

```md
---
dg-publish: true
title: "System Architecture Walkthrough"
tags:
  - architecture
  - slides
---

# Architecture Overview

Use the controls below or keyboard arrow keys to navigate the presentation:

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; border-radius: 8px; border: 1px solid #333; margin: 1.5rem 0;">
  <iframe 
   src="/<repo-name>/slides/system-architecture/" 
  style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; border: 0;" 
   allowfullscreen="true">
  </iframe>
</div>

[Open presentation in a new tab](/<repo-name>/slides/system-architecture/)

---

## Accompanying Notes

Here are the detailed references and background context for the slides above...
```

> **Path Note:** If your site is hosted on GitHub Pages under a repository subpath (e.g., `username.github.io/my-garden/`), include your repository name prefix: `/<repo-name>/slides/<deck-name>/`. If you use a custom domain (e.g., `notes.yourdomain.com`), use the root path: `/slides/<deck-name>/`.
> 
>   

### Step 6: Publish Your Note

1. In Obsidian, open the Command Palette (`Cmd + P` or `Ctrl + P`).

2. Run:

```
Digital Garden: Publish Active Note
```

3. Digital Garden commits the Markdown note to GitHub via the API, triggering Eleventy to rebuild the site.

Your readers will now see an interactive, full-screen Reveal.js presentation embedded within your published garden note.
# Slides Extended Example

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; border-radius: 8px; border: 1px solid #333;">
  <iframe 
    src="/slides/Test-Deck/" 
    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; border: 0;" 
    allowfullscreen="true">
  </iframe>
</div>

[Open Presentation Fullscreen](/slides/Test-Deck/)

# Slides.com Example

Slides.com is a visual interface for Reveal.js slides.   

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; border-radius: 8px; border: 1px solid #333;">
  <iframe 
    src="/slides/slides_arrows-lines-27ae9e/" 
    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; border: 0;" 
    allowfullscreen="true">
  </iframe>
</div>

# Marimo Slides Example

Marimo is a python notebook analysis tool that can export interactive Reveal.js slides. These slides can be copied to the local digital garden folder and embedded in notes.  

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; border-radius: 8px; border: 1px solid #333;">
  <iframe 
    src="/slides/marimo/demo_executed.html" 
    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; border: 0;" 
    allowfullscreen="true">
  </iframe>
</div>


# Marimo notebook Example

<div style="position: relative; width: 100%; height: 800px; overflow: hidden; margin: 1em 0;">
  <iframe 
    src="/slides/marimo/notebookDemo.html" 
    style="width: 100%; height: 100%; border: none;"
    allowfullscreen>
  </iframe>
</div>

