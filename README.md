# Production Agent Guide Docs

An assessment project with a Thally documentation site and a companion article about building and publishing that site.

## Deliverables

- **Published documentation source:** the Thally project at the repository root.
- **Task 1:** product introduction, package quickstart, and harness customization guide in `src/content/task-1/`.
- **Task 2:** canonical Markdown article at [`task-2/article.md`](task-2/article.md), with the rendered MDX page at `src/content/task-2/article.mdx`.
- **Build observations:** [`task-2/observations.md`](task-2/observations.md).

Product facts are based on the remote [`toybz-creator/agent-guide` main branch](https://github.com/toybz-creator/agent-guide/tree/main) at `3e53591b8e63dbea273ab324b497b0c6bdc31112` and the published [`production-agent-guide@0.2.1` package](https://www.npmjs.com/package/production-agent-guide). The local checkout was not used as a source.

## Run the documentation site

Requirements: Node.js 18 or later and npm.

```bash
npm install
npm run dev
```

Use the URL printed by the development server. To check and build the project:

```bash
npm run check -- --ci
npm run typecheck
npm run build
```

The project uses Thally’s default theme and site runtime. Page content lives in `src/content/`, navigation in `docs.json`, and site identity in `src/data/site.ts`.
