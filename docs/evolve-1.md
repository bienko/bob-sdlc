**PART 2 | EVOLVE**
# **PUBLISHING TO GITHUB**

---

## **i. Why this matters**

![type:video](./_videos/5 Publishing to GitHub [Bob SDLC L3].mp4)
!!! note ""
    Christopher Bienko *(AI Enablement GTM Product Manager)* provides an overview of the *Publishing to GitHub* module for the hands-on enablement. **<a href="https://ibm.seismic.com/Link/Content/DCRBWBdmffDJM89PR7MqDd6X22Gj" target="_blank">[2 min]</a>**

The finance dashboard from **<a href="https://bienko.github.io/bob-sdlc/build-1/" target="_blank">Part 1 | Build</a>** runs, passes its validation gates, and is ready to share. But it still lives only inside your IDE workspace. In fact, the workspace is not yet even a git repository — initialization has never occurred, not a single commit exists, and no `.gitignore` is present to keep dependencies and build artifacts out of version control. Until the code reaches a shared repository, there is no collaborative workspace for *issues* to capture requirements, for *pull* requests to carry review, or for continuous integration (CI) to guard the `main` branch.

Modern delivery treats GitHub as that workspace. An issue is opened, a branch is created, code is written, tests run, a pull request is reviewed, and something is merged. *Part 2* of this hands-on lab places IBM Bob inside that loop. Before Bob can fetch an issue or prepare a pull request, however, the repository has to exist in two places: locally as an initialized git repository, and on GitHub.com as the shared remote.

!!! warning "PUBLISH TO YOUR *OWN* GITHUB REPOSITORIES"
    A **personal** GitHub.com account is required for this module and the rest of **Part 2 | Evolve**. 
    
    If your personal GitHub account is already linked to the IBM GitHub enterprise account, you can still publish code to repositories under your own (*personal*) account. However, you will encounter restrictions on what you can commit and publish to the *IBM GitHub* repositories — as only approved contributors are able to commit to such endpoints.

    To publish code for the purposes of **Part 2 | Evolve**, you will need to ensure that code is committed via your personal repositories, *not* the IBM repositories. The repository can be public or private. Bob's workflow is identical either way.

In this module, you will:

- **Create the GitHub repository**, so the remote exists before anything is pushed to it
- **Let Bob inspect the workspace** and surface anything blocking a clean push
- **Initialize the local repository**, putting the *Part 1* code under version control
- **Publish the code**, using the commands Bob prepares
- **Enable the collaboration features** the rest of *Part 2* depends on
- **Open the issue** that drives the next enhancement

Pay attention to how Bob participates even in this administrative phase. It inspects the working tree, catches blockers like a missing `.gitignore`, and prepares the exact commands, while you retain control of the actual push.

!!! note "AI MATURITY CURVE"
    This module applies the **Scale & Extend** stage of the AI Maturity Curve to the delivery platform itself. Bob's reach now extends beyond the IDE — inspecting the working tree, surfacing blockers, and preparing the exact commands — while the one action that publishes the work, the push, remains yours to perform. As you learned throughout the previous course, extending an agent's reach and retaining control over consequential actions are practiced together, and this chapter asks you to do exactly that.

## **ii. Publishing to GitHub**

You will move through this module in four phases, in this order:

1. **Create the empty repository on GitHub.com** (steps 1 and 2), so the remote URL exists before anything else.
2. **Let Bob check readiness and produce the delivery artifacts** (steps 3 and 4).
3. **Initialize the local repository and push** (steps 5 through 7).
4. **Verify and stage the collaboration surface** (steps 8 through 10): confirm the publish, enable Issues, create the feature branch, and open the enhancement issue the next module builds on.

---

