# Content Configuration Reference

This is the author-facing configuration interface for course introductions,
local document tabs, and navigation. It is not a REST API: content is read at
build time and published as a static site. No API key or React changes are
needed to add a note set.

This repository owns content only. The renderer, modules, and `npm` commands
referenced below live in
[westlake-drawer.github.io](https://github.com/westlake-drawer/westlake-drawer.github.io).
For local preview, run them there with `NOTES_CONTENT_DIR` pointing at this
checkout. The public site is served at `https://westlake-drawer.github.io/`.

For the contribution workflow, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Choose What to Change

| Goal | File or directory to change |
| --- | --- |
| Add a course | Create its course and term categories, then a note-set introduction |
| Add another year | Add a term directory under the existing course |
| Add another author in the same term | Add a note-set directory under that term |
| Edit a repository introduction | Edit that note set's `index.md` |
| Add a local tab | Create a content file and declare it in `repositoryTabs` |
| Add another PDF or volume | Append to the tab's `documents` list |
| Refresh a compiled outline | Replace its `.toc` file with the new compiler output |
| Refresh upstream README or contributors | Run `npm run sync:repositories` |
| Change navigation labels | Edit `_category_.json` or page frontmatter |
| Refresh directory ordering | Sync repository statistics, then rebuild |

## Directory Contract

An introduction and its local tab files use matching directory segments:

```text
docs/notes/<subject>/<course>/<term>/<note-set>/index.md
content/repository-tabs/<subject>/<course>/<term>/<note-set>/<file>
```

| Segment | Meaning | Example |
| --- | --- | --- |
| `subject` | Existing subject slug | `physics` |
| `course` | Stable course slug, shared by all editions | `quantum-mechanics` |
| `term` | Teaching term, not upload date or release version | `2025-fall` |
| `note-set` | Author, organization, or distinct source | `jietexue` |
| `file` | Local Markdown or compiler-generated outline | `quantum-mechanics-notes.toc` |

Use lowercase, hyphenated directory names. Terms can span semesters, such as
`2025-fall-2026-spring`; use `undated` if the teaching term is unknown.
If one author has two independent sets in one term, use distinct slugs such as
`author-lecture-notes` and `author-study-guide`.

A PDF release update normally replaces files in the same note set. A new
teaching term or independent set gets another directory and introduction.
Preserve compiler-generated filenames; identical names in different directories
do not collide. Two PDFs with the same output filename in one set can use
`volume-i/notes.toc` and `volume-ii/notes.toc`.

## Introduction Frontmatter

Put YAML frontmatter between the first pair of `---` lines in `index.md`.

| Field | Type | Purpose |
| --- | --- | --- |
| `title` | String | Course title shown on the page |
| `description` | String | Short summary used by metadata and directory cards |
| `sidebar_label` | String | Author/source name within the term category |
| `slug` | String, optional | Explicit route, especially to preserve an existing URL |
| `repositoryTabs` | Array, optional | Local custom tabs; omitted means no local tabs |

For a new page, keep `title`, `description`, and `sidebar_label` meaningful.
New pages can omit `slug` and use their nested file-based route.
Existing introductions keep their published short `slug`; never copy one
into another note set. The directory name is not a repository identifier.

Link the actual GitHub repository in the article body:

```md
- Repository: [quantum-mechanics-notes](https://github.com/JieteXue/quantum-mechanics-notes)
```

Repository links are discovered from Markdown/MDX content, not inferred from
directory names or a custom `repository:` frontmatter field. Links to files or
releases in the same repository are deduplicated. Do not manually populate the
generated `githubRepositories` or `repositoryTabDocuments` fields.

## Navigation Categories

All discovered roots are scanned recursively. A new course may add
`docs/notes/<subject>/<course>/_category_.json` for labels and descriptions:

```json
{
  "label": "Quantum Mechanics",
  "description": "Quantum mechanics notes, organized by teaching term and source."
}
```

A term can also provide its own label:

```json
{
  "label": "Fall 2025"
}
```

| Field | Meaning |
| --- | --- |
| `label` | Visible course or term name |
| `description` | Optional short introduction to the automatic listing |
| `link.type` | Optional native override, e.g. `generated-index` |
| `link.slug` | Optional unique site-root route override, without the deployment prefix |
| `link.description` | Optional description when an explicit generated link is used |

Reuse existing categories when adding another source to the same term.
A source directory containing only `index.md` becomes a direct document link.
Existing course category routes end in `/editions` to avoid legacy introduction
URLs. Those explicit links are preserved; new categories need no link setting.
Use site-root slugs; the site is now served at `/`, without a project prefix.
No manual course list or sidebar registration is necessary.

### Automatic Ranking

All discovered sections use the tuple `(Stars DESC, Forks DESC, open issues DESC,
English name ASC)`. Names use case-insensitive English collation. Exact name
ties use stable document IDs or category routes, not their declaration order.
Do not configure `position` or `sidebar_position`; manual priorities are not
used by directory ranking. Each section's root overview remains pinned.

A document's score covers its linked repositories. A category's score covers
the union of repositories in its descendants and linked introduction, counting
each repository once even if several children link to it. Add unique
repositories' statistics, not precomputed child scores. This prevents repeated
editions of one repository from inflating their parent score.

The same order drives sidebar entries, directory cards, and pagination.
It does not reorder custom tabs or the TOC Document selector.
Counts come from `src/data/repository-contributors.json`:

| Snapshot field | GitHub repository API field | Meaning |
| --- | --- | --- |
| `info.stars` | `stargazers_count` | Stars |
| `info.forks` | `forks_count` | Forks |
| `info.openIssues` | `open_issues_count` | Open issues including open pull requests |

Missing or invalid values count as zero during sorting. A successful sync
requires all three nonnegative integer fields; an older snapshot without
`openIssues` is considered incomplete and automatically refreshed.
No extra API request is needed for the new field. Upstream failures retain the
previous snapshot, so sorting may use stale counts until the next successful sync.
Run `npm run sync:repositories`, then restart the development server or rebuild.
Visitors do not trigger GitHub requests or live reordering.

### Automatic Sections and Directory Pages

Each immediate `docs/<section>/` directory containing `index.md` or `index.mdx`
creates a navbar item, footer item, and `<section>Sidebar` automatically.
`_category_.json` sets its `label`; without it, the directory name becomes the
label. Root names prefixed with `_` or `.` are ignored. Navbar sections use
case-insensitive alphabetical label order; nested entries use repository rank.

Every nested directory containing documents gets a native generated category
page at `/<section>/<directory-path>`, with no manual `link` or listing Markdown
needed. `_category_.json` can supply `label` and `description`, but not `position`.
An authored `index.md` overrides the generated page for that category; an
explicit `link` preserves a custom or legacy route. These conventions apply
equally to Notes, Tools, and future sections at every depth.

Tools uses category/tool paths rather than teaching terms, for example
`docs/tools/ai-tools/plugin/<tool>/index.md`. Metadata-only empty categories are
not pages: add a real content document or an intentional placeholder document.
Root entry pages can use `<AutoDocIndex />` to list their contents without a
self-link; the same component works inside an authored category introduction.

GitHub links in real tool introductions participate in the same snapshot sync,
deduplication, and parent totals as Notes. No repository links means zero
metrics and alphabetical ordering. Do not add artificial links to affect rank.

## Local Tab Schema

Declare `repositoryTabs` on the note-set introduction, not on a category.
It requires that the introduction link to at least one GitHub repository.
Each array entry is either a single document or a document group:

| Field | Type | Single document | Document group |
| --- | --- | --- | --- |
| `id` | String | Required | Required |
| `label` | String | Required | Required |
| `file` | String | Required | Must be omitted |
| `source` | Object | Optional PDF link | Must be omitted |
| `documents` | Array | Must be omitted | Required, nonempty |

IDs must match `^[a-z][a-z0-9-]*$`. Tab IDs must be unique on their page.
`overview`, `pdf`, `readme`, `contributing`, and `license` are reserved.
`toc` is a supported custom ID with special placement, not a reserved ID.
Labels must be nonempty English strings.

Every item in `documents` requires its own `id`, `label`, and `file`, plus an
optional `source`. Child IDs must be unique within that group. Groups cannot
contain other groups. Do not use a tab ID equal to another group's generated
scope, such as `toc--volume-i`; the build rejects anchor-scope collisions.
Keep published tab and document IDs stable when changing labels or moving files.

### File and PDF Fields

| Field | Contract |
| --- | --- |
| `file` | Exact, case-sensitive path relative to this repository's root |
| Allowed location | Inside `content/repository-tabs/`, including nested directories |
| Allowed extensions | `.md`, `.toc`, or legacy `.toc.json` |
| Forbidden paths | Missing files, paths outside that root, or `..` segments |
| `source.label` | Nonempty visible PDF link text |
| `source.url` | Absolute HTTPS URL without embedded username or password |

When `source` is provided, both its fields are required. The URL need not end
in `.pdf`, but it should point to the actual PDF, not a generic repository page.
It adds a link and an automatic **PDF** preview tab after TOC and before README.
The website engine caches the PDF at startup/build and serves it from the
site's origin. It does not convert the PDF into Markdown or infer its TOC.
The separate **View on GitHub** link points to the local content file.

The preview uses the same `source` declaration for both Markdown and LaTeX TOC
files. For multiple PDFs, give each entry in `documents` its own `source`; the
PDF tab supplies a document selector. Repeated URLs are shown once. Without
a source, no PDF tab is added. Legacy `.toc.json` embedded sources also work.
Sources must be public HTTPS PDFs, up to 32 MB. The engine caches downloads for
24 hours and retains the previous copy on temporary failures. A first-download
failure displays an original-file link rather than blocking publication.
The PDF tab embeds the browser's native reader; controls vary by browser.
An open-in-new-tab link is always available, including when embedded reading
is unsupported. The PDF loads only when its tab is selected; no custom PDF
rendering library is used.
Keep `pdf` reserved for this automatically generated tab.

The file extension chooses the renderer; `layout` is not supported.
Other undeclared fields are not extension hooks: do not rely on them being
validated or having any effect.

### Single Document Example

Copy this block into the introduction's existing frontmatter. These paths
refer to real files in this repository.

```yaml
repositoryTabs:
  - id: toc
    label: TOC
    file: content/repository-tabs/physics/quantum-mechanics/2025-fall/jietexue/quantum-mechanics-notes.toc
    source:
      label: PDF v1.0.0
      url: https://github.com/JieteXue/quantum-mechanics-notes/releases/download/v1.0.0/quantum-mechanics-notes.pdf
```

### Multiple Documents Example

```yaml
repositoryTabs:
  - id: toc
    label: TOC
    documents:
      - id: volume-i
        label: Volume I
        file: content/repository-tabs/math/fundamental-algebra-analysis/2025-fall-2026-spring/westlake-noter/volume-i.md
      - id: volume-ii
        label: Volume II
        file: content/repository-tabs/math/fundamental-algebra-analysis/2025-fall-2026-spring/westlake-noter/volume-ii.md
```

Use one example or the other, not two separate `repositoryTabs` keys.
Markdown and `.toc` documents can be mixed in the same group. Each child can
have its own optional PDF `source`.
The first document is the default. More than one document shows a **Document**
selector; a one-document group works without showing the selector.

Tab order is Overview, TOC (if declared), README, Contributing, License,
then other custom tabs in declaration order. A reading guide or exercises tab
uses the same schema with another ID and label.

## Rendering and Data Ownership

| Content | Maintained where | Rendered how |
| --- | --- | --- |
| Overview | Note-set `index.md` | Normal Docusaurus Markdown/MDX, including site math and Mermaid support |
| Local `.md` tab | `content/repository-tabs/` | Sanitized GFM and KaTeX; no MDX imports, React, or interactive Mermaid |
| Local `.toc` tab | Original LaTeX compiler output copied here | Specialized outline preserving numbers, hierarchy, and printed page labels |
| Legacy `.toc.json` | Existing local content | Title/entry hierarchy; not recommended for transcribing LaTeX output |
| README, Contributing, License | Linked upstream repository | Saved GitHub document snapshots |
| Stars, forks, contributors | GitHub | Saved repository snapshots |

Do not extract sections called Contributing or License out of a README;
the built-in tabs use separate upstream files when available.
Do not edit generated snapshots in `src/data/` to add local content.

Local files are discovered recursively, but only declared files appear as tabs.
Custom documents are shared by the repositories linked on that introduction.
Changing the GitHub repository selector does not filter or replace local
documents. There is no `repository`, `year`, or `author` binding field in the
tab schema. Prefer separate introductions for independent note sets; a
deliberate comparison page may explicitly reference files from several sets.

For `.toc`, copy the generated file unchanged. No TeX commands are executed,
no `\input` files are followed, and no LaTeX compiler runs in CI.
Printed pages are labels, not PDF navigation targets: printed and physical PDF
page numbers can differ. Unsupported title macros fail the build.
See [LaTeX Course Outlines](CONTRIBUTING.md#latex-course-outlines) for supported
syntax and how to prepare the original LaTeX document.

## URL Interface

Parameters are attached to the introduction URL, before any `#heading`:

| Parameter | Meaning | Default or invalid-value behavior |
| --- | --- | --- |
| `view` | Built-in or declared custom tab ID | Overview |
| `repo` | Linked repository in normalized `owner/name` form | First discovered repository |
| `document` | Child ID in the selected tab's `documents` list | First document in that group |

Examples on the existing Algebra and Analysis page:

```text
/notes/math/fundamental-algebra-analysis/?view=toc&document=volume-ii
/notes/math/fundamental-algebra-analysis/?view=readme&repo=westlake-drawer%2Ffaa-lecture-notes-2025fall-2026spring
```

These are site-root paths; GitHub Pages now serves them directly under `/`.
Copy the normalized repository spelling from the generated selector URL;
query matching is exact. `document` has no effect on a single-file tab.
The `repo` parameter does not bind a local document to that repository.
Selections restore after refresh once the client initializes; the static
HTML initially contains Overview.

Use the heading links generated by the page rather than composing fragments.
A recognized scoped heading fragment selects its tab even if `view` conflicts;
a matching child-document scope also wins over a conflicting `document`.
Single-file anchors use the tab ID; grouped anchors also contain the child ID.
Custom heading IDs use the first discovered repository as their namespace.
Switching between single-file and grouped configuration, changing headings,
or changing that first repository can therefore change anchors.
File moves alone do not change them.

## Build, Preview, and Common Errors

Local tabs are embedded at build time. Restart `npm start` after changing their
files or declarations: tab source discovery runs when configuration loads.
`npm run build` picks up current source text and invalidates affected MDX caches.
No repository sync is needed for local tabs. Upstream GitHub document changes
require a sync and deployment; they are not fetched live by visitors.

Before submitting:

```sh
npm test
npm run build
git diff --check
```

| Symptom | Check |
| --- | --- |
| New introduction missing from navigation | Use a file under `docs/notes/`; check its course and term directories |
| Tab file exists but no tab appears | Add its declaration to the introduction's `repositoryTabs` |
| Duplicate route error | Check page `slug` and category `link.slug`; never reuse a legacy slug |
| `repositoryTabs must be an array` | Use a YAML list, not an object |
| `Invalid repositoryTabs entry` | Check IDs, labels, exact file paths, extension, group nesting, and mutually exclusive fields |
| `repositoryTabs requires a page linking to a GitHub repository` | Add a real GitHub repository link in the introduction body |
| `Invalid course outline` | Check PDF source fields or the legacy JSON structure |
| `Invalid LaTeX TOC` | Check nonempty compiler output and supported title macros |
| Preview still shows old tab content | Restart the development server |
| Repository selector does not change a local TOC | Expected: local content is page-wide, not repository-bound; use separate note-set introductions |

## Implementation Map

Internal modules and regression tests live exclusively in the application
repository. See its
[code map](https://github.com/westlake-drawer/westlake-drawer.github.io#code-map)
and [engine contribution guide](https://github.com/westlake-drawer/westlake-drawer.github.io/blob/main/CONTRIBUTING.md).
They are not a supported external SDK; content contributors only need the
directory and frontmatter contracts above.

`docusaurus.config.js` wires these adapters together. `sources` is the loader's
path-to-text map, `baseUrl` is the deployment prefix, and `sourceBase` is the
GitHub blob URL prefix for source links. Keep source discovery in remark options
so production caches notice local tab edits. Do not register tab files as
standalone docs or reverse the repository-discovery/tab-processing order.

Regression coverage lives in the application's `tests/` directory, including
`docs-layout.test.mjs`, `repository-custom-tabs.test.mjs`,
`repository-document-view.test.mjs`, and `sidebar-stars.test.mjs`.
