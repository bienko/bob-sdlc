**PART 2 | EVOLVE**
# **PREPARING A PULL REQUEST**

---

## **i. Why this matters**

![type:video](./_videos/7 Preparing a Pull Request [Bob SDLC L3].mp4)
!!! note ""
    Christopher Bienko *(AI Enablement GTM Product Manager)* provides an overview of the *Preparing a Pull Request* module for the hands-on enablement. **<a href="https://ibm.seismic.com/Link/Content/DCHJjRTm8qCFmGTRRp9Gp3R2DWjG" target="_blank">[2 min]</a>**

A convincing agentic workflow must show how the work gets *finished*— reviewed, described, delivered, and merged —every bit of detail as how the code gets written. The chart for a company selected by the user now runs locally and passes validation. From GitHub's perspective, however, nothing has happened yet. The issue is still open, the feature branch has no new commits, and no reviewer has anything to look at.

!!! warning ""
    The realistic closure path takes one of three shapes: Bob updates the PR branch with the requested code and tests, Bob prepares the branch and PR notes for a maintainer to merge, or a downstream GitHub automation merges or closes the PR after validation. This module demonstrates the **maintainer** merge path, the most common in practice.

In this module, you will use IBM Bob to review the updated branch, generate the pull request description, prepare the push commands, open the pull request on GitHub, and close the loop by merging the enhancement and letting GitHub close the issue that started it.

Note how the work is split: Bob prepares everything, GitHub runs the checks, and a person makes the decision to merge.

!!! note "AI MATURITY CURVE"
    Take note of where this final module places the authority to act. Bob prepares the review, the description, and the commands; GitHub's CI re-validates the work on a machine no agent can edit; and the decision to merge — the single action that changes `main` — belongs to a person. The control discipline that has run through this entire curriculum concludes precisely where it matters most: at the moment of delivery.


## **ii. Preparing a pull request**

**You will progress through this module across three phases:** review and describe the change, push and open the pull request, and merge to close the loop.

---

1. With **Agent mode** active, ask Bob to review the branch and prepare everything needed to deliver it. Copy and paste the following prompt into the **Agentic Sidebar**:

    ```
    Review the updated branch, summarize the feature implementation, generate a pull request update description, and provide the git commands needed to push the changes so the GitHub pull request can be completed.
    ```

    - Press **Send** (or hit ++enter++)
    - Bob asks for approval before its first command
    - It is read-only: `git log`, `git status`, and `git diff --stat`, run in `finance-app` to see the current state of the branch
    - Click **Approve once**

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/592ae795-9026-4ca5-b7c4-3dd6bf709aaf/user_cropped_screenshot_788a4183f1214839bbbb562743770fe5_text_export.jpeg" alt="" width="600" />
    </p>

    Bob reads through the changes you have not committed yet and produces a report titled **feat: add user-selected company chart**. Click **Open** to read it.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/d3eb60ca-536e-455d-87a9-1818ac6bfb4c/user_cropped_screenshot_377ff1f2143d4450b8270d15e78ef32e_text_export.jpeg" alt="" width="600" />
    </p>

    The header shows where the change is going, `Base: main ← Head: feature/user-selected-company-chart`, along with status badges for the checks, the test count, and the number of new files. The **Closes #1** tag links the work back to the issue. Below the header, the report is structured like a pull request body:

    !!! quote ""
        - **Feature summary**: what the new section does. It sits below the existing dashboards, the user types a ticker, and a quote card and 30-day line chart render for it. The three original views are untouched. The summary also covers input validation before any network call, loading and error states, the **Clear** button, the symbol surviving tab switches, and the `NVDA` fixture that lets the feature run offline.
        - **New data flow**: a diagram following one submission from the input to the data layer, `CompanySelector` → `dashboardStore` → `DashboardPage` → `CustomSymbolPanel` → `useCustomSymbolData` → `financeApi / mock`. One line underneath explains why nothing breaks: the new queries use their own cache keys, and neither one runs until a valid ticker is entered.
        - **Files changed**: a table listing each file with a one-line note on what changed in it.

    The final table deserves a closer inspection.

    !!! quote ""
        | FILE | TAG | CHANGE |
        | ---- | --- | ------ |
        | `src/store/dashboardStore.js` | Modified | Added `customSymbol: ''` state and the `setCustomSymbol()` action. Existing fields unchanged |
        | `src/hooks/useMarketData.js` | Modified | Added `useCustomSymbolData(symbol)`, two parallel React Query calls for quote and history. Existing hooks unchanged |
        | `src/components/dashboard/DashboardPage.jsx` | Modified | Two new imports, two new store selectors, and a Custom Company section appended *after* the existing view switch. No existing JSX removed |
        | `src/services/mock/quotes.json` | Modified | `NVDA` quote entry appended. The existing five entries are left in place |

    !!! warning "TREAT THE DESCRIPTION AS A DRAFT"
        Read Bob's description before you paste it into GitHub. It is generated from the code Bob wrote and the tests that passed, so treat it the way you would a colleague's PR body: check that every claim matches what you saw in the browser in the previous module, and edit anything that overstates the change.

    !!! note "SLASH COMMAND EQUIVALENTS"
        If your Bob build exposes slash commands, this phase maps onto them directly: `/review` performs the diff review and surfaces findings, and `/create-pr` generates the description and prepares the pull request in one step. The prompt above demonstrates the same capabilities conversationally.

    Keep the report open. It provides you the text you will paste into the pull request in *Step 3*.

