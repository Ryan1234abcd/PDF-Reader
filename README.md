# PDF Spec Sheet Extractor

One file: **`pdf-spec-extractor.html`**. Double-click it. Nothing is installed,
nothing is uploaded, no server runs — the PDFs are read inside your browser and
never leave your machine.

## How to use it

1. **Drop your folder of PDFs** onto the grey box at the top (sub-folders are
   searched too, and anything that isn't a `.pdf` is ignored). The first page of
   the first PDF appears so you have something to work on.
2. **Draw a box** with the mouse around a value you want — say the model number.
   A new field appears on the right with the cursor already in its name, so just
   type the column name (`Model Ref`) and press Tab.
3. **Repeat for every value** you want as a column. If a value lives on page 2,
   press *Next* to go to page 2 first, then draw — the field remembers the page
   it was drawn on. You can also change the page number in the field list.
4. **Adjust boxes** any time: drag them, drag the red corner handles to resize,
   select one and nudge it with the arrow keys (hold Shift for bigger steps),
   rename it, type exact percentages, or delete it with the `×` button.
5. **Press Run.** Every PDF is read in turn, with a progress bar. You get one row
   per PDF and one column per field.
6. **Check the results.** Click any cell and the page it came from is redrawn
   with that box on it, and the text it pulled out is printed underneath.
   * A **yellow** cell means the box found nothing.
   * A **red** cell means that page could not be read — the PDF is a scan with
     no text in it, the PDF has fewer pages than the template expects, or the
     file would not open at all. Those rows are flagged for you to do by hand.
7. **Export CSV** for the schedule, and **Export Template** for the boxes.
8. **Next time**: open the page, *Import Template*, drop the new folder, press
   Run. No redrawing.

The template JSON file is the only thing that is saved. The page deliberately
uses no browser storage, so keep that file somewhere you can find it.

## If something breaks

| What you see | What to do |
| --- | --- |
| Red banner: *PDF library did not load* | Your network blocked the CDN. Follow the instructions in the comment at the very top of the HTML file: download `pdf.min.js` and `pdf.worker.min.js`, put them next to the HTML file, and change the two marked lines. |
| A whole row is red | That PDF is a scan (an image, with no real text), or it has fewer pages than the template expects. This tool does no OCR, so handle those by hand. |
| One cell is yellow | The box missed. Click the cell to see the page, nudge or enlarge the box, and press Run again. |
| A cell grabbed the neighbouring column too | Make the box tighter, or raise `MIN_ITEM_OVERLAP` (near the top of the file) from `0` towards `0.5` so a text item must sit mostly inside the box to count. |
| Wrapped lines run together oddly | Adjust `LINE_MERGE_TOLERANCE` (how far apart two bits of text can be and still count as one line) or `WORD_GAP_RATIO` (how wide a gap counts as a space). |
| Boxes are slightly off on one supplier's sheets | Coordinates are stored as fractions of the page, so different paper sizes and rotated pages are handled. If a supplier has genuinely moved things, save a second template for them. |
| Excel mangles accented characters | The CSV already carries the marker Excel needs; if your Excel is old, import it with *Data → From Text* and pick UTF-8. |

All the tunable settings — CDN address, colours, tolerances, CSV options — are
grouped together in the `CONFIGURATION` block near the top of the file.
