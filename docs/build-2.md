**PART 1 | BUILD**
# **BUILDING AND VALIDATING THE DASHBOARDS**

---

## **i. Why this matters**

![type:video](./_videos/4 Building and Validating the Dashboards [Bob SDLC L3].mp4)
!!! note ""
    Christopher Bienko *(AI Enablement GTM Product Manager)* provides an overview of the *Building and Validating the Dashboards* module for the hands-on enablement. **<a href="https://ibm.seismic.com/Link/Content/DCTT73RR3HbPXG2MF7q3bPJ33HFB" target="_blank">[2 min]</a>**

The scaffold from the previous module runs, but it doesn't yet deliver any business value. The `Current Day` view renders mock quotes, and the `Last 7 Days` and `Last Quarter` tabs are empty.

Turning that scaffold into a real dashboard is where most of the engineering work actually lives. The data service has to fetch and normalize live market data. The views have to render summary cards and comparison charts across three time windows. And once the app is doing something real, it needs to survive the kinds of runtime failures that only surface when a live API is on the other end — missing tickers, environment-variable misreads, transient network errors.

!!! note "AI MATURITY CURVE"
    Take note of two working patterns from the previous course as they reappear here. The screenshot-driven debugging pass is evidence-grounded delegation: Bob diagnoses against the failing UI and against the code on disk, exactly as you practiced in **Module 2 | Delegate**. And the closing diff review — together with the commit message and pull request description it produces — translates that same discipline into delivery form: the developer reads what changed, decides what it means, and approves what ships.

In this module, you will use IBM Bob to integrate the Yahoo Finance data layer, fill in the three dashboard views, optionally enable the live proxy, debug a real runtime error using a screenshot as evidence, and prepare a review-ready commit and pull request description for GitHub delivery.

Pay attention to how Bob operates in the middle of the loop — reading the existing scaffold, extending it deliberately, and diagnosing failures with the current file contents in hand rather than guessing.

---

## **ii. Building and validating the dashboards**

You will move through this module in **four phases**: data integration, dashboard views, optional live-data enablement, and a debug-and-review pass. Each phase builds on the previous one and stays inside the folder layout you approved in the plan.

---

1. With **Agent mode** still active, ask Bob to implement the finance data layer. Copy and paste the following prompt into the Agentic Sidebar:

    ```
    Implement a finance data layer for IBM and its main competitors using Yahoo Finance data. Normalize the returned data so the UI can display quote summaries, short-term history, and quarterly trend views. Keep the code easy to extend if more companies need to be added later.
    ```

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/4b056b7a-04d2-4102-a961-e3fcb123e635/user_cropped_screenshot_9e12f40c91614bfdb1818b8f12e36a85_text_export.jpeg" alt="" width="600" />
    </p>

    Press the **Send** button to submit the task.

    Bob will extend the existing `financeService`, `yahooAdapter`, and `mockData` files, add new domain types for extended quote summaries and normalized history points, run the test suite, and confirm the lint and build gates are clean.

---

2. With the data layer in place, ask Bob to build the three dashboard views. Copy and paste the following prompt:

    !!! warning ""
        `
        Build 2 to 3 dashboard views for the finance application:
        - current day market summary
        - last 7 days trend comparison
        - last quarter comparison view

        Include IBM and up to four competitors. Use charts and summary cards where appropriate. Keep the UI simple, readable, and demo-friendly.
        `

    Bob will fill in `CurrentDayView`, `WeekView`, and `QuarterView` — the IBM spotlight card, competitor grid, ranked performance bars, normalized line chart, and quarterly area chart with a sortable ranking table — then re-run validation.

    !!! tip "MODE SELECTION"
        If a **Carbon React** mode is available in your Bob build, selecting it before submitting this prompt will tighten the visual result — the generated components inherit IBM Carbon Design System tokens and spacing rather than plain CSS.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/e7a03164-8b9a-4311-abfb-688ac6dccc12/user_cropped_screenshot_4c44ce612397401fa328ad0e374223e3_text_export.jpeg" alt="" width="600" />
    </p>

---

