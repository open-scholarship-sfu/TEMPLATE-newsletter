# Markdown email newsletter template

Write each newsletter issue in plain text (Markdown), render it with
[Quarto](https://quarto.org), and paste the result into your mailing
platform. The output is a table-based email with every style inlined, so it
holds up in Outlook, Gmail and Apple Mail and stacks to a single column on
phones. Every issue is kept here with its full history, so anyone can see
how it was made and exactly what was sent.

## Where this template comes from

This template was originally developed by the [Open Scholarship Initiative at
Simon Fraser University (SFU)](https://www.sfu.ca/research/research-impact/performance-excellence/open-science.html) for its monthly newsletter, and is shared
under a CC BY 4.0 licence so others can reuse and adapt it. If you use it,
please keep this credit (see [Licence](#licence)).

## Using this template

1. On GitHub, click **Use this template** → **Create a new repository**.
2. In `_quarto.yml`, replace the placeholders: the newsletter's name and
   organization, colours, footer (contact email, links, about text) and, if
   relevant, a land acknowledgement.
   Optionally, use a banner image instead of the text masthead: see
   `banner:` in `_quarto.yml`.
3. Choose where images will be hosted (usually your mailing platform's image
   library) and set its folder address as `image-base-url` in `_quarto.yml`.
4. In `CODEOWNERS`, replace `@your-org/newsletter-maintainers` with your
   team or GitHub usernames.
5. Replace `newsletter@example.org` with your contact address. It appears in
   `_quarto.yml`, `CONTRIBUTING.md`, `ACKNOWLEDGEMENTS.md` and the issue forms.
6. In the repository's settings:
   - **Actions** → **General** → **Workflow permissions**: tick "Allow
     GitHub Actions to create and approve pull requests", so the monthly
     draft can be opened automatically.
   - **Issues** → **Labels**: create `news`, `needs-info`, `accepted` and
     `declined` (`bug` and `enhancement` usually exist already).
   - Optionally, protect `main` with a ruleset, as described in
     `MAINTAINERS.md`.
7. Look at `issues/_example.qmd` for every component in use, then start your
   first issue (see [Making a new issue](#making-a-new-issue)).
8. Update `CITATION.cff` and `ACKNOWLEDGEMENTS.md` to describe your own
   newsletter, keeping the credit to the original template.

## How it works

Each issue is written in Markdown and rendered with Quarto into an email
that you paste into your mailing platform. Nothing needs installing beyond
[Quarto 1.4 or later](https://quarto.org/docs/get-started/), and you can skip
even that: open a draft pull request, click **Code** → **Codespaces** →
**Create codespace**, and you get the repository in your browser with
Quarto set up.

Want to help, or share news for an upcoming issue? See
[CONTRIBUTING.md](CONTRIBUTING.md). Maintainers' processes are in
[MAINTAINERS.md](MAINTAINERS.md).

## Folder layout

```
_quarto.yml               newsletter-wide settings (name, colours, footer, image address)
_extensions/newsletter/   the design: shell HTML and Lua filter
scripts/fragment.ts       tidies the output after each render (runs automatically)
scripts/new-issue.ts      creates a new month's issue file from the starter
issues/_starter.qmd       blank issue to copy (files starting with _ aren't rendered)
issues/_example.qmd       an example issue showing every component
issues/YYYY-MM-month.qmd  one file per issue
issues/images/            images for the example, and archive copies
_output/issues/*.html     rendered emails, committed as the record of what was sent
.github/                  issue forms, pull request template, monthly workflow
.devcontainer/            Codespaces setup with Quarto installed
```

## Making a new issue

1. On the 5th of each month, a draft pull request is opened automatically
   for **next** month's issue, such as `issues/2026-12-december.qmd` on
   November 5, copied from the starter. Switch to its branch and fill it in. (To create one
   yourself: `quarto run scripts/new-issue.ts 2026-12`.)
2. Upload the issue's images to your mailing platform's image library (see
   [Images](#images)).
3. Preview while writing:

   ```bash
   quarto preview issues/2026-12-december.qmd
   ```

4. Render **only that issue**:

   ```bash
   quarto render issues/2026-12-december.qmd
   ```

   Don't run `quarto render` on its own: it re-renders every published issue
   with the current template, so they would no longer match what was sent.

5. Open `_output/issues/2026-12-december.html` in a text editor, copy
   everything, and paste it into your mailing platform's HTML editor.
6. Send yourself a test before the real send.

## Images

Images are hosted in your mailing platform's image library, and its address is already
set as `image-base-url` in `_quarto.yml`. To use an image:

1. Upload it to your mailing platform exactly as supplied, without editing it.
2. In the issue, write its file name exactly as it appears in your mailing platform,
   with nothing in front of it: no folder and no web address. Make sure to add alt text, captions and credits.

   ```markdown
   ::: {.feature image="workshop-2026-11.jpg" alt="..." cap="Participants at the workshop, November 2026" credits="Jane Doe"}
   ```

The template adds the your mailing platform address when rendering, so both the preview
and the email load the image from your mailing platform. If an image doesn't appear,
check that it's uploaded and that the name matches your mailing platform's character for
character. Avoid spaces in file names. Full `https://` addresses are left as
written.

## Pasting into your mailing platform

With `fragment: true` in `_quarto.yml` (the default), the rendered
file is just the newsletter card, with no page background or outer padding.
It fills the width of your mailing platform's HTML editor, up to 600px.

Set `fragment: false` to get a complete standalone HTML page instead, with
its own background and a fixed 600px card.

## Writing an issue

Front matter: `title`, `subtitle`, `issue`, `date` (written as `YYYY-MM-DD`,
shown as "November 2026"), and `preheader` (the preview text shown in
inboxes).

Text before the first `##` heading is the intro. Each `## Heading` starts a
new section; add `{.tinted}` for a shaded one. When there are two or more
sections, an "In this issue" line with jump links appears after the intro
(turn off with `in-this-issue: false`).

Regular Markdown works everywhere: `###` and `####` headings, lists, links,
bold, quotes, images, tables and `---` dividers.

### Components

**Buttons.** A paragraph containing only button links becomes a row of
buttons.

```markdown
[Register](https://...){.button}
[Learn more](https://...){.button .secondary}
```

**Featured story** with a large image, shown 520px wide. `link` makes the
image clickable.

```markdown
::: {.feature image="story.jpg" alt="..." link="https://..."}
### Title
Text.
:::
```

**Event** with the date in a left column. Write `date` as `YYYY-MM-DD` and
the weekday is filled in for you. `time` and `place` are optional.

```markdown
::: {.event date="2026-11-18" time="10:00 to 11:30 a.m." place="Online"}
### Title
Text.
:::
```

**Spotlight** with a round photo (use a square image).

```markdown
::: {.spotlight image="person.jpg" alt="Portrait of ..."}
#### Name, department
Text.
:::
```

**Highlight box** for deadlines and calls.

```markdown
::: highlight
#### Applications open
Details.
:::
```

**Image beside text**, with the text flowing around it. `side` is `left` or
`right` (default `right`); `width` sets the image's width in pixels (default
200). `link`, `cap` and `credits` work as on other images. On phones, the
image moves above the text at full width. Keep the `::: {.wrap …}` line on
one line.

```markdown
::: {.wrap image="photo.jpg" alt="..." side="right" cap="..." credits="..."}
#### Title
A paragraph or more, so there's enough text to flow around the image.
:::
```

**Two columns** that stack on phones. Each column may have an image.

```markdown
::: two-col
::: {.col image="a.jpg" alt="..." link="https://..."}
#### Item one
Text.
:::
::: {.col image="b.jpg" alt="..."}
#### Item two
Text.
:::
:::
```

## Customising

All settings live under `newsletter:` in `_quarto.yml` and can be overridden
in an issue's front matter: name, organisation line, optional logo, colours,
font stacks, and the footer (about text, contact, links, land
acknowledgement, text below the card).

For unsubscribe or "view in browser" links, paste your mailing platform's
merge tags into `footer: raw-html:`. Many platforms also add their own
compliance footer; check before adding a second one.

Changes to `_quarto.yml`, `_extensions/` or `scripts/` affect every issue, so
please propose them first using the "Propose a template change" issue form
(see [CONTRIBUTING.md](CONTRIBUTING.md#changing-the-template)).

## Tips

- Always give images `alt` text: many readers see emails with images off.
- Keep a single issue under about 100 KB of HTML; Gmail clips longer
  messages.
- Avoid `::: callout-note` and other Quarto callouts; use `::: highlight`.

## Licence

The template is shared under the [CC BY 4.0 licence](LICENSE). It was
originally developed by the Open Scholarship Initiative at Simon Fraser
University; when you reuse or adapt it, keep this credit, for example: *"Based
on the newsletter template by the SFU Open Scholarship Initiative (CC BY 4.0)."*

Your own newsletter's text and images are yours to license as you choose.
Images, logos, event posters and quoted material belong to their respective
owners and are not covered by this licence.