---

2. Scroll to the **Push commands** block at the end of Bob's review. It contains three commands: one to stage the files, one to commit them with the message already written out, and one to push the branch.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/fe755492-3d55-43db-8c93-052c5dc63820/user_cropped_screenshot_a7555d7800a24c6fac3f749644e88759_text_export.jpeg" alt="" width="600" />
    </p>

    In the run shown, Bob produced this sequence:

    ```
    git add -A
    git commit -m "feat: add user-selected company chart (CompanySelector + CustomSymbolPanel)

    - customSymbol state in dashboardStore + setCustomSymbol action
    - useCustomSymbolData hook (quote + 30-day history, mock-aware)
    - CompanySelector: ticker input with 1-5 letter local validation
    - CustomSymbolPanel: quote card + line chart, loading/error states
    - NVDA mock fixture in quotes.json + history.json
    - ResizeObserver polyfill in test setup
    - 30 new tests: dashboardStore(12) CompanySelector(12) CustomSymbolPanel(6)
    - 128/128 tests, 0 ESLint warnings, Vite build clean

    Closes #1"

    git push -u origin feature/user-selected-company-chart
    ```

    Two things to note before you run it:
    
    - The whole commit message sits inside one pair of quotes, so the block can be pasted and run as it is.
    - The last line, `Closes #1`, is what makes GitHub close the issue when this work is merged.

    Copy the block, paste it into a Terminal console, and hit ++return++ to execute.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/fcbed3c9-5aeb-4b0d-b3b6-8cc7ec605f5e/user_cropped_screenshot_96117d07c56b4a0181eff51f4f20a010_text_export.jpeg" alt="" width="600" />
    </p>

    Check the output:

    !!! note ""
        - **`Your commit was protected by Block Secrets powered by IBM Vault Radar`**: the workspace scanned your files for passwords and keys before allowing the commit. This runs on every commit and is not a problem with your code.
        - **`[feature/user-selected-company-chart f36b1c4] feat: add user-selected company chart ...`**: the branch name and the new commit ID. The work went onto the feature branch, not `main`.
        - **`62 files changed`**: how big the commit is.
        - The file list, where `create mode 100644` marks a new file. Look for `CompanySelector.jsx`, `CompanySelector.module.css`, `CustomSymbolPanel.jsx`, and `CustomSymbolPanel.module.css`. Those four are the new feature.

    !!! warning "CHECK WHAT YOU ARE COMMITTING FIRST"
        `git add -A` stages every changed file without asking. Run `git status` before you commit and read the list. If it includes anything you did not mean to deliver, add files one at a time with `git add <path>` instead.

    The push runs straight after the commit and sends the branch to GitHub, which starts CI.

---

