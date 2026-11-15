# Whippet

**A fast file finder for Windows.** Find any file on Windows as you type, then preview, tag and move it without leaving the keyboard.

Whippet runs on top of [Voidtools Everything](https://www.voidtools.com/), which keeps an index of every file name on the computer, so a search across millions of files answers while you are still typing. At first sight it is Everything with a preview beside the list. In use, you will find you rarely need a file browser either.

![Whippet: typing a search, the Viewer following each row, tagging a file with Ctrl+T, and /todo listing the files tagged TODO](docs/img/whippet-demo.gif)

## Why you might want it

- **Any file, as you type.** The order follows what you are likely to want, so the file you have in mind is usually near the top.
- **A preview beside the list.** Documents, images, PDFs and media show as you move down the rows, without opening anything.
- **Each folder's own notes.** A folder's `README.md`, `CLAUDE.md` or similar shows beside its files, and you can edit it there.
- **A file browser in the same list.** Go into a folder with `→` and back up with `←`, select several rows, then copy, move, rename or delete them.
- **Stars** for the files and folders you return to.
- **Tags and tasks in the file name.** One command lists every TODO, or every overdue task, on every drive at once.
- **Keyboard first, mouse welcome.** Everything can be done from the keyboard, and the same actions are on buttons, the right-click menu and the `/` command list.

## For everyday users

### What it needs

- Windows 10 or 11.
- [Voidtools Everything](https://www.voidtools.com/) 1.5 (in beta) with its HTTP Server plugin and HTTP server turned on. Whippet relies on Everything 1.5's `sort-mix:` search; Everything 1.4 is not supported.

### Install

1. **Install Everything 1.5** from the [Everything 1.5 page](https://www.voidtools.com/everything-1.5/), then add the Everything HTTP Server plugin from [voidtools' plugins page](https://www.voidtools.com/support/everything/plugins/). Let Everything finish its first index.
2. **Turn on the HTTP server** in Everything under **Tools, Options, HTTP Server**. Leave the address and port as they are.
3. **Install Whippet**, either way:
   - Download a `Whippet Setup` installer from the [Releases page](https://github.com/stevepowell99/whippet-files/releases) and run it. The installer is not code-signed, so Windows SmartScreen may warn you the first time; choose **More info, Run anyway**.
   - Or run it from source: install [Node.js](https://nodejs.org/), then `git clone https://github.com/stevepowell99/whippet-files`, `npm install` and `npm start` in that folder.

Open Whippet. If it cannot search through Everything, a setup card takes the place of the list. It works out which step you are on (Everything not installed, not running, only version 1.4, no HTTP Server plugin, or the HTTP server switched off), shows that one step with a button, and checks again by itself until Everything answers, when the card goes. When the HTTP server is on at a port other than the one in **Settings**, under **Connection to Everything Search**, Whippet sets that address to the port Everything's settings name. The card only reads Everything's settings and never changes them.

Some buttons depend on other tools of mine or on a Google setup of your own, and appear only on a machine that has them: opening markdown in the [gmist](https://mist.broad-smoke-cc64.workers.dev) editor needs a gmist checkout beside Whippet, **Print to PDF** needs a JobCat checkout with its `make_pdf.py`, and Google Drive sign-in needs a Google OAuth client file (see "Google Drive setup" in [`docs/DEVELOPING.md`](docs/DEVELOPING.md)). Without them, those buttons are not shown.

Owner tags (`xp` plus a name, such as `xpSAM`) are the names you set in **Settings**, under **Owner tags**. There are none until you add some.

### Licence

MIT; see [`LICENSE`](LICENSE).

### Support

I built Whippet for my own work and use it every day. I read every issue, but I cannot promise fixes or support.

### First things to try

- **Set a search scope (recommended on a new PC).** In **Settings**, set one **search scope folder**, for example your profile or a projects root. Everything then only returns paths under that root.
- **Pick a current folder** from the breadcrumb. Search, paste and new files use the current folder.
- **Narrow the list** from the `/` list or the options panel: folders only, files only, or this folder only.
- **Tag a file.** Select a row, press `Ctrl`+`T`, and add a tag like `draft` or `2026`. The file is renamed on disk straight away.
- **Press `F1` or `Ctrl`+`/`** any time for the full in-app guide.

## Features

![Whippet main window](docs/img/whippet-main.png)

### Finding

- **Search as you type**, on every drive Everything indexes. Name matches rank above path matches, then files changed recently, starred and often used, with a boost for Windows' own Recent list. **Settings**, under **Ranking**, weights each of these.
- **From any program.** `Ctrl`+`Alt`+`Space` brings Whippet up with the search text selected, so typing replaces it and an arrow key steps out of it; the key can be changed in Settings. Alt-tab and a click into the window leave the cursor where it was. `Ctrl`+`Shift`+`Space` shows everything changed in the last hour.
- **Narrowing.** From the `/` list and the options panel: this folder only, folders only or files only, a file type (`.md`), a tag or a deadline. `Ctrl`+`R` steps through the last hour, day, week, month, year and any time. `-word` leaves a word out.
- **Stars.** `Ctrl`+`D` stars a file or folder, which then ranks near the top of every search that finds it. To star the folder you are in, press `Ctrl`+`D` with no row selected, click the star among the folder chip's buttons, or type `/star`. `Ctrl`+`Shift`+`1` to `9` opens the first nine in the order they were starred: a folder becomes the current folder and a file opens.
- **Saved searches and history.** `/` then **Save this search** keeps the folder, the text and the filters. Each saved search is a `/` command of its own, and `Alt`+`1` to `9` runs the first nine. `Alt`+`←` and `Alt`+`→`, or the two arrows in the search box, go back and forward through earlier searches.

### Looking

- **Previews.** Images, PDFs, Word, Excel, PowerPoint, text, JSON, markdown, audio and video show in the Viewer, on the right third of the window. Markdown and text files can be edited there.
- **Folder docs.** With a folder selected, the Viewer shows the folder's own note: a file named `-readme.md`, `readme.md`, `claude.md`, `agents.md`, `about.md`, `index.md` or similar, which you can edit in place, so each folder can describe itself. **Settings**, under **Viewer docs**, changes the names.
- **Mixed local and Google Docs.** Local Office files preview inline, older `.doc` files as their text. A Google Workspace shortcut (`.gdoc`, `.gsheet`, `.gslides`) is resolved from the shortcut or the Drive stream, including streamed (on-demand) files. The Viewer shows the PDF Google makes of it, fetched with the sign-in of Whippet's Google window, so open one there once to sign in. **Edit** opens it in that window.
- **Google Drive shortcut folders.** Shared-folder shortcuts under `.shortcut-targets-by-id` are resolved to their real names in the breadcrumb and Name column, cached across restarts.

### Managing files

- **The keys Explorer and Everything already use.** Copy, cut and paste; drag and drop; rename; new folders; delete to the Recycle Bin, or for good with `Shift`+`Del`.
- **Paste from Explorer and screenshots.** Paste copied files into the current folder, or paste a screenshot to save `Clipboard image.png` there.
- **The Shelf.** A staging strip under the results, one line high while empty. Drag files onto it from several folders, go elsewhere, then drag them onto a folder row or into the list. A drag moves and `Shift` copies, both onto the Shelf and off it. An item taken off with its × goes to the Recycle Bin. A clipboard for files that does not empty when you move.
- **Bulk rename.** Check several files, press `Ctrl`+`H`, and use wildcards on the last path segment with a live preview.
- **Open and Save dialogs.** With another app's Open or Save dialog in front, the toggle shortcut raises Whippet with a strip saying where the next pick goes. Enter sends the selected row to the dialog: in an Open dialog a file opens and a folder is shown; a Save dialog gets the folder, keeps its file name and leaves Save to you.

### Tags and tasks

- **Filename tags.** `Ctrl`+`T` adds and removes `xktag` labels stored in the file name: `Report xkTODO.docx` is tagged TODO, and typing `/todo` lists everything tagged TODO. There is no database and no hidden metadata, so the tags travel with the file into Explorer, Google Drive, zip files and git.
- **Tasks.** A task carries a status, an owner, a deadline (`xd-2026-07-15`) and a hide-until date (`xh-2026-09-01`), with filters for overdue, due today and due this week.
- **New TODO.** The Quick TODO box creates a small markdown file tagged `xkTODO`: `Alt`+`Shift`+`N` from any program, the Viewer's **New TODO** for the folder on screen, and the `/` command **New TODO in this folder** for the current folder.
- **Readable by AI assistants.** Because tags and tasks are plain names and markdown, people, sync services and an AI assistant working in the same folders can all read and change them.

### Other editors

- **Open in Obsidian.** A `.md` file inside one of your Obsidian vaults gets a gem button on its row, and the same entry in its `⋯` menu, which opens it in Obsidian; `Ctrl`+`O` does the same for the selected row. Whippet reads the vault list from Obsidian's own settings, so a newly added vault is picked up the next time the window gets focus. Files outside every vault, and inside a vault's hidden folders such as `.obsidian`, get neither, because Obsidian cannot open them.
- **Open in gmist.** On a machine with a [gmist](https://mist.broad-smoke-cc64.workers.dev) checkout, a markdown file (`.md`/`.qmd`) gets a row button for that web editor, and you choose local or online per click. The pen icon opens the file in a local gmist straight from disk by its path (any file on any drive, in or out of Google Drive), in a Whippet window, starting gmist first if it is not running; a cold start builds it, which takes about two minutes. The cloud icon opens the deployed editor in your browser using the file's Drive id (Google Drive files only). Both show on a Drive file. Online, auth is your signed-in gmist session; local gmist needs no sign-in.
- **Doc windows.** A gmist or Google Workspace window is titled with the file it was opened from, and shows that name in its own toolbar too. Minimised, it parks as a pill beside the Shelf, and a click on the pill brings it back. Its icon and its pill carry two letters from the name (`alpha report.docx` becomes AL); where two open documents would claim the same pair, the second takes another, so README.md, readme notes.md and reference sheet.md show as RE, RN and RS. Each gets a colour of its own, fixed by its letters. Opening a file that already has a window brings that window back.

### The window

- **Keyboard first.** Navigate, tag, rename, bulk-edit and search without the mouse. `/` lists every command, `?` or `Ctrl`+`/` every key, and `F1` opens the guide.
- **Hide the window's title bar.** A Settings box, off by default, applied on the next start. Windows then draws only the minimise, maximise and close buttons, over the top right of the page, and you drag the window by its top strip. Double-clicking the strip maximises and restores.

## For developers

How to run from source, build an installer and run the tests, with the design notes and measurements behind the code, is in [`docs/DEVELOPING.md`](docs/DEVELOPING.md).