3. At this point, the app still runs entirely on deterministic mock fixtures. If you want to see the dashboard against real market data, Bob provides an optional two-terminal workflow. Otherwise, skip ahead to *Step 8*.

    The live-data path uses two terminals: one for the Yahoo Finance proxy, one for the dev server with mock mode turned off.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/5c769181-c1a4-456a-941b-46b165767670/user_cropped_screenshot_e15f32f7832b44cc9386ef933074496c_text_export.jpeg" alt="" width="600" />
    </p>

---

4. Open a second terminal for the proxy. From the menu bar:

    **Terminal → New Terminal**

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/fda04eed-c9d3-48e5-adcc-af6ce2fafa15/user_cropped_screenshot_b6824a3e8ce44cada0e042c6422fbf31_text_export.jpeg" alt="" width="600" />
    </p>

---

5. In the new terminal, install the proxy dependencies and start it:

    ```
    cd finance-app
    npm install yahoo-finance2 express
    npm run proxy
    ```

    The Express proxy starts at `http://127.0.0.1:3001` and forwards `/api/finance/*` calls to Yahoo Finance.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/6458a146-b8bf-4dda-be82-3363dc88c4b4/user_cropped_screenshot_4e6df9bb390a4b19b7c692222db19618_text_export.jpeg" alt="" width="600" />
    </p>

---

6. Split the terminal so you can run the dev server side by side. Click the **split** icon in the terminal header, or from the dropdown next to the **+** button, choose **Split Terminal**.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/acc53a58-5025-4ee9-be34-2fec702ba803/user_cropped_screenshot_41ee23572aeb45c383ed06db79fe9658_text_export.jpeg" alt="" width="600" />
    </p>

---

7. In the second pane, start the dev server with mock mode turned off:

    ```
    cd finance-app
    VITE_USE_MOCK=false npm run dev
    ```

    Vite will forward `/api/finance/*` requests to the proxy. Reload the dashboard in your browser.

    !!! warning "PROXY MUST START FIRST"
        The proxy must be listening on port 3001 **before** the dev server is started, or API calls will return 502 errors. If you see a retry banner in the app, the proxy is not reachable.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/5317c22a-eea6-421c-8cb6-9177c0d00b60/user_cropped_screenshot_1405edfaaf0f403396202a46828abb18_text_export.jpeg" alt="" width="600" />
    </p>

---

8. This is where a real-world runtime failure shows up. When the dashboard loads against the live proxy, the Current Day view may render an error banner: **Unable to load market summaries. Please try again.**

    Rather than debugging this by hand, give the error to Bob. Take a screenshot of the browser, attach it to a new Bob task, and describe the issue in plain language.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/f5ba2df3-5c69-4ede-bf04-3f8007d3eecd/user_cropped_screenshot_f8d548c560374aa98af267d16d4ca6fb_text_export.jpeg" alt="" width="600" />
    </p>

---

9. Attach the screenshot and enter a prompt like:

    ```
    I tried launching the application. It has an error. I attached the screenshot for your reference.
    ```

    Bob will inspect the screenshot, cross-reference the running code, and trace the error to the environment-variable check inside `financeService.ts`. Approve any commands Bob needs to run for diagnosis.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/aa6a7bfd-bc6a-4fb1-bc03-504d5c785904/user_cropped_screenshot_56412b5f18804dceb784269bd3fff1f7_text_export.jpeg" alt="" width="600" />
    </p>

---

10. Bob will apply the fix — flipping the mock-mode check from opt-in (`=== 'true'`) to opt-out (`!== 'false'`) so a missing or undefined value defaults to safe mock mode instead of accidentally hitting the network — update the tests, and re-run validation.

    A **What was wrong and what's fixed** summary appears in the sidebar, explaining the Vite client-side env-injection race that caused the runtime failure.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/6172b586-eb20-4340-95b5-e92c2f614bad/user_cropped_screenshot_9fbdc2ce9b4644bcbdc5e56ce7c06c17_text_export.jpeg" alt="" width="600" />
    </p>

---