3. Open the pull request on GitHub. Refresh the repository page in your browser. GitHub shows a banner at the top of the file list saying the branch had recent pushes, with a green **Compare & pull request** button — click it.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/5ae4fb25-7a65-495c-903b-be27cd33bf58/user_cropped_screenshot_39b31d3856654d50a3d0c8b5abca65ce_text_export.jpeg" alt="" width="600" />
    </p>

    Note that `main` still shows the commit from the previous module. Your new work is on the feature branch, and the pull request is the request to bring it into `main`.

    !!! warning "IF THE BANNER IS NOT THERE"
        The banner disappears after a while. Open the **Pull requests** tab, click **New pull request**, then set the base to `main` and the compare branch to `feature/user-selected-company-chart`. That opens the same form.

    The **Open a pull request** form already has two things in it. GitHub fills the title and description from your commit. Below that is the repository's own template from `.github/PULL_REQUEST_TEMPLATE.md`, with empty **Summary**, **Linked Issue**, **Changes**, **Test Coverage**, **Validation**, **Screenshots / Data Snapshots**, and **Notes for Reviewer** headings.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/6fc23285-f18c-463b-a26f-76fe5e4e6605/user_cropped_screenshot_48912cab42374a8a8e9da5b6d792829a_text_export.jpeg" alt="" width="600" />
    </p>

    Fill in each heading using Bob's report from *Step 1*:

    | TEMPLATE SECTION | WHAT TO PASTE FROM BOB'S REPORT |
    | ---------------- | ------------------------------- |
    | **Summary** | The feature summary: what the new section does and what it renders |
    | **Linked Issue** | `Closes #1`, so the merge closes the issue |
    | **Changes** | The files-changed table, one bullet per file |
    | **Test Coverage** | The new tests, and tick the three checkboxes |
    | **Validation** | The check results: lint clean, all tests passing, clean build |

    !!! warning "FINISH THE `Closes #` LINE"
        The template includes an empty `Closes #` under **Linked Issue**. Change it to `Closes #1` or delete the line. On its own, `Closes #` does not link to anything.

    When the description is ready, click **Create pull request**.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/7eb728be-6cf5-4bdd-a595-92ab9f5b39eb/sps-redacted-86a0ba3f072b4e4ab01f7fa227b24501_3a4420231f5e4cbeae37eb0eb917fb4c_text_export.jpeg" alt="" width="600" />
    </p>

    The pull request is now open. Its header tells you:

    !!! note ""
        - The pull request number and the green **Open** badge.
        - Who wants to merge what, and in which direction: one commit, from `feature/user-selected-company-chart` into `main`.
        - The tabs: **Commits**, **Checks**, and **Files changed**, with the number of added and removed lines. The file count matches what the terminal reported in *Step 2*.
        - **Able to merge** at the top right, and `Closes #1` as a working link to the issue.

    !!! warning "CHECK THAT THE ISSUE IS LINKED"
        Look at the **Development** panel in the right sidebar. If the link worked, GitHub lists issue `#1` there under *Successfully merging this pull request may close these issues*.

        If it says **None yet**, edit the description so there is exactly one `Closes #1` on its own line, then save. This link is what closes the issue when you merge in *Step 6*.

---