1. Create the repository on GitHub first, so you have a remote URL to push to later. In your browser, go to GitHub and click **New** next to **Top repositories** on the dashboard, or navigate directly to <a href="https://github.com/new" target="_blank">**https://github.com/new**</a>.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/4dcb6e92-1449-4c2d-ac7e-646aced7431c/user_cropped_screenshot_47d399de264a4c4cad851d950d1db368_text_export.jpeg" alt="" width="600" />
    </p>

    On the **Create a new repository** form, click the **Repository name** field and enter `finance-app` (or your preferred name).

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/7fe08bec-6011-40c1-943b-9b56571a03aa/user_cropped_screenshot_b0ca728d9dcb4b11b8480cca43be9cca_text_export.jpeg" alt="" width="600" />
    </p>

    In the **Description** field, paste `IBM Bob Workshop Finance Dashboard Application`.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/cd45f3b2-aa41-4aa2-a6b4-a35e12199272/user_cropped_screenshot_2408034f870d4cdc8caaa04f5b712199_text_export.jpeg" alt="" width="600" />
    </p>

    Under **Configuration**, select a visibility. Public or Private is your choice; **Private** was used here.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/aa040b63-4705-4555-abdc-4e5c15369876/user_cropped_screenshot_9a106706675540b4a74711aee3143882_text_export.jpeg" alt="" width="600" />
    </p>

    Confirm **Add README** is toggled `Off`, and leave **Add .gitignore** on `No .gitignore` and **Add license** on `No license`. The repository must stay empty because you are pushing existing code. Then click **Create repository**.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/957dad25-325e-41bb-903d-da4a6071ca7b/user_cropped_screenshot_98e3fe08b08e499386612dffa3ebb7df_text_export.jpeg" alt="" width="600" />
    </p>

---

2. GitHub will land on the **Quick setup** page showing the repository's remote URL, in the form `https://github.com/YOUR_USERNAME/finance-app.git`

    **Copy and store it** within a notepad for now, as you will later need to paste it into a git command in *Step 6*.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/9c3f4764-1d8b-401a-aae8-10050b245f5d/user_cropped_screenshot_e6c33946a48547feb9da4a4565ecf59b_text_export.jpeg" alt="" width="600" />
    </p>

---

