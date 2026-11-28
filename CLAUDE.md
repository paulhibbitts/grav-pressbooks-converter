# Grav Pressbooks Converter

Converts a Pressbooks book (its XHTML export) into Grav pages for Open Publishing Space (Quark 2 or Quark) or
Grav Helios Open Reader, or into Standard Markdown. It runs on PHP with no build step: `index.html` is the web form,
`convert.php` makes the zip, `preview.php` shows a preview, and `src/` holds the work:

- `Parser.php` – reads the export: book details, parts, chapters, front and back matter, footnotes
- `ContentConverter.php` – turns each page's HTML into Markdown (tables, figures, videos, H5P, glossary terms)
- `ZipBuilder.php` – lays out the Grav pages, writes conversion-notes.txt and builds the zip
- `Helpers.php` – small shared helpers

## Tests

Run before every commit:

```bash
php tests/check.php
```

Each check converts a sample book from `tests/samples/` and compares the result with its saved output in
`tests/expected/`. The tests run offline, so nothing on the web can make them fail. When a change alters the output
on purpose, look at the differences, then save the new output with `php tests/check.php --update` and commit
`tests/expected/` along with the change.

The samples are six real books plus a small made-up one (`edge-cases.html`). Three of the real books are
excerpts, a few chapters each with that book's special cases. `tests/samples/ATTRIBUTION.txt` credits every book.
To test a new case, add a check to the `$checks` list in `tests/check.php` instead of a new script.

## Full books

The complete exports of all six books (and their covers) are in the `pb-test-imports` folder on Paul's iCloud.
Ask Paul to put it on the Desktop (`~/Desktop/pb-test-imports`), then convert one with the web form's settings:

```bash
php cli.php ~/Desktop/pb-test-imports/Working-Group-Guide-1698768885.html /tmp/wgg cover=$HOME/Desktop/pb-test-imports/552052150-resource-covers-working-group-guide-768x994.png
```

Options: `helios`, `standard-markdown`, `with-images`, `no-figure-html`, `embed-h5p` (see the top of `cli.php`).
Unlike the tests, `cli.php` can fetch from the web, such as videos the export leaves out.

To go on to Docsify-This, run the output through the Grav Docsify Bridge (the `grav-docsify-bridge` repo):

```bash
php ../grav-docsify-bridge/cli-test.php grav2docsify /tmp/wgg/pages /tmp/wgg-docsify clean
```

## Working with Paul

- Paul makes all commits and pushes, and deploys to grav-pressbooks-converter.hibbittsdesign.org. Suggest a commit
  message as one plain sentence, with no colon.
- Keep code readable for junior and intermediate developers: plain steps, clear names, brief comments on why.
- Use en dashes (–), never em dashes, in anything people read, including conversion notes.
- Say "web server", never a bare "server".
- Pressbooks sites sometimes block automated requests. If a book's site refuses, stop – never work around a bot
  check.
- Converted books keep their source's AI wishes. BCcampus (opentextbc.ca, pressbooks.bccampus.ca) asks that its
  books not be used for AI training, so their demo copies are blocked for AI crawlers in robots.txt.
