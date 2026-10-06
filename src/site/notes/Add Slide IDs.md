---
{"dg-publish":true,"dg-path":"Obsidian/SlidesExtended","permalink":"/obsidian/slides-extended/","title":"Add Slide IDs","hide":"true","pinned":true,"noteIcon":"2","created":"2026-10-05T19:34:14.064-04:00","updated":"2026-10-05T20:03:20.093-04:00","dg-note-properties":{"title":"Add Slide IDs","tags":null}}
---


Markdown presentations are easy to write, but linking them to external tools often presents a challenge.

## The Problem: The Fragile Slide Index

When building interactive slide decks, controlling live-stream scenes in OBS, or syncing slides with external visuals and lighting, you need to know which slide is currently active.

Most slide decks track position by index: **Slide 0, Slide 1, Slide 2**, and so on.

Tracking by index becomes problematic the moment you edit your presentation:
  
- You insert an introductory slide at the beginning. 
- Every downstream slide index increments by one.
- Slide 14 suddenly becomes Slide 15.
- Any automation triggered by slide index targets the wrong content.

```text
Slide 0: Title
Slide 1: Intro (Newly inserted!)
Slide 2: Main Agenda  <-- Was Slide 1! External triggers break here.
```

## The Solution: Persistent Slide IDs (`data-id`)

Instead of relying on unstable slide positions, you can assign each slide its own unique, permanent identifier: a **UUID** (Universally Unique Identifier).

Slides Extended supports Reveal.js slide attributes via HTML comments:


```md
<!-- slide data-id="e10adc3949ba59abbe56e057f20f883e" -->
# Introduction to Dynamic Pipelines
Content goes here...
```

### Why This Matters for Downstream Automation

When slides carry a permanent, dashless `data-id`:

1. **Rock-Solid Event Hooking:** You can hook into Reveal's `slidechanged` event in a custom template or browser window. Instead of checking `event.indexh`, inspect `event.currentSlide.getAttribute('data-id')`.

2. **OBS Scene Switching:** When navigating to a specific demo slide, a small local script can monitor the active slide ID and trigger an OBS scene switch (such as switching from a talking head to full-screen code). Reordering slides won't break the trigger.

3. **Interactive Visuals & Hardware Integration:** If you route presentations through creative visual tools (like cables.gl or WebGL viewports) or trigger DMX lights and MIDI cues, slide IDs act as deterministic event addresses.

4. **Analytics & Progress Auditing:** If you track audience engagement or export slide performance data, persistent IDs ensure historical metrics remain tied to the correct slide even after deck refactors.

Manually generating and typing UUIDs for 40 slides is tedious. Here is how to automate the entire process with a single keystroke using either **QuickAdd** or **Templater**.

## Option 1: QuickAdd Automation (Recommended)

QuickAdd lets you run standalone JavaScript files as custom commands.

### Step 1: Save the User Script