11. Restart the dev server and reload the dashboard. The **Current Day** view now renders the IBM spotlight card, the four competitor cards with open/prev close/day range/52-week range/market cap/target fields, and matching live data.

    Click through the **Last 7 Days** and **Last Quarter** tabs to confirm the trend chart and area chart both render against live data.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/ddb240a9-aed7-4a4e-9579-649c3d099737/user_cropped_screenshot_13d7e9fd2d7c4b9697697f3cab4513f0_text_export.jpeg" alt="" width="600" />
    </p>

---

12. With the dashboards working, run one final validation pass to lock in test coverage and prepare the change for delivery. Copy and paste the following prompt:

    ```
    Add appropriate validation for the finance dashboard project. Create tests for the data transformation logic and at least one UI rendering path. Provide commands to run the application checks locally and summarize what success should look like.
    ```

    Bob will add missing unit and component tests, run the full lint/test/build gate, and confirm every gate is clean.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/f8e0ae09-67c4-46c1-ad79-3f922ae2d6fe/user_cropped_screenshot_ca520543f35e4eb78e93e876c4965497_text_export.jpeg" alt="" width="600" />
    </p>

---

13. Once validation is green, ask Bob to review the uncommitted diff and prepare the commit and pull request artifacts. Copy and paste:

    ```
    Review the uncommitted changes in this repository, summarize what was added for the finance dashboard, identify any missing pieces, and prepare a concise commit summary and pull request description for the initial application delivery.
    ```

    Bob will provide a change summary for every meaningful change, flag anything missing, and draft a commit message (plus a PR description.)

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/26db2324-6654-469d-ae82-1c2fdc36b061/user_cropped_screenshot_5515355dc41a48a98fd21255cec12266_text_export.jpeg" alt="" width="600" />
    </p>

---

14. Bob's PR description covers what shipped, how the data service is wired, how the mock/live seam works, and how to reproduce validation locally — everything a reviewer needs to approve the change.

    Copy the commit message and PR description — you'll paste them into GitHub at the start of **Part 2 | Evolve**.

    <p align="left">
    <img src="https://colony-recorder.s3.amazonaws.com/files/2026-07-31/7e8bcf71-2639-49ca-bd34-9aa803269a91/user_cropped_screenshot.webp?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIA2JDELI43356LVVTC%2F20260731%2Fus-west-1%2Fs3%2Faws4_request&X-Amz-Date=20260731T042036Z&X-Amz-Expires=900&X-Amz-SignedHeaders=host&X-Amz-Signature=f825e2557405422fc12cf46477faaa4b2c6b428d7c6f20ff202e4d432fa4d888" alt="" width="600" />
    </p>

---

## **iii. Reviewing the completed build**

Take a moment to review what Bob has produced end-to-end:

- The **data service** normalizes Yahoo Finance responses into a stable domain model that the UI can rely on
- The **mock/live seam** defaults to safe mock mode when the env var is missing or malformed
- The **three dashboard views** cover current-day snapshots, 7-day comparative performance, and quarterly area trends with a sortable ranking table
- The **error path** surfaces a clear retry banner instead of a crash when the proxy is unreachable
- The **test suite** covers data-layer mapping, mock fixture integrity, normalization arithmetic, view rendering, and error states
- The **commit and PR description** are ready to paste into GitHub as-is

Every layer remains extensible: adding a new company is a one-line change to the `COMPANIES` registry, and swapping the data source is contained to `yahooAdapter.ts`. That extensibility is what makes Part 2 possible.

---

## **iv. Key takeaways**

- Integration and view work extend the scaffold while leaving its structure intact.
- A mock/live seam that defaults to safe mode prevents accidental network calls in tests and dev.
- Screenshots are a first-class debugging input for Bob — attaching a failing UI is often faster than describing it.
- Test coverage belongs alongside the feature it validates, never deferred to a follow-up commit.
- Bob-generated commit messages and PR descriptions should be reviewed but rarely need to be rewritten.
- The reviewer artifact (PR description) is what Part 2 will iterate on — write it once, well.

The next module hands the codebase over to GitHub. In <a href="https://bienko.github.io/bob-sdlc/evolve-1/" target="_blank">**Part 2 | Evolve**</a>, you will push this branch to a real repository, open an issue for a new feature, and let Bob take the request all the way to a merge-ready pull request.

---