3. Now let Bob assess the workspace. Back in the Bob IDE, with **Agent mode** active, copy and paste the following prompt into the Agentic Sidebar:

    ```
    Inspect the current repository changes, summarize the current implementation status of the finance dashboard, and identify whether the project is ready to be committed and pushed to GitHub.
    ```

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/87d8e612-b70b-4950-977f-1571d98d7d57/user_cropped_screenshot_7562ed9ef9054324a3a784382b6c4774_text_export.jpeg" alt="" width="600" />
    </p>

    Hit ++return++ or press the **Send** button to submit the task.

    Bob will begin calling tools to inspect the repository, and will pause for approval before the first one. The command it proposes is read-only, a look at recent history such as `git log --oneline -10`. Click **Approve once** to let it run.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/c801d246-89fc-4189-aa1a-ea6621f13e8d/user_cropped_screenshot_31806220e19048c38cdfedcebc8e2d00_text_export.jpeg" alt="" width="600" />
    </p>

    Bob works through the **Todo List** pinned at the top of the session as it inspects. This is the same validation Todo List Bob completed at the end of **<a href="https://bienko.github.io/bob-sdlc/build-1/" target="_blank">Part 1 | Build</a>**, now showing **6/6** with every item green, and it is the baseline Bob is assessing for delivery readiness:

    | TASK | OBJECTIVES |
    | ---- | ---------- |
    | **Sub-Task 1** | Fix the `dev:mock` script environment variable (`USE_MOCK` → `VITE_USE_MOCK`) and add the `validate` and `test:coverage` scripts |
    | **Sub-Task 2** | Extend `formatters.test.js` with `formatVolume`, `formatIndex`, and `formatAxisDate` suites |
    | **Sub-Task 3** | Add `PerformanceBadge.test.jsx` covering rendering and the positive, negative, and null states |
    | **Sub-Task 4** | Add `RankingStrip.test.jsx` covering sort order, all symbols rendered, null-percentage safety, and price display |
    | **Sub-Task 5** | Add `WeekChangeSummary.test.jsx` covering the chip per symbol, start and end prices, and the empty guard |
    | **Sub-Task 6** | Run the full validation suite and confirm all checks pass |

    Bob reruns the validation suite against the code on disk, explores the source folders, then checks what git would exclude. It finds **no root-level `.gitignore`**, meaning `node_modules/`, `dist/`, and `.DS_Store` would all be committed, and may try to create the file itself.

    Rather than approving each remaining inspection command one at a time, click **Approve for task** so Bob can finish the assessment without stopping at every read-only command.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/7d6af821-e14e-40c8-9a1a-e90bb1f7e367/user_cropped_screenshot_cf14302fa0874260a137f57a9ecb81a7_text_export.jpeg" alt="" width="600" />
    </p>

    !!! warning "IF BOB CANNOT WRITE THE `.gitignore`"
        Bob may report `Failed to write file .gitignore` because the workspace's own protection blocks writes to a root-level gitignore. This is not a problem. *Step 5* below creates the file from the terminal instead.

    Bob will inspect the workspace and report two kinds of findings:

    !!! quote ""
        - **Code readiness**: Bob reruns the validation gates and reports the results, for example `npm run lint` with zero errors, the full test suite passing, and a clean build.
        - **Delivery blockers**: expect Bob to point out that the workspace is **not yet a git repository** (commands like `git status` return `fatal: not a git repository`) and that **no `.gitignore` exists**, which means `node_modules/` (thousands of files) and `dist/` would be committed. This is normal. Part 1 never initialized version control, and steps 5 through 7 below fix both findings.

    When the assessment finishes, Bob produces the delivery artifact: a numbered, copy-paste sequence of the exact git commands for the push, ending with a staging check, the commit, and the push itself.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/294a6452-a084-4ea6-91b4-2f0ca9e860f3/sps-redacted-92feaab666634cde8c2bd3ea56f2af4d_8746d22528be499eaa62347a191d1d7d_text_export.jpeg" alt="" width="600" />
    </p>

    Read the tail of that artifact carefully, because the next steps reuse it:

    - A **staging check** (`git status` and `git diff --staged --stat`) with an explicit reminder that `node_modules` and `dist` must **not** appear in the staged set
    - A **commit message** in conventional-commit form, for example `feat: add initial React finance dashboard application`, with a body covering the five tracked companies, the three time-window views, the data layer, the passing test count, and the CI workflow
    - The **push command**, `git push -u origin <branch>`
    - A closing note that once the push lands, the CI workflow under `.github/workflows/` runs automatically: it installs dependencies, lints, runs the test suite with `VITE_USE_MOCK=true`, builds, and uploads `dist/` as a workflow artifact

---

