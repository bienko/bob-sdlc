**PART 2 | EVOLVE**
# **IMPLEMENTING A FEATURE REQUEST**

---

## **i. Why this matters**

![type:video](./_videos/6 Implementing a Feature Request [Bob SDLC L3].mp4)
!!! note ""
    Christopher Bienko *(AI Enablement GTM Product Manager)* provides an overview of the *Implementing a Feature Request* module for the hands-on enablement. **<a href="https://ibm.seismic.com/Link/Content/DCMhX3Cq8hmQ7GFGjCWqVgjX8TDP" target="_blank">[2 min]</a>**

**This module is the central demonstration of the lab.** All of your work prior to this stage— the plan, the scaffold, the dashboards, the push to GitHub —has been preparation for the next steps. Now a real enhancement request exists as a GitHub issue, and the question that this hands-on lab originally posed will be answered: *can IBM Bob take a repository enhancement request and convert it into working, validated implementation?*

In this module, you will use IBM Bob to fetch and analyze the GitHub issue from the previous module, define the acceptance criteria, identify the required UI, state, and data service changes, implement the chart for a company selected by the user, and validate the result, all without disturbing the existing dashboards for IBM and its competitors.

!!! warning ""
    The enhancement request stays fixed throughout this module: users can enter or select a company or ticker, the app resolves data for it, a new graph renders alongside the existing dashboards, and invalid symbols produce a clear message to the user.

Pay attention to how Bob grounds each phase in repository context (the issue text, the existing service layer, the company registry) rather than generating an isolated feature in a vacuum.

!!! note "AI MATURITY CURVE"
    The requirements gate ahead is the plan-approval discipline of **Module 2 | Delegate**, now applied to work that arrives from *outside* the conversation. A four-bullet GitHub issue goes in; numbered acceptance criteria — each with its verification method attached — come out, and nothing is implemented until you agree to the scope. The pattern is unchanged from the first time you practiced it: your judgment governs the branch points, and Bob works autonomously between them.


## **ii. Implementing a feature request**

**You will progress through this module via five actions:** create the branch the enhancement will travel on, ask Bob to analyze the requested scope and approve it at the requirements gate, ask Bob to implement the enhancement and approve its subtasks, run the validation gate, and verify the result in the browser. This mirrors the arc of a real delivery cycle, from requirements through development to testing, compressed into a single working session.

---

1. Every enhancement gets its own branch. Open a **new Terminal** (++ctrl+shift+grave++) within the Bob IDE and create the branch this feature will travel on, following the naming convention from the previous module:

    ```
    # Create and switch to the enhancement branch
    git checkout -b feature/user-selected-company-chart

    # Push the branch to GitHub so it exists on the remote
    git push -u origin feature/user-selected-company-chart
    ```

    Confirm with `git branch` that `feature/user-selected-company-chart` is now the active branch. All of the implementation work in this module lands here, never on `main`.

---

