# PDF Spec Sheet Extractors

Two separate tools, deliberately kept apart so neither gets bloated. Both are a
single HTML file you double-click, and both share the same extraction engine,
the same drawing and the same exports.

| File | Use it when |
| --- | --- |
| **`pdf-spec-extractor.html`** | You have a **folder of separate PDFs**, one data sheet each. One row per file. |
| **`merged-sheet-extractor.html`** | You have **one PDF with many data sheets merged into it**. One row per data sheet. |

---

# 1. Folder of PDFs — `pdf-spec-extractor.html`

One file: **`pdf-spec-extractor.html`**. Double-click it. Nothing is installed,
nothing is uploaded, no server runs — the PDFs are read inside your browser and
never leave your machine.

It handles both kinds of spec sheet:

* **Text PDFs** — read directly, instantly and exactly.
* **Scanned PDFs** (the page is a picture; you cannot select any text in it) —
  read with built-in OCR. The `Scanned sheets` box in the toolbar is set to
  *OCR only when needed*, so OCR is used on a page only when there is no real
  text on it. Cells filled by OCR are tinted **blue**.

  Every scanned page is read **twice**, at two different sizes, and any cell the
  two readings disagree about is flagged orange. On a scan a decimal point is
  only two or three dark pixels, so whether it survives depends on how the page
  happens to be turned into pixels — that is how `5.0` becomes `50`. No single
  setting gets it right every time (measured: some settings lose the point in
  one value, others lose it in a different one), so instead of guessing, the
  tool tells you which handful of cells to look at. It doubles the time on
  scanned sheets; untick **Double-check scans** in the toolbar to turn it off.

The screen is kept bare on purpose: press **Info**, top right, to show the
explanatory notes, and press it again to put them away.

The layout is the same in both tools: one bar of controls across the top (files,
Run, CSV, sessions, reading mode), the page in the middle drawn as wide as there
is room for, and everything to do with the boxes — template, which documents a
move applies to, column order — grouped directly above the column list on the
right, where it stays put while the list of columns scrolls under it.

## Do I need OCR?

You do not have to decide — leave **Reading** on *Automatic* and the right thing
happens. The chip beside it tells you what the open PDF is:

* **real text** — the sheet was made by a computer, the text is read exactly and
  instantly, and OCR never runs.
* **a scan** — the sheet is a picture of a page, so the text has to be recognised
  from the image. About 5 seconds a page, and the first scanned page of a session
  downloads roughly 15 MB of language data.
* **hardly any text** — usually a scan carrying an invisible stamp. Automatic will
  trust the little text there is, so if the cells come back empty choose *Force OCR*.

A folder holding both kinds is fine: the decision is made page by page.

## Picking up where you left off

**Save Session** writes one file holding the boxes, the results table, and — unless
you turn it off under Info — the PDFs themselves. **Open Session** puts all of it
back: boxes, table, documents, per-file tweaks and column order, ready to click
through or re-run. Nothing is kept in the browser, so the file works on any machine
and can go in the job folder with the drawings.

## How to use it

1. **Drop your PDFs** onto the grey box at the top — a whole folder, or any
   number of loose files. Sub-folders are searched too, and anything that isn't
   a `.pdf` is ignored. The first page of
   the first PDF appears so you have something to work on. **Files loaded** under
   the status line folds open to list them — click a name to show that PDF, and
   after a run anything needing a check is marked there in red.
2. **Draw a box** with the mouse around a value you want — say the model number.
   A new field appears on the right with the cursor already in its name, so just
   type the column name (`Model Ref`) and press Tab.
3. **Repeat for every value** you want as a column. If a value lives on page 2,
   press *Next* to go to page 2 first, then draw — the field remembers the page
   it was drawn on. You can also change the page number in the field list.