4. Ask Bob for the delivery artifacts you will use during the push and later on GitHub. Copy and paste the following prompt into the **Agentic Sidebar**, then hit ++enter++ to execute the instructions:

    ```
    Generate a concise summary of the finance dashboard implementation, propose a commit message, and prepare a pull request description that explains the new application, dashboards, and validation steps.
    ```
    
    Bob will pause for approval before its first tool call. The command is read-only, a `find` over `finance-app/src` that lists the source files so the summary is grounded in what is actually on disk. Click **Approve once**.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/1d6a34fe-883c-4df7-87d3-c040803e7ce9/user_cropped_screenshot_353fb38a6dd443d5892213c6a90762e4_text_export.jpeg" alt="" width="600" />
    </p>

    Bob reruns the validation chain, reports **Live validation confirmed**, and produces the three deliverables as a single self-contained HTML report, the same way it produced the implementation plan in Part 1. Alongside the report, Bob posts a plain-text summary in the sidebar so you can read the essentials without opening anything.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/b9613150-d440-4b11-b2c5-4c489880577e/sps-redacted-1a3cad87a31f45d8b8b6e5f4753da591_91bb709dac4649278215438c8a41bdc8_text_export.jpeg" alt="" width="600" />
    </p>

    The sidebar summary describes what was built in one paragraph, then quantifies it under **By the numbers**: 47 source files in `src/`, 2 server files, 2 GitHub delivery artifacts, **114 tests passing** across 7 suites (75 unit plus 39 component render), a production build producing 7 split chunks, and `npm run validate` exiting 0.

    It then tells you where each artifact goes, which is the part that matters for the rest of this module:

    | ARTIFACT | WHERE IT GOES |
    | -------- | ------------- |
    | **Commit message** (section 2) | Into `git commit -m "..."`. The full multi-line body is already formatted for a standard git commit |
    | **PR description** (section 3) | Pasted into the GitHub PR body when the pull request is opened from `feature/initial-finance-dashboard` into `main` |

    Bob also notes that once the branch is pushed, the CI workflow at `.github/workflows/finance-app-ci.yml` runs the identical **lint → test → build** chain and uploads the `dist/` folder as a downloadable artifact for reviewers.

    Now click **Open** on the **Finance Dashboard — Commit & PR Package** card to review the full report in the IDE. The report is written to a temporary `bob-artifacts/` directory, so open it before starting a new task.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/a91436e9-faf0-48e7-b55a-8d28832dd292/user_cropped_screenshot_b3c139e00f57450490b2a311a443b1ce_text_export.jpeg" alt="" width="600" />
    </p>

    The report opens with a **Validation live run** panel confirming the three gates Bob just executed against the code on disk: lint reporting **0 errors and 0 warnings**, **every test in the suite passing**, and a **clean production build with no warnings**. Below the panel, the report is organized into numbered sections.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/753f6590-eada-495a-a200-042a9ff1d1b2/user_cropped_screenshot_603e21f4dd514b69a50500a46d26500b_text_export.jpeg" alt="" width="600" />
    </p>
    !!! note ""
        - **1 · Implementation summary**: what the `finance-app/` directory delivers — a complete React 18 market analytics dashboard tracking IBM against Microsoft, Oracle, SAP, and Salesforce, with a lightweight Express proxy isolating the browser from third-party API concerns and an offline mock mode (`VITE_USE_MOCK=true`) that lets the full UI run with no network access, used by the dev server, the tests, and CI.
        - **Three views**: a table mapping each time window to what it shows and the components that render it — **Today** (`RankingStrip`, `SummaryCard`), **Last 7 Days** (`LineChart`, `WeekChangeSummary`), and **Last Quarter** (`AreaChart` in base-100 index mode, so IBM at ~$175 and MSFT at ~$415 sit on the same Y axis).
        - **Source file inventory**: a layer-by-layer table of config, constants, utilities, data service, state, components, and tests, naming the files in each layer and the responsibility it carries.
        - **2 · Commit message** and **3 · Pull request description**: the two artifacts you carry forward.
    !!! warning "YOUR NUMBERS WILL DIFFER"
        Those three green results are the substance of the report. The run shown here reports 114 tests across 7 suites, but the exact counts and timings depend on what **<a href="https://bienko.github.io/bob-sdlc/build-1/" target="_blank">Part 1 | Build</a>** produced on your machine and are not something to match.

    Keep both artifacts handy: the commit message is used in *Step 7*, and the PR description is used when a pull request is opened on GitHub.

---

5. Time to fix the blockers Bob found. Open a new Terminal in the IDE (**Terminal → New Terminal**) and create the `.gitignore` at the workspace root, so dependencies and build artifacts never enter version control:

    ```
    cat > .gitignore <<'EOF'
    node_modules/
    dist/
    .DS_Store
    .env
    coverage/
    EOF
    ```

---

6. You will next need to execute the complete code block ***after*** making modifications.

    - Copy and paste the code block below to a notepad for editing
    - Retrieve the GitHub **repository remote URL** that you recorded earlier in *Step 1*
    - Substitute the `<REMOTE_URL>` placeholder token with your *repository remote URL*
    - Copy and paste the modified code block into the **Terminal** console and press ++return++ to execute

    ``` hl_lines="5"
    # Initialize the repository
    git init

    # Connect the GitHub remote
    git remote add origin <REMOTE_URL>

    # Verify the remote was added
    git remote -v
    ```

---