4. Let GitHub run the checks. Scroll to the bottom of the **Conversation** tab. Below the description is a panel reporting **Some checks haven't completed yet**.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/00e498b2-d0a2-498f-a769-6be8f38df9ae/user_cropped_screenshot_3814f2145c4f40df943825febd15b13d_text_export.jpeg" alt="" width="600" />
    </p>

    There are two entries, and both come from the workflow file you pushed in module 2-1:

    !!! note ""
        - **`Finance App CI / validate (push)`** — already finished. This ran when you pushed the commit in *Step 2*, because the branch name matches the `feature/**` pattern in the workflow's push trigger.
        - **`Finance App CI / validate (pull_request)`** — still running. This one started when you opened the pull request against `main`, which matches the workflow's second trigger.

    Below them, GitHub says **No conflicts with base branch**, which means it can merge the branch automatically once the checks finish.

    Click the **Finance App CI** check name (or **Details** beside it) to open the run.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/ded7d65c-714f-4b48-b790-83071dfee31e/sps-redacted-e80832ce896a47f0bd2930325d5f56af_c50bae0885f04656b1dab432b2d05d12_text_export.jpeg" alt="" width="600" />
    </p>

    This is the `validate` job running, one step per line. Click any step to see its log. The first steps set up the machine: it checks out your branch, installs Node, then runs `npm ci` to install the dependencies from `package-lock.json`. The three checks are queued below and greyed out until their turn: **Run npm run lint**, **Run npm run test:ci**, and **Run npm run build**.

    !!! note ""
        The log says **Node 20 is being deprecated**. That is GitHub retiring the version its own actions run on. It has nothing to do with your application.

        It also says **npm cache is not found**. That is expected on a first run: there is nothing cached yet, so `npm ci` downloads everything. Later runs are faster.

    Wait for the job to finish, then reload.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/268028b8-b98d-4956-9062-b9714b3141f1/sps-redacted-2c30c488c44a4375a5b7051228ef4398_19a364d0a4e143ec8264329788fd5d1d_text_export.jpeg" alt="" width="600" />
    </p>

    The `validate` job succeeded, with a green check on every step. Click **Run npm run lint** and the log shows the command GitHub ran:

    ```
    > finance-app@0.1.0 lint
    > eslint src --ext .js,.jsx --report-unused-disable-directives --max-warnings 0
    ```
    !!! warning ""
        Bob validated the code locally, but the merge decision rests on CI results no agent can edit and a review no agent can skip.

    That is the same command Bob ran in the previous module, now run on a fresh machine that has never seen your workspace. With **Run npm run test:ci** and **Run npm run build** green as well, all three checks Bob ran locally have passed again on GitHub.

---

5. Back in the Bob IDE, ask Bob to confirm the branch is finished. Copy and paste the following prompt:

    ```
    Summarize the completed enhancement, confirm the branch is ready, and describe the final GitHub action needed to merge or close the request.
    ```

    Press **Send**. Bob runs `git show --stat HEAD` first so the summary is based on the commit that actually exists, then reports back that everything is confirmed. It produces a report titled **SDLC Loop Closure — User-Selected Company Chart**. Click **Open**.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/18a9e9ff-ee85-4d82-a423-127c787b0d02/user_cropped_screenshot_67ed74d65abc41cc83681bdaffe7906e_text_export.jpeg" alt="" width="600" />
    </p>

    This report is the summary of everything the workshop produced.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/cf0926a2-73cd-49c9-a11f-f51e9c9c0150/user_cropped_screenshot_ce0d50ccc7b0489b8c34473a066f0c3d_text_export.jpeg" alt="" width="600" />
    </p>

    The header gives the commit and the branch, tagged **Ready to merge** and **Closes #1**. Below it, four tiles show the totals: tests passing, new tests added, ESLint warnings, and acceptance criteria met.

    The **Enhancement summary** describes the feature in plain terms: a user enters a ticker and gets a quote card and a 30-day price chart, in a **Custom Company** section below the existing dashboards. It then covers the rest of the behaviour, including validation before any network call, the loading and error states, the Clear button, the symbol surviving tab switches, and the `NVDA` fixture for offline use.

    Two columns list the new files and the changed ones:

    !!! quote ""
        | NEW FILES | MODIFIED |
        | --------- | -------- |
        | `CompanySelector.jsx` + `.module.css` | `dashboardStore.js`, `customSymbol` state and action |
        | `CustomSymbolPanel.jsx` + `.module.css` | `useMarketData.js`, the `useCustomSymbolData` hook |
        | `dashboardStore.test.js` (12 tests) | `DashboardPage.jsx`, custom section wired below the views |
        | `CompanySelector.test.jsx` (12 tests) | `mock/quotes.json` and `mock/history.json`, `NVDA` appended |
        | `CustomSymbolPanel.test.jsx` (6 tests) | `tests/setup.js`, the `ResizeObserver` polyfill |

    That is a much shorter list than the file count in the commit, because git counts every file the session rewrote while this report lists only the files the feature actually needed. If someone asks what this change consists of, the table above is the answer.

    The report ends with **Full SDLC journey — steps completed**, which walks back through the lab stage by stage: the issue Bob fetched, the acceptance criteria it defined, the implementation, the validation, and the delivery.

    Bob also posts the same summary in the sidebar, so you can read it without opening the report.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/37519557-f53d-49fa-9e1a-9505f22aefab/user_cropped_screenshot_ff72b8380449477ab6a337919282ff50_text_export.jpeg" alt="" width="600" />
    </p>

    It starts with **Enhancement complete, branch is merge-ready**, recaps what was built, and lists four reasons the change is safe: `CompanySelector` validates the ticker before any network call; `CustomSymbolPanel` only appears once a symbol is entered and reuses the existing `SummaryCard` and `LineChart` unchanged; `useCustomSymbolData` uses its own cache keys, so it cannot affect the existing queries; and `customSymbol` is stored in Zustand, so it survives tab switches.

    Below that, the **Branch state** table lists what Bob checked against git:

    !!! quote ""
        | CHECK | RESULT |
        | ----- | ------ |
        | **Commit** | The commit ID and message |
        | **Working tree** | 0 untracked or modified files |
        | **ESLint** | 0 errors, 0 warnings |
        | **Tests** | All passing, with no regressions |
        | **Build** | Completed successfully |
        | **Acceptance criteria** | All met, with passing tests |

    The row to check before merging is **Working tree: 0 untracked or modified files**. It means there are no changes left on your machine that GitHub has not seen, so the CI run in *Step 4* tested exactly the code you are about to merge.