2. With **Agent mode** active, ask Bob to fetch and analyze the enhancement request. 

    - Copy and paste the following prompt into the **Agentic Sidebar**:

    ```
    Fetch the GitHub issue or pull request context requesting a new graph driven by a company selected by the user. Analyze the requested enhancement, define the likely acceptance criteria, identify the required UI, state, and data service changes, and explain how to implement the feature without breaking the existing dashboards that compare IBM against its competitors.
    ```

    - Press **Send** (or hit ++return++) to have Bob begin grounding the analysis in real code
    - Bob will pause for approval before its first tool call, such as a read-only listing like `find finance-app/src -type f | sort`
    - Click **Approve once**
    - Bob resolves the request from repository context, restates it in engineering terms, and produces a **Todo List** that plans the work end to end

    | SUB-TASK | OBJECTIVES |
    | -------- | ---------- |
    | **Sub-Task 1** | Add `customSymbol` and `setCustomSymbol` to `dashboardStore` |
    | **Sub-Task 2** | Add a `useCustomSymbolData` hook to `useMarketData` |
    | **Sub-Task 3** | Build the `CompanySelector` component and its CSS module |
    | **Sub-Task 4** | Build the `CustomSymbolPanel` component and its CSS module |
    | **Sub-Task 5** | Wire `CompanySelector` and `CustomSymbolPanel` into the dashboard page |
    | **Sub-Task 6** | Write tests for the store, the selector, and the panel |
    | **Sub-Task 7** | Add a mock data entry for the custom symbol, for example `NVDA` |
    | **Sub-Task 8** | Run `npm run validate` and confirm zero warnings with every test passing |

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/4d8b48fe-2d2f-4ef4-a124-6adc041e64f9/user_cropped_screenshot_ea07a9bb318f4c40a12eccfc8d245864_text_export.jpeg" alt="" width="600" />
    </p>

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/be1ea535-02e9-48ee-88d2-d64bb5b0a519/sps-redacted-e9b7d998400e43f6b3eebf6c9f664f17_af3e14bf998b4a999faad99d34ad6b72_text_export.jpeg" alt="" width="600" />
    </p>

    </br>
    Alongside the plan, Bob delivers the analysis itself as a report: **Feature Request Analysis — User-Selected Company Graph**
    
    - Click **Open** to read it
    - Notice that the issue asked for four things and Bob derived nine, each with a verification method attached
    - Scroll to the end of Bob's response for the impact map and the non-breaking guarantee

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/0d87fc01-e45b-468d-b97b-d056bdc16d63/sps-redacted-b23393f1189947248ce5d90fd7818dc3_f3465117873e4e2189192f36136ecc3e_text_export.jpeg" alt="" width="600" />
    </p>

    !!! warning ""
        **This report is the requirements gate.** It plays the same role the implementation plan played in *Part 1*: scope is agreed before a line of code is written. The report is tagged **Enhancement** and **Non-breaking**, names the branch it targets, restates the business request, and then does the thing that matters most, turning a four-bullet issue into numbered acceptance criteria that each say how they will be proven:

    !!! quote ""
        | ID | CRITERION | VERIFIABLE BY |
        | -- | --------- | ------------- |
        | AC-1 | A text input accepts any 1–5 letter uppercase ticker symbol | Integration test, valid input calls `onSubmit` |
        | AC-2 | The input is validated before fetch; empty or non-alpha strings show an inline error | Integration test, submitting empty or `IBM1` shows an alert |
        | AC-3 | While data is loading a spinner is shown inside the new panel | Integration test, mock returns `isLoading: true` |
        | AC-4 | If the symbol is unknown or the fetch fails, a clear error message is shown | Integration test, mock returns `isError: true` or `quote: null` |
        | AC-5 | On success a price chart and summary card render for the selected symbol | Integration test, panel renders with fixture data |
        | AC-6 | The three existing views remain untouched | All 114 existing tests continue to pass |
        | AC-7 | Clearing the selection hides the chart panel | Integration test, Clear button calls `onClear` |
        | AC-8 | The selected symbol persists across time-window tab switches | Store unit test, `customSymbol` survives `setActiveWindow` |
        | AC-9 | In mock mode the panel works offline with a fixture ticker | Mock fixture added to `quotes.json` and `history.json` |

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/146854b2-ff9a-41e3-b123-3c03ffc25425/sps-redacted-2a6e7811eb654112b530a3bdd9432594_a863622bb8c54c89886a554a83cf2427_text_export.jpeg" alt="" width="600" />
    </p>

    !!! warning "BOB STOPS AND WAITS"
        Bob ends the analysis by naming the next step and holding: the implementation subtasks are planned but not started. Nothing is written until you ask in *Step 3*. The requirements gate functions as a genuine checkpoint such that nothing advances until you say so.

    !!! note "ISSUE COVERAGE REVIEW"
        If your Bob build exposes slash commands, `/review --issue-coverage` performs a related workflow from the review side. It checks whether the changes on a branch actually cover what a referenced issue asked for. It is a useful cross check at the end of this module once the implementation exists.

    Bob lists exactly which files change and, just as importantly, which do not: **zero changes** to the API layer, the data normalization service, the company registry, the existing chart components, or the Express proxy. The new state is a new field on the store, the new data path uses its own React Query cache keys so it cannot collide with the existing ones, and the new panel only mounts once the user has entered a symbol, so it is invisible until it is asked for.

    If anything in the analysis contradicts the issue, say so in the sidebar and let Bob revise before you move on. Correcting scope here costs one message; correcting it after implementation costs a rewrite.

---

