# Build and publish a focused Thally documentation site

Let’s build a documentation site together. We’ll use Thally to publish a small, practical guide for [Production Agent Guide](https://github.com/toybz-creator/agent-guide), a developer tool that helps coding agents learn a project’s rules, plan implementation work, verify changes, and keep useful project context close to the code.

The goal was simple: a new developer should be able to understand what the guide does, install it, and know what to customize next. I’ll walk through how we turned that goal into a live site, why we chose each Thally feature, and what we left for a later version. You can follow the same path for another developer tool.

## Before you begin

Have these ready:

| Requirement | Why it helps |
| --- | --- |
| A Thally account | Creates and publishes the documentation site. |
| Git and a GitHub account | Stores the docs source and connects it to Thally Cloud. |
| Node.js and npm | Runs the Thally scaffold, local checks, and preview. Use versions supported by the current scaffold. |
| A short outline of your reader’s first task | Keeps the first release focused on a useful journey instead of a feature inventory. |

Decide whether the source repository can be public before you connect it. Keep credentials, customer data, and unpublished product details out of public repositories.

## 1. Start with the reader’s first win

Before choosing a theme or writing pages, ask: what should a new reader be able to do after five minutes?

For this guide, the answer was: recognize whether Production Agent Guide fits their project, install and activate it, run its setup checks, and then tailor the harness to their team. That gave us a natural page sequence:

| Reader question | Page we wrote | First useful outcome |
| --- | --- | --- |
| What is Production Agent Guide? | Introduction | Understand who the package helps and where to start. |
| How do I try it in my project? | Install and activate the guide | Install the package, add its agent instructions, initialize the harness, and run the doctor. |
| How do I adapt it to my team? | Customize the project harness | Record project decisions, synchronize context, and review task evidence. |
| How was this documentation site made? | This article | Follow the Thally workflow behind the example site. |

That reader journey is the structure of the site. A small outline like this helps you resist the temptation to copy every starter page or document every feature in the first release.

~~~mermaid
flowchart LR
  A[Meet the product] --> B[Install and activate]
  B --> C[Customize project context]
  C --> D[Learn how the docs site was built]
~~~

## 2. Choose a Thally setup path that fits your access needs

In the [Cloud-first quickstart](https://docs.thally.io/quickstart) we followed, Thally recommended the shortest route: start in the dashboard, create a repository, and grant the GitHub App access to **All repositories**. That is convenient when the scope fits your account. For this assessment, I wanted the app’s write access selected for one documentation repository, so I scaffolded locally and connected that existing repository in Thally Cloud.

There’s one permission detail to check carefully: the GitHub installation screen stated that public repositories remain readable by the app even when write access is selected for one repository. A one-repository selection limited write access; it did not remove read-only access to other public repositories. Review the current permission summary before you approve the connection.

## 3. Scaffold the site and connect its source

From the parent directory where you want your docs project, run Thally’s supported generator:

~~~bash
npx --yes create-thally-docs@latest production-agent-guide-docs --yes --install
cd production-agent-guide-docs
~~~

The scaffold gives you a working site, installs its dependencies, initializes Git, and shows where the content and configuration live. For this assessment, the source repository is [toybz-creator/production-agent-guide-thally-assessment](https://github.com/toybz-creator/production-agent-guide-thally-assessment).

If you need to create a new public repository for your own project, review its contents first, then adapt this command to your GitHub handle:

~~~bash
gh repo create your-handle/production-agent-guide-docs \
  --public \
  --source=. \
  --remote=origin \
  --push
~~~

Use a private repository when the content requires it and your Thally plan supports connecting it. The important part is to decide on source visibility and app access before publishing.

## 4. Make the navigation tell the same story

The scaffold’s content lives in `src/content/`. Site identity and external links are in `src/data/site.ts`; page order and grouping are in `docs.json`. We kept the built-in theme and used a short primary action that takes readers straight to the quickstart.

Here is the core of the navigation we used:

~~~json
{
  "theme": "default",
  "markdown": { "enabled": true },
  "navbar": {
    "primary": {
      "label": "Install the guide",
      "href": "/task-1/quickstart"
    }
  },
  "tabs": [
    {
      "tab": "Production Agent Guide",
      "groups": [
        {
          "group": "Start here",
          "pages": [
            "introduction",
            "task-1/quickstart",
            "task-1/practical-guide"
          ]
        },
        {
          "group": "Build this documentation site",
          "pages": ["task-2/article"]
        }
      ]
    }
  ]
}
~~~

Page IDs are paths below `src/content/` without the `.mdx` suffix. After you edit the navigation, compare every page ID with a real file. That simple check prevents orphaned pages and links that lead nowhere.

## 5. Write one page for each reader need

The site uses four MDX pages:

~~~text
src/content/
├── introduction.mdx
├── task-1/
│   ├── quickstart.mdx
│   └── practical-guide.mdx
└── task-2/
    └── article.mdx
~~~

Each page starts with a title and description in YAML frontmatter. For example:

~~~mdx
---
title: Install and activate the guide
description: Add Production Agent Guide to a project, create its harness, and verify the setup.
---

Start from your project root. If the project has no package.json yet, create one with npm init -y.
~~~

The quickstart then moves from installation to activation, initialization, and verification. It tells readers what to expect from `npx pag doctor`, and explains that the command checks the harness files rather than running their application’s tests. The practical guide follows naturally: it shows which project decisions belong in the harness and how the activated agent creates or resumes an implementation guide for each task.

Thally also provides a native Mermaid component for diagrams in MDX. We used it to show a workflow in the published article:

~~~mdx
<Mermaid>{`
flowchart LR
  A[Outline the reader journey] --> B[Write MDX pages]
  B --> C[Check and preview]
  C --> D[Connect the repository]
  D --> E[Publish with Thally]
`}</Mermaid>
~~~

In the standalone Markdown article, use a fenced `mermaid` block instead. That keeps the source readable in GitHub and the diagram renderable in Thally.

## 6. Check the site before you publish

Run the Thally content checker, the TypeScript check, and the production build:

~~~bash
npx --yes @thallylabs/cli@latest check --ci .
npm run typecheck
npm run build
~~~

Then start the local preview:

~~~bash
npm run dev
~~~

Open the URL printed by the dev server and walk through the site as a first-time reader. Check the introduction, quickstart, practical guide, and this article. Follow the navigation, copy the commands, test the internal and external links, inspect the Mermaid diagram, and check both a wide desktop view and a narrow mobile view.

The checker caught content and navigation issues before publication. The production build completed, though it also printed a skipped embeddings step and a deprecated Next.js middleware notice. Neither blocked this docs build; I recorded both in the observations rather than treating them as silent success.

## 7. Connect, publish, and check the live reader experience

In Thally Cloud, create a site and connect the repository you just pushed. For this assessment I selected the docs repository, skipped Product sources and Knowledge surfaces, reviewed the public pages, and published to the Thally-hosted `.thally.app` address. We did not need a custom domain for a focused assessment site.

For later edits, push the reviewed change to the connected branch and watch **Deployments** in Thally Cloud. In this project, each push produced a production deployment; the dashboard showed its source commit and whether the site was healthy.

After publishing, I used Thally’s **Agent readiness** report as one more check. On October 25, it analyzed all four published pages and showed 100/100 with no pages flagged. It confirmed the page descriptions, internal links, structure, code language labels, structured data, discovery, and JSON/Markdown/JSON-LD outputs. Four of its 13 checks were not evaluated because this guide has no OpenAPI specification, API-reference pages, `lastVerified` metadata, or configured golden questions. Readiness is useful feedback, but still open the public site yourself and read the instructions as a developer would.

## Features we used—and what we saved for later

You don’t need to turn on every option to get a useful first release. Here’s how we approached Thally’s feature set:

| Feature | How it fit this build |
| --- | --- |
| MDX pages, frontmatter, and `docs.json` navigation | We used them to shape the four-page reader journey and keep every page reachable. |
| Built-in theme and site navigation | We kept the default theme and focused on clear page order instead of spending time on visual customization. |
| Native Mermaid, code blocks, and links | We used the Mermaid component for a workflow diagram and ordinary code examples for commands. |
| Built-in search, page actions, and machine-readable formats | We kept Thally’s search and page actions, left Markdown output enabled, and used the readiness report to confirm JSON, Markdown, and JSON-LD representations for the published pages. |
| Thally Cloud hosting and deployments | We connected the GitHub source and checked each production publish against its commit. |
| Agent readiness | We used its report to review page quality, links, discovery, and machine readability after publishing. |
| Track and Product sources | We did not configure these because the first release was hand-authored documentation, not a workflow that turns product code changes into proposed docs updates. |
| Knowledge surfaces | We skipped these because one public documentation site was enough for this audience. |
| Custom domain, analytics, and Reader questions | We left them for later: the Thally-hosted address worked, and the assessment did not need traffic reporting or a support-feedback loop. |
| OpenAPI documentation | We did not add it because Production Agent Guide is a developer workflow guide, not an API reference, and there was no OpenAPI specification to publish. |

Those choices are reversible. Add a product-source workflow when code changes need a documentation review loop; add another surface when a different audience needs a different experience.

## What got in the way—and how we recovered

The Cloud-first path’s broad GitHub grant did not fit the repository access choice for this assessment. We used the CLI scaffold and connected the existing repository instead. The install screen still disclosed read-only access to public repositories, which is useful context for anyone reviewing the integration.

The first return from GitHub failed because `app.thally.io` did not resolve. Reopening setup and using **Check GitHub** with the already-authorized account recovered the connection; the repository picker then listed the assessment repo. If the callback fails for you, retry the session check before creating another app installation.

The scaffold also included generic sample pages and navigation. We removed the pages we did not need and updated `docs.json` to match the four pages we wrote. During the product quickstart check, `npx pag help` did not list the README-documented `pag task` command even though it worked with a date-prefixed ID. Because the package’s activated instructions tell the agent to create or resume an implementation guide before each task, we removed that optional manual command from the user-facing quickstart and practical guide.

## Troubleshooting

| Symptom | What to check | Next step |
| --- | --- | --- |
| A page is missing from navigation | Its path may be missing or misspelled in `docs.json`. | Match the page ID to a file under `src/content/`, without the `.mdx` suffix. |
| The checker reports an orphan page | A content file is not listed in navigation. | Add it to `docs.json` or remove the unused sample page. |
| Mermaid displays as code or fails to compile | The MDX component wrapper or template literal may be malformed. | Compare it with the native Mermaid component example above and check the opening and closing template-literal marks. |
| A pushed edit is not live yet | The commit may not have reached the connected branch, or its deployment may still be running. | Check the source commit and deployment status in Thally Cloud. |
| GitHub returns to an error page | The installation callback or account session may have expired. | Reopen setup and use **Check GitHub** before creating another installation. |

## Further reading

- [Thally quickstart](https://docs.thally.io/quickstart)
- [Thally project structure](https://docs.thally.io/guides/project-structure)
- [Thally writing content](https://docs.thally.io/guides/writing-content)
- [Thally navigation](https://docs.thally.io/guides/configuring-navigation)
- [Thally branding and theming](https://docs.thally.io/guides/branding-and-theming)
- [Thally Mermaid component](https://docs.thally.io/components/mermaid)
- [Thally CI checks](https://docs.thally.io/guides/ci-checks)
- [Production Agent Guide README](https://github.com/toybz-creator/agent-guide/blob/main/README.md)
- [Production Agent Guide source](https://github.com/toybz-creator/agent-guide/tree/main)