---

6. Merge the pull request. Open the repository on GitHub and click the **Pull requests** tab in the header.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/69ee545f-bf7c-411b-9d5b-74a61d5e39e7/user_cropped_screenshot_7bca623eeadc496497608840fd539836_text_export.jpeg" alt="" width="600" />
    </p>

    The list shows open pull requests only. The single entry is the one you created, with a green check because its checks passed and a **3 tasks** counter from the checkboxes you ticked in the template. Click the title to open it.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/e5993019-544b-4c91-a872-ab1f523bb925/user_cropped_screenshot_44efc3b68f764433a488bbd937c91815_text_export.jpeg" alt="" width="600" />
    </p>

    Review the change one final time before merging. Open the **Files changed** tab and compare it against Bob's report from *Step 5*. You should find the two new components, the store field, the hook, the fixtures, and the tests, and nothing else.

    Then go back to the **Conversation** tab and scroll to the merge box at the bottom.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/043d6b3e-de67-4dd8-a826-3f179e05c813/user_cropped_screenshot_b0914757636b4242b8324c04ef015024_text_export.jpeg" alt="" width="600" />
    </p>

    Both entries are green now: **All checks have passed**, with **2 successful checks** where *Step 4* had one still running, and **No conflicts with base branch**. Click **Merge pull request**.

    GitHub asks you to confirm and shows the merge commit it will create.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/c55da5e3-f150-4201-af75-5aa475018406/user_cropped_screenshot_9d61c7c70cf74da88f84153627e8a4a9_text_export.jpeg" alt="" width="600" />
    </p>

    GitHub fills in the **Commit message** and **Extended description** for you. Leave both as they are and click **Confirm merge**.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/3cf43904-521c-40ea-99b8-997475eb21de/user_cropped_screenshot_093f74e826514e97b8225719706f3e32_text_export.jpeg" alt="" width="600" />
    </p>

    The badge at the top turns purple and reads **Merged**, and the header now says the commit was merged into `main`. The feature is now part of the main branch.

    Next, check what the merge did to the issue. Click the **Issues** tab.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/ef6b478f-f67a-4e54-a5e7-b5c97b873845/user_cropped_screenshot_5ac6e5ec29b14c1e9df2eab6dfd6e18b_text_export.jpeg" alt="" width="600" />
    </p>

    The default view shows open issues, and it now reports **Open 0** with **No results**. Click the **Closed** filter to see where the issue went.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/b2d3516c-7469-446b-8ff5-3f032a7f9597/user_cropped_screenshot_f4fabdf8e4ca44759385a26dcbf3d206_text_export.jpeg" alt="" width="600" />
    </p>

    Issue `#1`, **Add a graph for a user selected company**, is marked closed, with a link on the right to the pull request that closed it. You never closed it yourself. The `Closes #1` in the description did that when you merged.

    !!! warning "IF YOUR ISSUE IS STILL OPEN"
        The `Closes #1` line did not register. Close the issue by hand, and next time add it to the description before you create the pull request.

    The loop is complete: a GitHub issue went in, and a merged feature came out, checked by CI and linked back to the issue. The next enhancement follows the same path: new issue, new branch, new analysis, new pull request.