3. With the scope agreed, ask Bob to implement the enhancement. Copy and paste the following prompt into the **Agentic Sidebar**:

    ```
    Implement the requested feature so that the finance dashboard allows a user to indicate a company symbol and see a new graph for that company. Reuse the existing finance data layer where possible, keep the UX simple, and preserve the existing dashboards. If a symbol has no data available, show a clear no-data message rather than falling back to another company's data.
    ```

    - Press **Send** (or hit ++return++) to execute.
    - Bob starts the subtasks planned in *Step 2* and asks for approval as it goes
    - The Todo List at the top of the session tracks progress
    - The header will read **All tasks completed** once Bob is done

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-08-04/9553d7cf-f7bf-42d5-ae4d-8b2220567ef0/user_cropped_screenshot_d3b64244e65849bda5fe784e084293d0_text_export.jpeg" alt="" width="600" />
    </p>

    Watch the order in which Bob writes. Rather than creating files one at a time, it groups them into **dependency-ordered batches**, so nothing is written before the things it imports exist:

    | BATCH | WHAT IT WRITES |
    | ----- | -------------- |
    | **Batch 1** | The store change: `customSymbol` and `setCustomSymbol` added to `dashboardStore.js` |
    | **Batch 2** | The data hook: `useCustomSymbolData()` exported from `useMarketData.js` |
    | **Batch 3** | The new components: `CompanySelector` and `CustomSymbolPanel`, each with its CSS module |
    | **Batch 4** | The wiring: `CompanySelector` and `CustomSymbolPanel` mounted in `DashboardPage.jsx`, below the existing view switch |
    | **Batch 5** | The fixtures and tests: an `NVDA` entry appended to the mock JSON, plus the new test files |

    Each batch reports the files it touched. None of the existing view components are rewritten to make room for the new panel.

    After the last batch, Bob posts a **What was built** summary.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/d5747c43-1ba5-409f-8dbf-f538f522b7f3/user_cropped_screenshot_4357b7a34f31463aa03d08555a8913f6_text_export.jpeg" alt="" width="600" />
    </p>

    *Step 4* breaks the test count into new versus pre-existing and turns it into a formal verdict.

    Below the numbers, Bob lists the work by layer. These are the rows that carry the enhancement:

    !!! quote ""
        | LAYER | FILES | NOTES |
        | ----- | ----- | ----- |
        | **Store** | `dashboardStore.js` | `customSymbol` + `setCustomSymbol` for the new feature |
        | **Hooks** | `useMarketData.js`, `useCompanies.js` | `useCustomSymbolData()` is the new hook |
        | **Dashboard** | `DashboardPage.jsx` + existing views | `CompanySelector` + `CustomSymbolPanel` wired below existing views |
        | **New feature** | `CompanySelector.jsx`, `CustomSymbolPanel.jsx` | Pure and props-driven; validates the ticker with `/^[A-Z]{1,5}$/` |
        | **Services** | `financeData.js`, `financeApi.js`, `quotes.json`, `history.json` | `NVDA` fixture included |
        | **Tests** | 10 files, 3 unit and 7 integration | `dashboardStore.test.js`, `CompanySelector.test.jsx`, and `CustomSymbolPanel.test.jsx` cover the feature |

    Every one of those rows was named in the impact map in *Step 2*, and nothing turned up that the analysis did not predict. The API client, the normalization service, the company registry, and the existing chart components have no feature changes.

    The summary ends with a **Feature behaviour** paragraph. This is what you will check in the browser in *Step 5*:

    !!! quote ""
        Enter any ticker (for example `NVDA`) in the **Custom Company Chart** section and it appears permanently below the **Today / 7 Days / Quarter** views. The panel shows a quote summary card plus a 30-day line chart. Invalid input is rejected before any fetch. **Clear** removes the panel. Under `npm run dev:mock`, `NVDA` resolves from the fixture, and any other symbol returns the **No data found** state.

    Remember the last sentence for *Step 5*. In mock mode, only `NVDA` returns data. Every other symbol shows **No data found**, whether it is a real company like `AAPL` or a made-up ticker like `ZZZZ`.

    Bob stops here without committing anything. The counter at the bottom of the sidebar shows how many files changed. Do not commit or undo them: *Step 4* runs the validation against these changes, and the next module reviews the diff before anything is committed.

    !!! warning "ATTENTION: YOUR RESULTS MAY VARY"
        As in **<a href="https://bienko.github.io/bob-sdlc/build-1/" target="_blank">Part 1 | Build</a>**, Bob's implementation is **not deterministic**. The exact subtasks, file names, test counts, and UX shape (input field versus dropdown, panel placement) may differ from what is shown here. Judge the result against the acceptance criteria rather than against these screenshots.

        The session shown here rebuilt the whole application before adding the feature, which is why it reports **62 files changed**. If you carried your Part 1 code forward, expect far fewer.