7. Stage, commit, name the default branch, and finally push it. Use the commit message Bob proposed in *Step 4*.

    - Copy and paste the code block below to a notepad for editing
    - Retrieve the **commit message** that IBM Bob recommended earlier in *Step 4*
    - Substitute the `<COMMIT_MESSAGE>` placeholder token with the message from *Step 4*
    - Copy and paste the modified code block into the **Terminal** console and press ++return++ to execute

    ``` hl_lines="8"
    # Stage everything
    git add .

    # IMPORTANT: confirm node_modules and dist do NOT appear in the staged list
    git status

    # Commit using the message Bob proposed
    git commit -m "<COMMIT_MESSAGE>"

    # Name the default branch main
    git branch -M main

    # Push to GitHub
    git push -u origin main
    ```

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/3523adbd-7f17-40fb-bd7a-5ad9fd1cf154/sps-redacted-b35ea47406614f64bcb9ed53ecd455bd_09f2efb979644569814f90c8494e80cd_text_export.jpeg" alt="" width="600" />
    </p>

    The commit output lists every file as `create mode 100644 ...`, so read it and confirm that only source files are being tracked. The push output then reports the objects written and ends with the two lines that confirm the publish succeeded:

    ```
    * [new branch]      main -> main
    Branch 'main' set up to track remote branch 'main' from 'origin'.
    ```

    !!! note "RENAME THE BRANCH *AFTER* THE FIRST COMMIT"
        `git branch -M main` renames the branch you are currently on. Immediately after `git init` there are no commits yet, so that branch does not exist as a reference and the rename fails with `error: refname refs/heads/master not found`. Git 2.30 and later handle this case, but older versions do not, so this workshop renames the branch after the commit. This is also the order GitHub's own **Quick setup** page uses.

        If you have already hit the error, either commit first and then rerun `git branch -M main`, or point `HEAD` at the branch directly with `git symbolic-ref HEAD refs/heads/main`.
    
    ??? warning "TROUBLESHOOTING: AUTHENTICATION ISSUES"
        GitHub no longer accepts account passwords over HTTPS. If the push prompts for credentials, use a **Personal Access Token (PAT)** in place of the password, or configure **SSH keys** and use the SSH remote URL instead. See <a href="https://docs.github.com/en/authentication" target="_blank">GitHub's authentication documentation</a> for both paths.

        If the push is **rejected**, confirm you have write access to the repository, check that no branch protection rule is blocking the push, and verify the remote URL with `git remote -v`.

---

8. Verify the published repository in your browser. Refresh the repository page on GitHub.com and confirm:

    - All files from *Part 1* are visible: the application source, the docs, and the PR template
    - `node_modules/` and `dist/` are **not** in the file tree
    - The **Finance App CI** workflow is listed under the **Actions** tab
    - The pull request template is visible at `.github/PULL_REQUEST_TEMPLATE.md`

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/c2fae477-25cb-4241-b296-1a06844c2804/user_cropped_screenshot_6530b4c345494320ac2257da13f0a92d_text_export.jpeg" alt="" width="600" />
    </p>

    Open the repository from your GitHub dashboard.

    - The repository home page should show a single commit carrying Bob's commit message, on one branch, with the root file tree: `.github/`, `docs/`, `finance-app/`, `.gitignore`, `LICENSE`, `README.md`, and the workshop markdown files.
    - Confirm that `node_modules/` and `dist/` are **not** included within that list (the `.gitignore` you created in *Step 5* is what kept them out)

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/f2d4321d-5aea-4814-a2c4-6f0935837e91/user_cropped_screenshot_6ac6ed0f08684ed8a85490771e726010_text_export.jpeg" alt="" width="600" />
    </p>

    Click into `finance-app/` and check the application directory the same way. You should see `public/`, `server/`, `src/`, and the config files (`.eslintrc.cjs`, `index.html`, `package.json`, `package-lock.json`, `vite.config.js`), with no `node_modules/` and no `dist/`.
    
    !!! warning ""
        Note that `package-lock.json` **is** committed, which is what lets CI run a reproducible `npm ci`.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/a1a5be71-3450-4521-bd42-057be7ea2058/sps-redacted-5459d8837d61490fa6c440aa0494f91a_a7d56e617e7c4565b1363aec488e1080_text_export.jpeg" alt="" width="600" />
    </p>

    !!! note "NO WORKFLOW RUNS YET — AND THAT IS EXPECTED"
        The **Actions** tab will list **Finance App CI** in the left sidebar but report **There are no workflow runs yet**. GitHub detects the workflow by parsing `.github/workflows/finance-app-ci.yml` on the default branch; detecting it is not the same as running it.

        The workflow is deliberately scoped to the delivery paths that matter, and a direct push to `main` is not one of them:

        ```yaml
        on:
          push:
            branches:
              - 'feature/**'
              - 'fix/**'
              - 'chore/**'
          pull_request:
            branches:
              - main
        ```

        The first run arrives in *Step 9*, when the `feature/initial-finance-dashboard` branch is pushed and matches `feature/**`.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/bfde1197-8390-469d-a27c-1a0f0ff468f0/sps-redacted-30991c26e1c849bf8a0a518f72d1bccf_41ef3ace018f4a6f8693ee4cb4f4570a_text_export.jpeg" alt="" width="600" />
    </p>

    Then go to **Settings → General** and scroll to the **Features** section.
        - Confirm the **Issues** checkbox is ticked, since the feature request workflow in the next module depends on it.
        - Leave **Issue permissions** on the default, `Creation allowed by: All users`.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/24571a1b-f293-4bd2-94a6-fbbadce443e2/user_cropped_screenshot_645e6344630e4b11b595417042e21e07_text_export.jpeg" alt="" width="600" />
    </p>