1. Inside your Obsidian vault, create a folder named `Scripts` (if you don't already have one).

2. Inside that folder, create a file named `add-slide-uuids.js`.

3. Paste the following JavaScript into `add-slide-uuids.js`:


```js
module.exports = async (params) => {
  const { app, obsidian } = params;
  const { MarkdownView, Notice } = obsidian;

  const activeView = app.workspace.getActiveViewOfType(MarkdownView);
  
  if (!activeView) {
   new Notice("No active markdown editor found.");
   return;
  }

  const editor = activeView.editor;
  const content = editor.getValue();

  // Helper to generate a clean, 32-character dashless UUID
  const getCleanUUID = () => crypto.randomUUID().replace(/-/g, "");

  // Separate YAML frontmatter if present
  let body = content;
  let frontmatter = "";
  if (content.startsWith("---")) {
   const endFmIndex = content.indexOf("\n---", 3);
   if (endFmIndex !== -1) {
   frontmatter = content.slice(0, endFmIndex + 4);
   body = content.slice(endFmIndex + 4);
   }
  }

  // Regex matching Slides Extended horizontal (---) and vertical (--) delimiters
  const slideRegex = /(^|\n)(---|\-\-)(?=\s*(\n|$))/g;

  let lastIndex = 0;
  let result = "";
  let match;

  // Tag initial slide if not already tagged
  const initialSlideHasId = /^\s*<!--\s*slide\s+[^>]*data-id=/i.test(body.trimStart());
  if (!initialSlideHasId && body.trim().length > 0) {
   result += `<!-- slide data-id="${getCleanUUID()}" -->\n`;
  }

  while ((match = slideRegex.exec(body)) !== null) {
   const chunk = body.slice(lastIndex, match.index + match[0].length);
   result += chunk;
   lastIndex = match.index + match[0].length;

   // Check if upcoming slide already has a data-id
   const nextSnippet = body.slice(lastIndex, lastIndex + 120);
   if (!/^\s*<!--\s*slide\s+[^>]*data-id=/i.test(nextSnippet)) {
   result += `\n<!-- slide data-id="${getCleanUUID()}" -->`;
   }
  }

  result += body.slice(lastIndex);

  editor.setValue(frontmatter + result);
  new Notice("Slide data-ids injected.");
};
```

### Step 2: Register in QuickAdd

1. Open **Settings → Community Plugins** and ensure **QuickAdd** is installed and enabled.

2. Go to **Settings → QuickAdd**.

3. Scroll to **Manage Macros** and click the button.

4. Name your macro (e.g., `Add Slide UUIDs`) and click **Add macro**.

5. Click **Configure** next to the new macro:
- In the **User Scripts** dropdown at the bottom, select `add-slide-uuids.js`.
- Click **Add**.

5. Return to the main QuickAdd settings page:
    
      
    - In the input field, name your choice `Add Slide UUIDs`.

    - Set the type dropdown to **Macro**.
        
          
        
    - Click **Add Choice**.
        
          
        
    - Click the gear icon on the choice row and link it to your `Add Slide UUIDs` macro.
        
          
        
    - Click the **Lightning Bolt (⚡)** icon next to it.
        
          
        

The command is now registered directly in your Command Palette.

  

## Option 2: Templater Automation

If you already manage your workflow with **Templater**, you can use an in-editor execution template instead.

  

### Step 1: Create the Template Note

1. Inside your vault's `Templates` folder, create a new note named `Add Slide UUIDs.md`.
    
      
    
2. Paste the following template code:
    
      
    

Markdown

```
<%*
const activeView = app.workspace.getActiveViewOfType(tp.obsidian.MarkdownView);

if (!activeView) {
  new tp.obsidian.Notice("No active markdown editor found.");
  return;
}

const editor = activeView.editor;
const content = editor.getValue();

// Helper to generate a 32-character dashless UUID
const getCleanUUID = () => crypto.randomUUID().replace(/-/g, "");

// Separate YAML frontmatter if present
let body = content;
let frontmatter = "";
if (content.startsWith("---")) {
  const endFmIndex = content.indexOf("\n---", 3);
  if (endFmIndex !== -1) {
    frontmatter = content.slice(0, endFmIndex + 4);
    body = content.slice(endFmIndex + 4);
  }
}

// Regex matching Slides Extended horizontal (---) and vertical (--) delimiters
const slideRegex = /(^|\n)(---|\-\-)(?=\s*(\n|$))/g;

let lastIndex = 0;
let result = "";
let match;

// Tag initial slide if not already tagged
const initialSlideHasId = /^\s*<!--\s*slide\s+[^>]*data-id=/i.test(body.trimStart());
if (!initialSlideHasId && body.trim().length > 0) {
  result += `<!-- slide data-id="${getCleanUUID()}" -->\n`;
}

while ((match = slideRegex.exec(body)) !== null) {
  const chunk = body.slice(lastIndex, match.index + match[0].length);
  result += chunk;
  lastIndex = match.index + match[0].length;

  // Check if upcoming slide already has a data-id
  const nextSnippet = body.slice(lastIndex, lastIndex + 120);
  if (!/^\s*<!--\s*slide\s+[^>]*data-id=/i.test(nextSnippet)) {
    result += `\n<!-- slide data-id="${getCleanUUID()}" -->`;
  }
}

result += body.slice(lastIndex);

editor.setValue(frontmatter + result);
new tp.obsidian.Notice("Slide data-ids injected.");
-%>
```

### Step 2: Register as a Command

1. Open **Settings → Templater**.
    
      
    
2. Scroll down to **Template Hotkeys**.
    
      
    
3. Click **Add new template hotkey** and select `Templates/Add Slide UUIDs.md`.
    
      
    
4. Templater registers this template directly as an executable command in the Command Palette.
    
      
    

## Running the Command

1. Open any Markdown note formatted for Slides Extended.
    
      
    
2. Press `Cmd + P` (macOS) or `Ctrl + P` (Windows/Linux) to bring up the Command Palette.
    
      
    
3. Type `Add Slide UUIDs` and hit Enter.
    
      
    

Each slide will receive an injected comment containing its permanent, dashless UUID:

  

Markdown

```
---
theme: black
---

<!-- slide data-id="4b7a10dfa31245089312c417623910ab" -->
# Agenda
- Introduction
- Architecture

---

<!-- slide data-id="2819fc781b23419087cfc84301beaf82" -->
# Live Demo
Notice how this slide's ID stays constant even if we reorder earlier slides!
```

Existing `data-id` comments are preserved, preventing ID overwrites when you run the command again after adding new slides.

With deterministic IDs in place, you can reorder, split, and edit slides freely while keeping external integrations, WebSocket listeners, and automation pipelines stable.