---

4. With the implementation in place, run the validation gate. Copy and paste the following prompt:

    ```
    Run the appropriate project validation for the new graph feature for a company selected by the user. Summarize what passed, what failed, and whether the branch is ready to push back to GitHub.
    ```

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/7efb6dda-98cb-485c-a459-573998c1fe32/user_cropped_screenshot_37bc5e5b75a14ca7b29220dc5599065d_text_export.jpeg" alt="" width="600" />
    </p>

    Press **Send** (or hit ++return++). Bob runs the lint, test, and build gates against the code on disk, fixes anything that fails, and reports whether the branch is ready to deliver. If a gate fails, let Bob work through it.

    When it finishes, Bob answers with **Validation result: PASS, branch is ready**, then shows the results behind that verdict.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/4b209a05-3001-41af-bbc9-32aab623c5a1/user_cropped_screenshot_f412612c66b149b8a39d9e294105251f_text_export.jpeg" alt="" width="600" />
    </p>

    **Gate results** lists the three checks Bob ran. These are the same three GitHub Actions will run on the branch once it is pushed:

    !!! quote ""
        | CHECK | RESULT | WHAT IT MEANS |
        | ----- | ------ | ------------- |
        | **ESLint** | 0 errors, 0 warnings | No code-style or correctness warnings anywhere in `src/` |
        | **Vitest** | 128 / 128 passed | Every test in all 10 suites passed, 0 failures |
        | **Vite build** | Clean | The production build compiled with no warnings |

    Bob then lists what the 30 new tests actually check, one row per test file:

    !!! quote ""
        | SUITE | TESTS | SCENARIOS |
        | ----- | ----- | --------- |
        | `dashboardStore.test.js` | 12 | Initial `customSymbol: ''`, `setCustomSymbol` uppercases and trims, replaces a previous value, clears back to `''`, and survives `setActiveWindow` |
        | `CompanySelector.test.jsx` | 12 | Render, valid submit calls `onSubmit('NVDA')`, draft clears after submit, empty submit shows the error alert, digits rejected, Clear button visibility, `onClear` callback, active-symbol label, error clears on keystroke |
        | `CustomSymbolPanel.test.jsx` | 6 | Loading spinner, fetch-error banner, no-quote banner, success panel renders, ticker in header, empty-history fallback |

    Those scenarios match the acceptance criteria from *Step 2*: digits rejected and empty submit are AC-2, the loading spinner is AC-3, the error banners are AC-4, the success panel is AC-5, the Clear button is AC-7, and `survives setActiveWindow` is AC-8.

    **Zero regressions** answers AC-6:

    !!! quote ""
        All **98 pre-existing tests pass without modification**. The store change adds new fields only; the `DashboardPage.jsx` change appends the feature section *after* the existing view switch, so no existing JSX was removed or reordered.

    Bob ends with the commands to deliver the branch: `git add -A` and a commit whose message lists the store field, the hook, the two components, the `NVDA` fixture, the `ResizeObserver` polyfill, and the 30 new tests.

    !!! warning "DO NOT RUN THE PUSH COMMANDS YET"
        The branch is ready, but do not commit or push yet. The next module has Bob review the diff first, then prepares the delivery commands with the pull request in mind. Review the commands here; the next module is where you will run them.

    !!! note "IF A GATE COMES BACK RED"
        A first-run failure is normal and is worth watching rather than skipping past. The `ResizeObserver` polyfill in the run above exists because the chart component needs a browser API that jsdom does not implement, which surfaces as a failing test before it becomes a one-line fix in the test setup. Tell Bob to fix what failed and rerun the validation. The branch is not ready until this response reads **PASS** with all three gates green.

---

