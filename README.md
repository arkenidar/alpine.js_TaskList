# JS Framework Comparison

A concrete exploration of different JavaScript approaches by building the same application three ways: Alpine.js, jQuery Slim, and Vanilla JS.

## Two Experiments

| App | Alpine.js | jQuery | Vanilla JS | Description |
|-----|-----------|--------|------------|-------------|
| **[TaskList](index.html)** | [Alpine](index.html) | [jQuery](jquery.html) | [Vanilla](vanilla-js.html) | Add, delete, edit, filter tasks |
| **[Tic-Tac-Toe](tic-tac-toe/index.html)** | [Alpine](tic-tac-toe/index.html) | [jQuery](tic-tac-toe/jquery.html) | [Vanilla](tic-tac-toe/vanilla-js.html) | Game grid, winner detection, move history |

Each app is implemented three times with identical functionality, comparing automatic reactivity (Alpine.js) vs manual reactivity (jQuery, Vanilla JS).

---

## TaskList

This repository documents a hands-on evaluation of three different approaches to building the same tasklist application:

1. **Alpine.js** - Automatic reactivity ✅
2. **jQuery Slim** - Manual reactivity with convenient syntax ✅
3. **Vanilla JS** - Manual reactivity without dependencies ✅

## Motivation

The goal was to evaluate different frameworks and patterns by implementing identical functionality in each, then comparing:
- Code clarity and maintainability
- Reactivity patterns (automatic vs manual)
- Dependencies and bundle size
- Development experience

## Project Structure

```
/home/darioc/web/alpine.js_TaskList/
├── index.html           # Alpine.js version
├── jquery.html          # jQuery Slim version
├── vanilla-js.html      # Vanilla JS version
├── css/
│   ├── alpine.css      # Styles for Alpine.js version
│   ├── jquery.css      # Styles for jQuery version
│   └── vanilla-js.css  # Styles for Vanilla JS version
├── show-source.js       # Source code viewer from arkenidar/dirlist-suite
└── README.md
```

