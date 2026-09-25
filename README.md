# Approval Workflows — flexible steps prototype

A standalone, clickable prototype of changes to INGENIOUS.BUILD approval workflows in Project Settings. It's a stakeholder demo, not production code. There's no backend and nothing is saved between reloads.

Live version: https://claude.ai/artifact/FpGVuTf52NMZJdFXLe1AER

## Run it

Open `index.html` in a browser. There's no build step. It loads `tokens.css`, `components.css` and `icons.js` from the Ingenious design system, which are copied next to it.

## What it changes

One settings page, **Approval Workflows**, replaces the separate Construction Administration and Financial menu items. It has two tabs:

- **Financial Approval Workflows:** Budget (Owner / Your Budget), Budget Reallocations, Contracts, Contract Changes, Invoices
- **Construction Approval Workflows:** Submittals, Drawings, Inspections

Each workflow is a list of **steps**. New per step:

| Feature | How it works |
|---|---|
| Anyone with a role | Pick a role and choose **Anyone with role**. Anyone holding that role on the project can act, and the first to act completes the step. |
| More than one person | Add several people, then choose **All** (everyone must approve) or **Any** (the first approval moves it on). |
| Optional step | Turn **Required** off. Those people are notified and can approve, but the next step doesn't wait for them. |
| Submittal types | Submittal steps are **Submitter** or **Approver**. Other modules keep Approver / Reviewer. |
| Review time | Days a step has to act, counted from when it becomes that step's turn. Blank means no deadline. |

"Add User" is renamed **Add Step**. A path line under each workflow summarises its steps.

## Demo walkthrough

1. **Budget → Owner Budget:** step 1 needs Joel Finnerty **and** JD Martinez. Step 2 is anyone with the Cost Manager role. Step 3 is an optional reviewer.
2. Click **Simulate approval**. Approve as Joel: step 1 stays open until JD also approves. Hannah Cole is notified at step 3 but doesn't hold anything up.
3. **Invoices:** step 2 is Joel **or** Marcus with a Greater than $ 50,000.00 limit. In the simulator, change the amount to see that step skipped or included.
4. **Construction → Submittals:** the GC submits, then anyone with the Architect role approves, then JD or Priya.

## Open questions

- Overdue review time: flag only, or escalate (notify someone else, auto-skip optional steps)?
- Should one person be allowed to be both Submitter and Approver on the same submittal workflow?

## Where this maps in the product

- Financial settings: `frontend/src/io-modules/financial-approvals/settings/` (`SingleWorkflow.vue`, `SingleApprover.vue`)
- Construction settings: `frontend/src/io-modules/project-settings-construction-administration-approvals/`
- Approval engine: `app/Modules/FinancialApprovalsV2/`
