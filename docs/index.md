# **GitHub SDLC Automation with IBM Bob**

---

## **i. GitHub is the connective tissue of modern software delivery**

![type:video](./_videos/1 Introduction [Bob SDLC L3].mp4)
!!! note ""
    Christopher Bienko *(AI Enablement GTM Product Manager)* introduces the *GitHub SDLC Automation with IBM Bob* curriculum for hands-on enablement. **<a href="https://ibm.seismic.com/Link/Content/DCpPWJGchFcJ8GMMD7PgdcTb6JHP" target="_blank">[6 min]</a>**

For the last decade, GitHub has become the connective tissue of modern software engineering. *Issues* capture requirements, *pull requests* carry code review, *Actions* drive continuous integration & continuous delivery (CI/CD), and *branches* organize the work in-between.

!!! success ""
    **Typical GitHub-centered workloads include:**

    - Web and mobile front-end applications
    - Backend services and REST APIs
    - Data pipelines and analytics tooling
    - Infrastructure-as-code and platform engineering
    - Internal developer tools and dashboards
    - Open-source libraries and SDKs

!!! failure ""
    **Despite the maturity of the tooling, most teams still feel the same friction:**

    - Requirements buried in long issue threads
    - Time-consuming code reviews that block delivery
    - Inconsistent PR descriptions and commit hygiene
    - Context switching between the IDE, the browser, and the terminal
    - Uneven test coverage and validation habits
    - A widening gap between what a codebase does and what its docs say

Whether a team is shipping a mobile app, a data platform, or an internal microservice, the workflow tends to look the same: an issue is opened, a branch is created, code is written, tests run, a pull request is reviewed, and — eventually — something is merged and deployed.

It is *precisely* this friction that IBM Bob was designed to relieve.

---

## **ii. What is IBM Bob?**

!!! warning "RECOMMENDED PREREQUISITE: *CLIMBING THE AI MATURITY CURVE*"
    This course assumes you have completed the <a href="https://ibm.github.io/bob-l3/" target="_blank">**Climbing the AI Maturity Curve with IBM Bob**</a> Level 3 curriculum, and it assumes those concepts are familiar ground: the Agentic Sidebar and its modes, the plan-first delegation discipline, Bob Rules, and the context economics that govern every session. The material ahead builds directly upon that foundation— the discipline you practiced there is precisely what this lab applies to the GitHub delivery loop.

**IBM Bob is an AI-powered software engineering partner that supports the entire software development lifecycle (*SDLC*).** Where a traditional code-completion tool is anchored to the current line, Bob is designed to participate in the actual delivery loop — reading a repository, understanding a GitHub issue, implementing a targeted change, running tests, reviewing its own diff, and preparing a merge-ready pull request.

For GitHub-centered teams, Bob can:

- Analyze a repository's structure and propose an implementation plan
- Scaffold a new application from a description of the target outcome
- Integrate external APIs and data sources
- Generate UI components, service layers, and tests
- Run project validation and interpret the failures
- Summarize `git diff` output into clean commit and PR descriptions
- Fetch and interpret GitHub issues as scoped work items
- Perform code review with issue-coverage checks
- Prepare branches for merge or automated closure

Bob operates inside a VS Code-based IDE with direct access to your Git working tree and GitHub context. Every conversation is grounded in the current state of the repository — the branch, the diff, the open issues, the CI results.

---

## **iii. The environment**

The application used throughout this workshop is a **React-based finance dashboard** that tracks IBM and a small set of major competitors — Microsoft, Oracle, SAP, and Salesforce — using publicly available market data from Yahoo Finance.

The app is intentionally small enough to build in one sitting and realistic enough to demonstrate the full delivery loop. It contains the pieces most modern web applications rely on:

| Component | Responsibility |
|-----------|----------------|
| Dashboard page | Top-level layout for the three time-window views |
| Chart cards | Reusable components for current day, 7-day, and quarterly trends |
| Finance data service | Yahoo Finance integration with response normalization |
| Company registry | Configurable set of tracked ticker symbols |
| Validation suite | Unit tests for data transforms and one rendering path |
| GitHub workflow | CI pipeline for lint, test, and build on every push |

Together, these components exercise the patterns that dominate real-world engineering:

- Component-oriented React architecture
- Service-layer separation between data and UI
- External API integration with resilience to missing or partial data
- Chart-based data visualization
- Unit and rendering tests
- GitHub-driven CI/CD

Although the app is small, it drives the same delivery workflow that larger teams use every day.

---

## **iv. Objectives and outline**

Modern engineering teams rarely struggle to write code. The struggle concentrates in the work *around* the code— understanding requirements, reviewing diffs, writing PR descriptions, keeping tests current, coordinating across issues —which consumes a disproportionate share of every sprint.

!!! warning ""
    **The hardest questions are usually:**

    - What does this issue actually ask for?
    - Which files need to change?
    - What could this break?
    - Is the test coverage still meaningful?
    - Does the PR description reflect what was actually done?
    - Is this branch really ready to merge?

!!! note ""
    **IBM Bob accelerates these activities by providing:**

    - Repository-grounded analysis of the actual codebase
    - Direct access to Git and GitHub context
    - Guided workflows for scaffolding, review, and PR preparation
    - Auto-generated commit messages and PR descriptions from real diffs
    - Issue-coverage review to confirm a change matches its request
    - Guardrails that keep the developer in control at every step

    All while keeping the engineer firmly in charge of what ships.

This workshop follows a realistic GitHub-driven delivery workflow. Rather than generating isolated snippets, you will use IBM Bob to perform the same activities engineering teams do every day — planning, building, testing, reviewing, and evolving an application through GitHub.

| Part | Focus | What You'll Learn |
|------|-------|-------------------|
| **<a href="https://bienko.github.io/bob-sdlc/build-1/" target="_blank">Part 1: Build</a>** | Scaffold and deliver the initial application | Use Bob to interpret the project goal, scaffold a React app, integrate Yahoo Finance data, build the dashboard views, add tests, and prepare a review-ready commit |
| **<a href="https://bienko.github.io/bob-sdlc/evolve-1/" target="_blank">Part 2: Evolve</a>** | Deliver enhancements through GitHub | Push the app to GitHub, open an issue for a new feature, let Bob analyze the request, implement the change, validate it, and prepare a merge-ready pull request |

!!! note ""
    Take note of the shape of this journey and how it relates to the topics covered within the <a href="https://ibm.github.io/bob-l3/" target="_blank">**Climbing the AI Maturity Curve with IBM Bob**</a> Level 3 curriculum.
    
    - **<a href="https://bienko.github.io/bob-sdlc/build-1/" target="_blank">Part 1 | Build</a>** is the plan-first delegation rhythm of **<a href="https://ibm.github.io/bob-l3/delegate/2-1/" target="_blank">Module 2 | Delegate</a>** applied at project scale — an implementation plan is agreed, then approved sub-task by sub-task, with Bob working autonomously between the gates.
    - **<a href="https://bienko.github.io/bob-sdlc/evolve-1/" target="_blank">Part 2 | Evolve</a>** operates at the far right of the curve, **<a href="https://ibm.github.io/bob-l3/scale/4-1/" target="_blank">Module 4 | Scale & Extend</a>** — Bob's reach extends outward into GitHub itself (issues, CI, pull requests), while your control concentrates at the moments that matter most: the requirements gate, the push, and the merge. The instrument is unchanged from the previous course; what changes here is that the delivery loop itself becomes the workbench.

Each part builds on the previous one, demonstrating how AI can assist across the complete SDLC — from an empty repository to an application that ships changes through pull requests. By the end of the hands-on lab, you will have a working React finance dashboard backed by tests, a CI pipeline, and a GitHub-driven evolution loop.

---

## **v. Next steps**

In **<a href="https://bienko.github.io/bob-sdlc/build-1/" target="_blank">Part 1 | Build</a>**, you will build the initial finance dashboard from an empty repository. You'll use Bob to interpret the goal, scaffold the app, integrate Yahoo Finance data, build the dashboards, add tests, and prepare a clean set of Git changes.

In **<a href="https://bienko.github.io/bob-sdlc/evolve-1/" target="_blank">Part 2 | Evolve</a>**, you will push the app to GitHub and let Bob take over the evolution loop — fetching an issue, implementing a user-selected company chart, validating the result, and preparing a merge-ready pull request.

By the end, you will have seen IBM Bob participate in every stage of a modern software delivery workflow — an engineering partner operating inside the GitHub-centered delivery loop that most teams already use.

---