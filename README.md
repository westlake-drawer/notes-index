# Westlake Drawer

**[Visit the website](https://westlake-drawer.github.io/)**

This repository contains the **content** for Westlake Drawer: a
subject-organized index of course-note repositories and research tools.
The application, rendering system, tests, and deployment live in
**[westlake-drawer.github.io](https://github.com/westlake-drawer/westlake-drawer.github.io)**.

Browse the website for course descriptions, repository links, and PDF access
instructions. This repository maintains the website, not the original lecture
notes; corrections to those notes belong in their source repositories.

## Project Overview

- **Course Notes:** subject and course pages with automatically generated navigation.
- **Tools:** categorized research tools with repository information.
- **Authoring:** Markdown and MDX, React components, KaTeX formulas, and Mermaid diagrams.
- **Appearance:** an English interface with Fumadocs Glass-inspired surfaces and
  GitHub-style reading density; the initial
  color mode follows the system, with a persistent light/dark toggle.
- **Publishing:** the separate site repository reads this repository, manually
  or daily at 03:23 UTC+8. Content pushes do not deploy automatically.

## Local Development

Content-only contributions need no Node installation. Edit files in:

- `docs/`: introductions and category metadata.
- `content/repository-tabs/`: local tab Markdown and compiler-generated TOCs.
- `static/`: public images and other content assets.

For a local preview, clone the application beside this checkout and use Node.js 22:

```sh
git clone https://github.com/westlake-drawer/westlake-drawer.github.io.git ../westlake-drawer.github.io
cd ../westlake-drawer.github.io
npm ci
NOTES_CONTENT_DIR=../notes-index npm start
```

Open `http://localhost:3000/`.

To check and preview a production build:

```sh
NOTES_CONTENT_DIR=../notes-index npm test
NOTES_CONTENT_DIR=../notes-index npm run build
npm run serve
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for adding course pages, organizing subjects,
writing MDX and formulas, testing changes, and opening pull requests.
See [CONTENT_API.md](CONTENT_API.md) for the directory and content-configuration
reference, including custom tabs and shareable URL parameters.

Course details are maintained on the website rather than duplicated here.
For theme, React components, synchronization, or deployment changes, contribute
to the application repository instead.