**Architecture:**
- **Separation of Concerns**: CSS extracted into separate files for maintainability
- **Identical Styling**: All three CSS files contain the same styles to ensure fair comparison
- **Source Viewing**: Each HTML file includes `show-source.js` for viewing source code in-browser
  - Source: https://raw.githubusercontent.com/arkenidar/dirlist-suite/refs/heads/main/web/show-source.js
  - Repository: [arkenidar/dirlist-suite](https://github.com/arkenidar/dirlist-suite)
- **No Build Step**: Pure HTML + CSS + JavaScript - just open in browser

## Reference Implementation

Based on [arkenidar/list-app](https://github.com/arkenidar/list-app), which uses a **manual reactivity pattern**:
- Data stored directly on DOM nodes
- Explicit function calls trigger updates
- Visual filtering (show/hide) instead of re-rendering
- Simple `[text, status]` array structure

## The Three Versions

### 1. Alpine.js Version ([index.html](index.html))

**Status:** ✅ Complete

**Approach:** Automatic reactivity with declarative templates

**Key Features:**
- Reactive data binding via `x-model`
- Automatic view updates
- Computed properties with getters
- Minimal JavaScript code

**Data Model:**
```javascript
{
  tasks: [
    { id: 1, text: "Task", completed: false, editing: false }
  ],
  filter: 'all',
  get filteredTasks() { /* computed */ }
}
```

**Pros:**
- ✅ Very concise (~270 lines total)
- ✅ Automatic reactivity - change data, view updates
- ✅ Declarative templates easy to read
- ✅ No manual DOM manipulation

**Cons:**
- ❌ Requires Alpine.js dependency (~15KB)
- ❌ Learning curve for Alpine syntax
- ❌ "Magic" automatic updates (less explicit control)

**Code Example:**
```html
<!-- Automatic two-way binding -->
<input type="checkbox" x-model="task.completed">

<!-- Computed filtering -->
<template x-for="task in filteredTasks" :key="task.id">
  <li>...</li>
</template>
```

---

### 2. jQuery Slim Version ([jquery.html](jquery.html))

**Status:** ✅ Complete

**Approach:** Manual reactivity with jQuery's convenient DOM API

**Key Features:**
- Data stored in DOM element: `$container.data('list-content', array)`
- Explicit function calls for updates
- Show/hide filtering (arkenidar's pattern)
- jQuery's fluent API for DOM manipulation

**Data Model:**
```javascript
// Stored in container.data('list-content')
[
  ["Task text", "done"],   // status: "done" or "todo"
  ["Another task", "todo"]
]
```

**Reactivity Flow:**
```
User Action → Event Handler → Update Data → Call Render Function → DOM Updates
```

**Pros:**
- ✅ Explicit control over updates
- ✅ jQuery syntax is clean and familiar
- ✅ Easy DOM manipulation and traversal
- ✅ Matches arkenidar's design philosophy

**Cons:**
- ❌ Requires jQuery Slim dependency (~24KB)
- ❌ More verbose than Alpine.js
- ❌ Manual re-render calls needed

**Code Example:**
```javascript
// Manual data update
function list_checkbox_change(index, $container) {
  const listContent = $container.data('list-content');
  listContent[index][1] = listContent[index][1] === 'done' ? 'todo' : 'done';

  // Explicit call to update view
  list_visually_filter($container, $container.data('filter'));
}

// jQuery's fluent API
const $text = $('<span class="task-text">')
  .text(text)
  .toggleClass('completed', status === 'done')
  .on('click', () => { /* edit handler */ });
```

---

### 3. Vanilla JS Version ([vanilla-js.html](vanilla-js.html))

**Status:** ✅ Complete

**Approach:** Manual reactivity without dependencies - Direct conversion from jQuery

**Key Features:**
- Zero dependencies - 100% native JavaScript
- Data stored on DOM node: `container.data_content`
- Explicit function calls for updates
- Modern DOM APIs (querySelector, classList, etc.)
- Same arkenidar reactivity pattern as jQuery version

**Data Model:**
```javascript
// Stored on container.data_content
[
  ["Task text", "done"],   // status: "done" or "todo"
  ["Another task", "todo"]
]
```

**jQuery → Vanilla JS Conversions Used:**
```javascript
// Data storage
$container.data('list-content')  →  container.data_content

// Element creation
$('<div>')                       →  document.createElement('div')

// Selectors
$container.find('.task-list')    →  container.querySelector('.task-list')
$container.find('.task-item')    →  container.querySelectorAll('.task-item')

// Event handlers
.on('click', fn)                 →  .addEventListener('click', fn)

// DOM manipulation
.append(el)                      →  .appendChild(el)
.empty()                         →  .innerHTML = ''
.replaceWith(el)                 →  .replaceWith(el)

// Class manipulation
.addClass('class')               →  .classList.add('class')
.removeClass('class')            →  .classList.remove('class')
.toggleClass('class', bool)      →  if/else with classList

// Text/Value
.text('text')                    →  .textContent = 'text'
.val()                           →  .value
```

**Pros:**
- ✅ Zero dependencies
- ✅ Full control over code
- ✅ Modern native APIs
- ✅ No library lock-in

**Cons:**
- ❌ More verbose than jQuery
- ❌ More manual DOM manipulation
- ❌ Have to write helper patterns yourself

**Code Example:**
```javascript
// Manual DOM creation
const textSpan = document.createElement('span');
textSpan.className = 'task-text';
textSpan.textContent = text;
if (status === 'done') {
  textSpan.classList.add('completed');
}

textSpan.addEventListener('click', () => {
  // Edit handler
  const input = document.createElement('input');
  input.className = 'task-edit-input';
  input.value = text;
  textSpan.replaceWith(input);
  input.focus();
  input.select();
});
```

---

## Arkenidar's Manual Reactivity Pattern

A key insight from [arkenidar/list-app](https://raw.githubusercontent.com/arkenidar/list-app/refs/heads/main/list-app.js):

### Core Principles

1. **Data on DOM Nodes**
   ```javascript
   // Store state directly on the container
   main_node.data_content = [["Task", "done"], ["Task2", "todo"]];
   main_node.filter_criterion = 'all';
   ```

2. **Explicit Updates**
   ```javascript
   // Every change requires manual function call
   function list_checkbox_input(index) {
     const task = main_node.data_content[index];
     task[1] = task[1] === 'done' ? 'todo' : 'done';

     // MUST call filter to update view
     list_visually_filter(main_node.filter_criterion);
   }
   ```

3. **Visual Filtering (Not Re-rendering)**
   ```javascript
   // Show/hide existing DOM elements
   function list_visually_filter(criterion) {
     items.forEach((item, index) => {
       const status = main_node.data_content[index][1];
       const show = criterion === 'all' ||
                    criterion === status;
       item.style.display = show ? 'flex' : 'none';
     });
   }
   ```

### Why This Pattern?

**Philosophy:** Explicit over implicit
- You control when updates happen
- No "magic" - you see every state change
- Clear data flow: action → update → render
- Predictable and debuggable

**Trade-offs:**
- More verbose code
- Easy to forget to call render
- But: Total control and transparency

---

## Feature Comparison

All three versions implement identical functionality:

| Feature | Alpine.js | jQuery | Vanilla JS |
|---------|-----------|--------|------------|
| **Add tasks** | ✅ | ✅ | ✅ |
| **Delete tasks** | ✅ | ✅ | ✅ |
| **Toggle completion** | ✅ | ✅ | ✅ |
| **Inline editing** | ✅ | ✅ | ✅ |
| **Filter (All/To Do/Done)** | ✅ | ✅ | ✅ |
| **Keyboard shortcuts** | ✅ | ✅ | ✅ |
| **Empty state** | ✅ | ✅ | ✅ |

### User Experience Features

- **Click to edit**: Click task text to edit inline
- **Enter saves**: Press Enter to save changes
- **Click outside saves**: Clicking outside the edit input accepts the change (blur)
- **Escape cancels**: Press Esc to discard changes
- **Auto-focus**: Input auto-focuses when editing
- **Text selection**: Text auto-selects for easy replacement
- **Active filter**: Red border shows active filter
- **Visual feedback**: Strikethrough for completed tasks

---

## Code Size Comparison

| Version | HTML + JS | CSS | Total Lines | Dependencies | Bundle Impact |
|---------|-----------|-----|-------------|--------------|---------------|
| Alpine.js | 163 lines | 165 lines (shared) | 328 lines | Alpine.js (~15KB) | ~343KB |
| jQuery Slim | 239 lines | 165 lines (shared) | 404 lines | jQuery Slim (~24KB) | ~428KB |
| Vanilla JS | 278 lines | 165 lines (shared) | 443 lines | None | ~443KB |

**Architecture:**
- **CSS Extraction**: All styling moved to separate files (`css/*.css`)
- **Identical Styling**: All three CSS files contain the same 165 lines
- **Separation**: HTML structure + JavaScript logic vs CSS presentation
- **Source Viewing**: Each includes `show-source.js` reference

**Key Findings:**
- **Vanilla JS** is the most verbose (278 lines) but has zero dependencies
- **Alpine.js** is the most concise (163 lines) with automatic reactivity
- **jQuery Slim** falls in the middle (239 lines) with convenient syntax
- CSS accounts for ~50% of total code in all versions (165/328-443 lines)

---

## Reactivity Patterns Compared

### Alpine.js: Automatic Reactivity

```javascript
// Change data
this.tasks.push({ id: 3, text: "New", completed: false });

// View updates automatically! ✨
```

**How it works:**
- Alpine tracks dependencies in getters
- Proxies detect data changes
- Automatically re-renders affected DOM

### jQuery/Vanilla: Manual Reactivity

```javascript
// Change data
listContent.push(["New task", "todo"]);

// MUST manually trigger render
list_render($container);
```

**How it works:**
- You modify data directly
- You call render function explicitly
- You control when DOM updates

---

## Key Learnings

### 1. Automatic vs Manual Reactivity

**Automatic (Alpine.js):**
- Less code, more productivity
- Harder to debug when things go wrong
- Framework-dependent

**Manual (jQuery/Vanilla):**
- More code, more explicit
- Easier to understand and debug
- Framework-independent

### 2. The jQuery Question

jQuery's main value today:
- ✅ Convenient fluent API
- ✅ Familiar syntax
- ❌ But: Modern DOM API is actually quite good
- ❌ And: 30KB for conveniences you can write yourself

### 3. Arkenidar's Pattern Insights

**Strengths:**
- Simple data structure (arrays)
- Visual filtering is efficient
- No complex state management
- Easy to understand

**Trade-offs:**
- Array indices as IDs (fragile on delete)
- Data on DOM nodes (non-standard)
- Manual render calls (easy to forget)

**Modern improvements:**
- Use objects with stable IDs
- Consider separate data store
- Helper functions to ensure consistency

---

## Running the Examples

Each version is a standalone HTML file. Just open in your browser:

```bash
# Alpine.js version
open index.html

# jQuery version
open jquery.html

# Vanilla JS version
open vanilla-js.html
```

No build step, no npm install, no configuration. Just HTML + CSS + JavaScript.

### Viewing Source Code

Each HTML file includes `show-source.js` which displays the source code directly in the browser below the application. This makes it easy to:
- See the implementation while using the app
- Compare code side-by-side in different browser tabs
- Learn from the code without opening a text editor
- Share live examples with code visible

**Source:**
- Script URL: https://raw.githubusercontent.com/arkenidar/dirlist-suite/refs/heads/main/web/show-source.js
- Repository: [arkenidar/dirlist-suite](https://github.com/arkenidar/dirlist-suite)
- Author: arkenidar

---

## Findings & Next Steps

**All three versions completed!** ✅ Here's what we learned:

1. **Code Size:** Vanilla JS is most verbose (434 lines), Alpine.js most concise (319 lines)
2. **Dependencies:** Vanilla JS = 0KB, Alpine.js = ~15KB, jQuery Slim = ~24KB
3. **Developer Experience:** Alpine.js fastest to write, Vanilla JS most explicit
4. **Reactivity:** Automatic (Alpine) vs Manual (jQuery/Vanilla) - both valid approaches

**Potential Extensions to Explore:**
   - LocalStorage persistence
   - Drag and drop reordering
   - Task categories
   - Due dates

---

## Final Comparison

After implementing all three versions, here's the verdict:

### When to Use Each

**Alpine.js** - Best for:
- Projects where developer velocity matters
- Apps with complex state and reactivity needs
- Teams comfortable with declarative frameworks
- When 15KB dependency is acceptable

**jQuery Slim** - Best for:
- Teams already familiar with jQuery
- Projects where DOM manipulation is frequent
- When you want manual control with convenience
- Transitioning legacy jQuery code

**Vanilla JS** - Best for:
- Zero-dependency requirement
- Maximum control and transparency
- Learning exercise / educational purposes
- Performance-critical applications
- When bundle size must be minimal

### The Real Insight

**There's no "best" choice** - it depends on your priorities:
- Value **developer time**? → Alpine.js
- Value **no dependencies**? → Vanilla JS
- Value **familiar syntax**? → jQuery Slim
- Value **explicit control**? → Vanilla JS or jQuery

Arkenidar's manual reactivity pattern proves that sometimes explicit is better than automatic, even if it requires more code.

---

## Philosophy

This project is about **concrete exploration**, not theoretical comparison:

- Build the same thing multiple ways
- Feel the differences in practice
- Make informed decisions based on experience
- Understand trade-offs deeply

As arkenidar's implementation shows, sometimes the "old-fashioned" manual approach is clearer and more maintainable than automatic "magic" - it depends on your values and the specific use case.

### Code Organization Evolution

The project structure evolved to improve maintainability:

**Initial approach:** Single HTML files with embedded CSS and JavaScript
- ✅ Simple to understand and deploy
- ❌ Harder to compare styling differences
- ❌ Duplication of identical CSS across files

**Current approach:** Separated CSS into external files
- ✅ Single source of truth for styles
- ✅ Easier to maintain and update styling
- ✅ Clearer separation of concerns (HTML structure, CSS presentation, JS behavior)
- ✅ Still no build step required - just linked files
- ✅ Fair comparison since all versions use identical CSS

This demonstrates that even in "simple" projects, proper separation of concerns improves maintainability without adding complexity.

---

## Resources

- [Alpine.js Documentation](https://alpinejs.dev/)
- [jQuery Documentation](https://jquery.com/)
- [arkenidar/list-app](https://github.com/arkenidar/list-app) - Reference implementation (manual reactivity pattern)
- [arkenidar/dirlist-suite](https://github.com/arkenidar/dirlist-suite) - Source of show-source.js viewer
- [Modern DOM APIs](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
- [You Might Not Need jQuery](http://youmightnotneedjquery.com/)

---

## License

This is educational exploration code. Use freely for learning and evaluation.