4. **Draw tight.** Only what is really inside the box is taken, letter by
   letter — on scanned sheets and in ordinary text PDFs alike. A box around just
   the number gives `11.000`, not `11.000m³/s`, and a box that stops above the
   next row will not pick that row up. Include the unit in the box if you do
   want the unit. In a text PDF the letter positions are estimated (the exact
   font is not available to the page), so leave a little air between your box
   edge and the unit.
5. **If one PDF is laid out differently**, press **Changing: every PDF** so it
   reads *Changing: THIS PDF only*. It turns amber, as do the boxes, and moving a
   box then changes it for that one file. A box that has been moved is drawn with
   a dashed edge; select it and press **Reset box** to put it back.
6. **Adjust boxes** any time: drag them, drag the red corner handles to resize,
   select one and nudge it with the arrow keys (hold Shift for bigger steps),
   rename it, or delete it with the `×` button. A box is only drawn on the page
   it belongs to, so page 2 is not cluttered with page 1's boxes — use **View**
   in the field list to jump to a field's page. Box names stay out of your way:
   point at a box (or select it) to see its name, or tick **Show all box names**
   above the page if you want them all at once.
7. **Order the columns.** The field list is the column order: top of the list
   is the left-most column, and the file name is always last. Click a field
   (Ctrl+click or Shift+click for several), then press ↑/↓ or use the
   **Move up / Move down** buttons. The arrow keys do one of two jobs depending
   on which half of the screen you last clicked in — nudge the box on the page
   view, move the column in the field list — and the panel with an outline
   round it is the one that has them.
8. **Press Run.** Every PDF is read in turn, with a progress bar. You get one row
   per PDF and one column per field.
9. **Check the results.** Rows are striped so a wide table stays readable.
   Click any cell and the page it came from is redrawn with that box on it, the
   text it pulled out is printed underneath, and the whole row is boxed in so
   you can follow it across without losing your place.
   * A **yellow** cell means the box found nothing.
   * An **orange** cell with a `?` means the two readings of that scanned page
     disagreed about it — hover or click it to see both. This is nearly always
     a decimal point one reading could not see, and these are the only cells
     worth checking by hand.
   * A **red** cell means that page could not be read — the PDF is a scan with
     no text in it, the PDF has fewer pages than the template expects, or the
     file would not open at all. Those rows are flagged for you to do by hand.
10. **Export CSV** for the schedule. For the boxes, type a name in the
   **Template name** box and press **Export Template** — the name becomes the
   file name and is remembered inside the file, so it comes back when you load
   it again.
11. **Next time**: open the page, *Import Template* (or just drop the template
   `.json` onto the drop zone), drop the new folder, press Run. No redrawing.
   The template can be loaded before or after the PDFs — either order works.

The template JSON file is the only thing that is saved. The page deliberately
uses no browser storage, so keep that file somewhere you can find it.

## If something breaks