5. Verify the feature in the running application. Everything up to this point has been Bob reporting on its own work. This is where you check it yourself.

    With an open **Terminal** console, start the dev server in mock mode with the following commands:

    ```
    cd finance-app
    npm run dev:mock
    ```

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/56e00b65-fa9c-4c9b-8e88-d4a3b1c1e630/user_cropped_screenshot_64f6af796099474c99b3de09a98f3e4e_text_export.jpeg" alt="" width="600" />
    </p>

    Copy the local URL Vite prints, `http://localhost:5173/`, into a browser. The dashboard loads on the **Today** view with a **MOCK** badge beside the title, confirming everything on screen is fixture data.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/0c18a189-642e-401b-8e43-924e0634afc5/user_cropped_screenshot_7323d28069e541278a7e03e85f159280_text_export.jpeg" alt="" width="600" />
    </p>

    Check the original dashboard before touching the new feature. The **Today / Last 7 Days / Last Quarter** switch is still in the header, **Daily Performance Ranking** still ranks the tracked companies with their bars and daily percentages, and **Quote Detail** still renders a card per company. Nothing from Part 1 was moved or restyled to make room for the enhancement.

    !!! note "SIX COMPANIES, NOT FIVE"
        The ranking and quote grid above list six companies rather than the original five, because the `NVDA` fixture Bob added in *Step 3* was also registered in `companies.js`, the single source of truth for the tracked set. That is an additive entry in the registry rather than a change to the views, and it is why `NVDA` behaves like a first-class company here. Your run may show the original five with `NVDA` available only to the custom panel. Either is acceptable.

    Now scroll to the bottom of the page. Below **Quote Detail** there is a new **Custom Company** section holding the **Custom Company Chart** panel: one line of instruction, a ticker input, and a **Load** button. This is `CompanySelector`, mounted after the existing view switch exactly as the impact map in *Step 2* promised.

    Start with a malformed entry. Type `IBM1` and click **Load**.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/7a45c4cb-ed6c-4695-82b0-9852ed0996eb/user_cropped_screenshot_d0828bf07b2d475e9ec06f94d3a855b8_text_export.jpeg" alt="" width="600" />
    </p>

    The input rejects it inline with **Ticker must be 1–5 letters (e.g. NVDA)** and no fetch is issued. That is AC-2 confirmed in the browser, and it is the same rule the digits-rejected case in `CompanySelector.test.jsx` exercises. An empty submission behaves the same way.

    Next, try a valid ticker that has no data in mock mode, such as `AAPL`.

    <p align="left">
    <img src="https://colony-recorder.s3.us-west-1.amazonaws.com/files/2026-08-10/bf227869-5dbd-4379-9787-05f4eeb066a3/user_cropped_screenshot_d84fb2982dd34049b69bb83652a3b47a_text_export.jpeg" alt="" width="600" />
    </p>

    The panel accepts the symbol, reports **Currently showing: AAPL**, and replaces the chart with a clear message: **No data found for symbol "AAPL". Check the ticker and try again.** That is AC-4: a valid symbol the app cannot resolve produces a clear message instead of a crash or an empty chart.

    Finally, load `NVDA`. The panel shows NVIDIA's quote card and its 30-day price chart. Click **Clear** and the panel disappears. Load it again, then switch between **Today**, **Last 7 Days**, and **Last Quarter**: the panel stays put.

---

## **iii. Reviewing the enhancement**

Walk the acceptance criteria one final time against what is now running:

| ACCEPTANCE CRITERION | HOW IT IS MET |
| -------------------- | ------------- |
| Users can enter or select a company or ticker | New selector control on the dashboard |
| The app fetches or resolves data for that company | Data service extended to resolve arbitrary tickers, reusing the **<a href="https://bienko.github.io/bob-sdlc/build-1/" target="_blank">Part 1 Build</a>** layer |
| A new graph is rendered without removing the existing dashboards | Additive chart panel; the Current Day, Last 7 Days, and Last Quarter views unchanged |
| Invalid symbols produce a clear message to the user | Malformed input is rejected inline before any fetch; a valid symbol with no data shows a clear no-data message |
| Tests or validation steps cover the new behavior | New unit and component tests; the full lint, test, and build gate is green |

A GitHub issue went in, and a working, tested feature came out, with the analysis, the plan, and the test results visible at every step.

---

## **iv. Key takeaways**

- A GitHub issue is enough context for Bob to derive acceptance criteria, work out which files are affected, and identify what must not break.
- Agreeing the scope before any code is written is the same checkpoint the plan review provided in **<a href="https://bienko.github.io/bob-sdlc/build-1/" target="_blank">Part 1 | Build</a>**.
- Reusing the existing data layer keeps the enhancement additive: nothing was rewritten or restructured to fit it in.
- The branch is not done until lint, tests, and build all pass, and the feature has been checked in the browser.
- Bob is not deterministic, so your component and file names may differ from the screenshots. Judge your result against the acceptance criteria.

</br>
Don't commit any code yet. In the final module, <a href="https://bienko.github.io/bob-sdlc/evolve-3/" target="_blank">**Preparing the Pull Request**</a>, Bob reviews the diff, writes the pull request description, prepares the push, and hands the enhancement off for merge.

---