---

9. Create the feature branch for the workshop. Feature work never lands on `main` directly; it travels on its own branch and arrives through a pull request. Back in the IDE terminal:

    ```
    # Create and switch to a new feature branch
    git checkout -b feature/initial-finance-dashboard

    # Push the branch to GitHub
    git push -u origin feature/initial-finance-dashboard
    ```

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-04/bb75ad09-8a37-4ebc-8aa7-9d394d60a2d6/user_cropped_screenshot_6e27c3518bab4a3bbdf3c8e3c55fb3c0_text_export.jpeg" alt="" width="600" />
    </p>

    The terminal confirms the switch with `Switched to a new branch 'feature/initial-finance-dashboard'`. At that point the branch exists only locally; the `git push` that follows is what publishes it.

    This push is what starts CI. The branch name matches the `feature/**` pattern in the workflow trigger, so return to the repository on GitHub and click the **Actions** tab. The repository header should now read **2 Branches**.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/57056eda-de3c-40b4-9144-4fdb1b3c4c42/user_cropped_screenshot_decaaec4bb9d464db42a8e634c91f22f_text_export.jpeg" alt="" width="600" />
    </p>

    Where the Actions tab reported no runs in *Step 8*, it now shows **1 workflow run**, titled with your commit message and labelled `Finance App CI #1`, with the commit hash, your username, and the `feature/initial-finance-dashboard` branch it ran against.
    
    **Click** the run to open it.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/41e35334-dded-4d93-a237-7bae52f00c15/user_cropped_screenshot_5a452bc541734c6b9d4aae7643580504_text_export.jpeg" alt="" width="600" />
    </p>

    The run summary is what this entire module has been building toward. Confirm:

    - **Triggered via push** on `feature/initial-finance-dashboard`, with **Status: Success**
    - The single `Lint → Test → Build` job carrying a green check, completing in well under a minute
    - **Artifacts: 1**, listing `finance-app-dist`, the built `dist/` folder uploaded by the workflow's final step and downloadable by any reviewer

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/2dcc0441-8ce7-47a8-b190-4f4b958863b4/screenshot_c83e514a710743c5a9057fe6960aeda4_text_export.jpeg" alt="" width="600" />
    </p>

    The same gates Bob ran locally in *Step 3* and *Step 4* have now run on a clean machine, against the code as GitHub received it. That is the point of pushing to a branch first: the validation is no longer taking your word for it.

    !!! warning "AN ANNOTATION WARNING IS NOT A FAILURE"
        The run may show **Annotations: 1 warning** reporting that Node.js 20 is deprecated and that `actions/checkout`, `actions/setup-node`, and `actions/upload-artifact` are being forced onto a newer Node.js runtime. This is GitHub deprecating the runtime its own actions run on rather than a problem with the finance dashboard. The job status is still **Success**, and the workshop continues unchanged.

    !!! note "BRANCH NAMING CONVENTIONS"
        A consistent branch naming convention keeps AI assisted work traceable alongside human work. This workshop uses the pattern most teams will recognize:

        - `feature/initial-finance-dashboard` (created just now, the published baseline)
        - `feature/user-selected-company-chart` (created in the next module, carries the enhancement)
        - `fix/chart-data-normalization` (the shape a future bug fix would take)