| What you see | What to do |
| --- | --- |
| Red banner: *PDF library did not load* | Your network blocked the CDN. Follow the instructions in the comment at the very top of the HTML file: download `pdf.min.js` and `pdf.worker.min.js`, put them next to the HTML file, and change the two marked lines. |
| Status line: *OCR engine could not start* | Your network blocked the OCR CDN. See the second set of instructions at the top of the HTML file — and read the warning there: Chrome will not load local OCR files from a double-clicked page, so that route needs a shortcut with `--allow-file-access-from-files`. |
| A whole row is red | The PDF has fewer pages than the template expects, would not open, or is a scan that OCR could not read. The cell text says which. |
| Everything is slow on scanned sheets | That is OCR: roughly 5 seconds per scanned page, and the first one also downloads about 15 MB of language data. Text PDFs stay instant. Lower `OCR_PAGE_SCALE` from 5 to 4 to trade a little accuracy for speed. |
| An orange `?` cell | The two readings disagreed. Hover it to see both, click it to see the box on the page, and type the right value into the CSV. Usually a lost decimal point. |
| An OCR value is slightly wrong | Click the cell to see the box on the page. Superscript footnote markers (the `(1)(2)` after a value on Güntner sheets) and `m³/h` are what OCR fumbles most; numbers and model codes come through reliably. Keeping the box to the number alone avoids most of it. |
| One cell is yellow | The box missed. Click the cell to see the page, nudge or enlarge the box, and press Run again. |
| A cell grabbed the neighbouring row or column | Make the box tighter. Matching is already strict — only text whose middle is inside counts — so this normally means the box genuinely overlaps the other row. |
| A cell is empty although the value is plainly there | The box is a shade too small, so the middle of the text falls outside it. Enlarge it slightly. If you would rather draw rough boxes everywhere, set `MATCH_RULE` (top of the file) to `'overlap'`, or raise `BOX_TOLERANCE` from `0` to about `0.002`. |
| A unit is still stuck to the number | Set `OCR_TRIM_PART_WORDS` to `true` (it is on by default) and redraw the box so it stops before the unit. |
| Wrapped lines run together oddly | Adjust `LINE_MERGE_TOLERANCE` (how far apart two bits of text can be and still count as one line) or `WORD_GAP_RATIO` (how wide a gap counts as a space). |
| Boxes are slightly off on one supplier's sheets | Coordinates are stored as fractions of the page, so different paper sizes and rotated pages are handled. If a supplier has genuinely moved things, save a second template for them. |
| *Import Template* seems to do nothing | Fixed. The hidden file inputs used to sit inside the drop zone, so opening the template chooser also opened the PDF chooser on top of it, and the `.json` was never picked. If you are on an older copy, replace it with this one. |
| A unit is still coming through | Your box overlaps it. Shrink the box, or check that `TRIM_TO_BOX_EDGE` at the top of the file is `true`. |
| Excel mangles accented characters | The CSV already carries the marker Excel needs; if your Excel is old, import it with *Data → From Text* and pick UTF-8. |

All the tunable settings — CDN address, colours, tolerances, CSV options — are
grouped together in the `CONFIGURATION` block near the top of the file.


---

# 2. One merged PDF — `merged-sheet-extractor.html`

Same idea, same everything — drawing, OCR, double-checking, column order,
templates, CSV — except the input is a single PDF holding many data sheets, and
a row comes out per data sheet instead of per file.

## How to use it

1. **Drop the merged PDF** on the drop zone.
2. **Say where each data sheet starts.** Go to the first page of a sheet and
   press **Start a data sheet on this page**; a thick red line appears across
   the top of that page, like a page break in Word. If every sheet is the same
   length — and they usually are — set **every N pages** and press **Mark them
   all** instead. Page 1 always starts the first sheet.
3. **Draw your boxes on the first data sheet** and name them, exactly as in the
   other tool. Each field remembers which page *within* a data sheet it sits on
   (**sheet page** 1 is the page the sheet starts on, 2 is the next), so the
   same boxes are used at the same place on every sheet.
4. **If one sheet is different**, press **Changing: every data sheet** so it
   reads *Changing: THIS data sheet only*. It turns amber, as do the boxes, and
   moving or resizing a box then changes it *for that sheet alone*. A box that
   has been moved is drawn with a dashed edge; select it and press **Reset box**
   to put it back. Press the toggle again for normal editing.
5. **Press Run.** One row per data sheet, with the sheet number and its page
   range as the last two columns.
6. Everything else behaves as in the other tool: click a cell to see the page it
   came from, orange `?` for OCR readings that disagreed, CSV export, and
   templates. The template file also remembers where the sheet breaks are and
   any per-sheet box tweaks, so next month's identical file needs one import.

## If something breaks

Everything in the table above applies here too. Two extra ones:

| What you see | What to do |
| --- | --- |
| Every row is identical, or there is only one row | No breaks are marked, so the whole document counts as one data sheet. Mark them, or use **every N pages**. |
| One data sheet's values are empty while the rest are fine | That sheet is laid out slightly differently. Go to it, switch to **This data sheet only**, and move the box; the other sheets are not affected. |