---

## **iii. Complete SDLC coverage with Bob + GitHub**

Across the two parts of this workshop, every stage of the software development lifecycle appeared, and Bob took part in each one:

| SDLC STAGE | WHERE IT HAPPENED | OUTCOME |
| ---------- | ----------------- | ------- |
| **1. Planning & Requirements** | Bob fetched and analyzed the GitHub issue (previous module) | Requirements understanding and acceptance criteria |
| **2. Design & Architecture** | Bob identified the UI, state, and data service change areas | Implementation plan that preserves existing features |
| **3. Implementation** | Bob implemented the new company chart | Working code that satisfies the requirements |
| **4. Testing & QA** | Bob ran lint, tests, and build locally | Verified functionality with passing gates |
| **5. Code Review** | Bob reviewed the diff before delivery (`/review`) | Findings surfaced before a human reviewer's time is spent |
| **6. Integration Preparation** | Bob generated the commit message and PR description | Clean commits and a description ready for review |
| **7. Release Management** | The pull request opened against `main` (`/create-pr`) | PR ready for maintainer review and merge |
| **8. Continuous Integration** | GitHub Actions validated the push and the PR | Automated lint, test, and build verification |
| **9. Maintenance & Evolution** | The loop repeats for every new issue | Continuous product evolution with Bob's assistance |

Every change also traces back to the request that started it:

```
GitHub Issue Created
       ↓
Bob Fetches & Analyzes the Request
       ↓
Bob Implements the Feature
       ↓
Bob Runs Tests & Validation
       ↓
Bob Reviews the Changes
       ↓
Bob Prepares the Pull Request
       ↓
GitHub Actions CI/CD Validates
       ↓
Human Maintainer Reviews & Merges
       ↓
Feature Deployed and Issue Closed Automatically
```

---

## **iv. Key takeaways**

- Finishing the work can be automated too. Reviewing the changes, writing the pull request description, and preparing the push are all things Bob can do.
- `Closes #N` in the pull request description is the cheapest traceability available. Merging the work closes the request that asked for it.
- Running the checks again on GitHub is what makes the work trustworthy. Passing locally is a *claim* whereas passing on CI is *proof*.
- The merge is left to a person by design: Bob prepares the delivery, GitHub validates it, and a human-in-the-loop makes the decision.
- The loop repeats. Every future enhancement follows the same path: issue, analysis, implementation, validation, pull request, merge.

Congratulations on completing this hands-on curriculum! In **Part 1 | Build** you planned and built an application with Bob; in **Part 2 | Evolve** you published it to GitHub and carried a real feature request from an open issue to a merged pull request.

Step back and consider what that loop demonstrates: the discipline of the **AI Maturity Curve**, operating inside the delivery platform your clients already use. Bob performing the work at every stage, and the human deciding what ships. That pairing is the story worth carrying into your next conversation about AI-assisted delivery.

---

## **v. Earn accreditation**

---

In order to receive credit for completion of this course, you must perform one of the two actions (in accordance with your role) described below:

!!! note "IBMers: RECORD A STAND & DELIVER"

    **IBM regular employees** must record a Stand & Deliver to receive credit for this course. By default, IBMers will be evaluated by their First Line Manager (FLM) — although they may request another manager to evaluate their Stand & Deliver, if appropriate. Instructions on how to submit a Stand & Deliver are included within the activity on **IBM YourLearning**.

!!! warning "Business Partners: COMPLETE THE SKILLS EVALUATION QUIZ"

    The skills evaluation quiz **only applies to business partners**. The quiz consists of multiple choice questions, with four possible responses (and only one correct answer) for each question. The quiz questions will ask you about on-screen text or descriptions that come up as you work through the lab guide. Participants must pass the quiz with a grade of 80% or higher. Answers to the quiz can only be determined by carefully following the instructions of the hands-on lab. A link is provided within the course page on **IBM Training**.

---