---

10. Finally, open the issue that drives the next module. Click the **Issues** tab in the repository header, then click **New issue**.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/0d4a618a-e5b0-4f12-86a9-261bb86d5eff/user_cropped_screenshot_9148cfb2bad04ac7a544e4b76febbb63_text_export.jpeg" alt="" width="600" />
    </p>

    On the **Create new issue** form, fill in:

    - **Add a title**: `Add a graph for a user selected company`
    - **Add a description**:

    ```
    Add a new graph based on a company indicated by the user.

    * Allow the dashboard to accept a stock symbol or company selection
    * Render an additional comparison chart for the selected company
    * Invalid or unsupported symbols should produce a clear message to the user
    * The existing dashboards for IBM and its competitors must remain unchanged
    ```
    ??? warning "WHY IS THE BODY WRITTEN THIS WAY?"
        The **description** is deliberately written as an outcome plus four constraints rather than as a set of implementation instructions. It says what the user should be able to do, what must appear, how failure should behave, and what must not regress. That is enough for Bob to plan against, and it leaves the design decisions where they belong. Notice that the last bullet is a guard: it tells Bob what success must *not* break.

    - Leave **Assignees**, **Labels**, **Projects**, and **Milestone** empty (Bob reads the title and body, and nothing else here changes what it does in the next module)
    - When ready to continue, click **Create**

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/9bb1a4c1-6aac-422d-918a-9255c4f14b88/user_cropped_screenshot_10a17b18ea56483f8b4a44e14a1a39b9_text_export.jpeg" alt="" width="600" />
    </p>

    !!! warning ""
        Note the issue number GitHub assigns (typically `#1`). Bob will fetch this issue by number in the next module.

---

## **iii. Verification checklist**

Before moving on, confirm every row. The next two modules assume all of this is in place:

| CHECK | WHAT TO CONFIRM |
| ----- | --------------- |
| GitHub repository created and accessible | Repository page loads at `github.com/YOUR_USERNAME/finance-app` |
| `.gitignore` in place | `node_modules/` and `dist/` absent from the GitHub file tree |
| Local git repository initialized | `git status` reports a branch, not an error |
| Local repository connected to the GitHub remote | `git remote -v` shows `origin` |
| Code pushed to GitHub successfully | **<a href="https://bienko.github.io/bob-sdlc/build-1/" target="_blank">Part 1 Build</a>** files visible on the `main` branch |
| CI workflow detected | **Actions** tab lists **Finance App CI**, with no runs until the feature branch is pushed |
| Issues and Pull Requests enabled | **Settings → General → Features** |
| Feature branch created and pushed | `feature/initial-finance-dashboard` visible on GitHub |
| CI run green | **Actions** shows a passing **Finance App CI** run for the feature branch |
| Enhancement issue created | Issue `#1` open in the **Issues** tab |

## **iv. Key takeaways**

- Publishing happens in a fixed order: create the empty GitHub repository, let Bob confirm readiness, initialize the local repository, then push. Each step depends on the one before it.
- Bob catches delivery blockers a developer might miss, like a missing `.gitignore` that would commit thousands of dependency files, and prepares the exact commands to fix them.
- The developer performs the push. Bob prepares the delivery, but the repository remains under your control.
- Feature work travels on branches, never directly on `main`. The baseline branch exists now, and the enhancement gets its own branch in the next module.
- An issue is the entry point of the evolution loop. The next module starts from issue `#1` rather than from a prompt written from scratch.

</br>
The next module is the central demonstration of the lab. In <a href="https://bienko.github.io/bob-sdlc/evolve-2/" target="_blank">**Implementing a Feature Request**</a>, Bob will fetch the issue you just created, analyze the requested scope, implement the new company chart, and validate the result.

---