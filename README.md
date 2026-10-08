# Deodhar Studio

A standalone, offline editor for drawing cells, tracing pipes, and checking Deodhar diagram conditions. The interface is entirely in English, including the custom reading-order editor.

The code for this app was written by GPT through an iterative "vibe coding" workflow guided by the project author.

## Download and open

[Download the HTML app](https://github.com/iiivyliu/deodhar-studio/releases/latest/download/deodhar-editor.html) · [Download the ZIP with this guide](https://github.com/iiivyliu/deodhar-studio/releases/latest/download/deodhar-studio.zip) · [All releases](https://github.com/iiivyliu/deodhar-studio/releases)

1. Open this repository's [**Releases** page](https://github.com/iiivyliu/deodhar-studio/releases).
2. Download **deodhar-editor.html**, or download **deodhar-studio.zip** and extract it.
3. Double-click `deodhar-editor.html` to open it in a modern desktop browser. You can also choose **Open with** and select your browser.

No installation, build, server, account, or internet connection is needed to use the downloaded app. All code and styling are inside the HTML file. The optional reference link in **Definitions & scope** opens an external paper when clicked.

If you download from the repository's file viewer, use **Download raw file**; saving the GitHub page itself does not download the app. The repository's **Code > Download ZIP** option also includes the HTML and this guide.

## Drawing and controls

| Action | Control |
| --- | --- |
| Create a rectangle | Set **Rows A** and **Columns B**, then select **New rectangle**. |
| Load the built-in valid example | Select **3 × 4 example**. |
| Switch a tile | Click an existing cell to toggle **0: elbow** and **1: crossing**. |
| Add or remove cells | Hold **Ctrl** while clicking or dragging. Starting on an empty cell adds; starting on an existing cell erases. The stroke keeps that action. |
| Edit shapes without a modifier key | Enable **Shape brush**, then click or drag. |
| Move the keyboard cursor | Focus the board and use the arrow keys. |
| Edit with the keyboard | **Space** toggles a tile; **Ctrl + Space** adds or removes a cell. |
| Undo / redo | Use the buttons, **Ctrl + Z**, or **Ctrl + Shift + Z**. **Ctrl + Y** also redoes. |
| Change annotations | Choose **None**, **0 / 1**, **Generator**, or **Reading order** under **Cell labels**. |
| Zoom | Use **−** and **+** above the board. |
| Save an editable diagram | **Save diagram** downloads a JSON file. |
| Restore a saved diagram | **Open diagram** loads a compatible JSON file. |
| Export a figure | **Export SVG** downloads a vector image. |

The app also recognizes Command as the modifier for drawing and undo/redo on macOS; **Shape brush** is available if a browser or operating system intercepts a shortcut.

Rectangle dimensions are limited to 1–40 in each direction. Edited shapes may contain at most 1,600 cells inside a 64 × 64 bounding box. Holes, missing corners, staircases, and disconnected cells are supported.

## Custom reading order

The default reads rows **from bottom to top**, and cells **from right to left** within each row.

1. Select **Custom order**.
2. Click cells individually to assign positions **1, 2, 3, …**. Green borders identify cells available next.
3. Use **Step back**, **Start over**, or click an already numbered cell to revise the sequence. **Complete remaining** fills the rest with a compatible order.
4. Select **Apply order** after every cell is numbered and the dependency check passes. **Cancel** keeps the previously applied order.

Operations whose simple-generator indices differ by more than 1 can exchange positions. Cells with equal or neighboring generator indices must retain their relative order from the default sequence. An incompatible sequence shows its missing prerequisites and cannot be applied. This restriction preserves the diagram's permutation and distinguished-condition results.

Changing elbows and crossings preserves an applied custom order. Adding or removing cells resets it to the default; **Undo** restores the prior state, including its order. **Reset reading order** returns to the default sequence. JSON saves include the applied order, and SVG export preserves the selected reading-order labels.

## Validation rules and mathematical scope

The app evaluates explicit combinatorial conditions. A green result applies only to the selected rule and the conventions below.

### Default rectangle rule

For a complete A × B rectangle, **Notes definition / Generalized** checks the four conditions identified in the app as Definition 2.1:

- A and B are coprime.
- The mask is distinguished: at every elbow, the current bottom-left pipe label is smaller than the top-right label.
- The final pipe permutation is the identity, so boundary labels match.
- There are exactly **(A−1)(B−1) crossings**, equivalently **A+B−1 elbows**.

Non-coprime rectangles are reported as outside this definition. The four conditions are stated here so the tool can be used without separate notes.

### Word-mask convention for arbitrary shapes

Cell coordinates increase rightward in x and upward in y. For the selected cells, let **d = min(x+y)**. A cell contributes the adjacent transposition **s_(x+y−d+1)**. The strand count **n** is one more than the largest generator index. The default reading order, or a compatible custom order, gives the word.

Start with labels 1 through n. A crossing swaps adjacent positions. An elbow leaves them unchanged and is allowed only when those two current labels are increasing. Blank cells are excluded from the word and do not count as elbows. Translating the whole shape preserves the checks. Faint gap connectors in the drawing route identity pipes through blank cells.

For a nonrectangular shape, the default checks **distinguishedness and identity only**. It does not establish maximality, and it does not assume the shape is a Young diagram from the notes.

### Other selectable rules

| Rule | Conditions |
| --- | --- |
| **Identity + distinguished** | Distinguished mask and identity final permutation. |
| **Distinguished only** | Distinguished mask; identity and crossing count are not required. |
| **Mₙ: identity, distinguished, n−1 elbows** | Distinguished mask, identity, and exactly n−1 elbows. The app labels a passing mask maximal only when the full word, with every generator included, gives a single n-cycle. This is not a general maximality test for arbitrary shapes. |

The app links to Galashin–Lam, Definitions 9.2–9.3, for published definitions. Its precise working conventions are also available in **Definitions & scope**.

**No Deodhar-to-Dyck-path bijection is implemented.**

## Saving and privacy

The current draft is saved in browser-local storage when available. It is not sent to a server. Browser settings, private browsing, clearing browser data, or moving the HTML file can make that draft unavailable. Use **Save diagram** for a portable backup, especially before replacing the app with a newer version. JSON stores the cells, selected validation rule, and applied reading order; SVG is an exported figure rather than an editable diagram backup.

## Verification

The release preparation passed **30 automated algorithm and DOM-adapter checks**, including exhaustive small-rectangle counts, independent pipe tracing on 250 irregular shapes, invalid import handling, drawing state, undo/redo, SVG output, and custom-order save/restore. These checks use a simulated DOM; they do not establish actual browser layout or native pointer-capture behavior. Actual browser UI testing was not performed for this release preparation.
