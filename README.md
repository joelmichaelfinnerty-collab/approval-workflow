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
| Anyone with a role (financial only) | Pick a role and choose **Anyone with role**. Anyone holding that role on the project can act, and the first to act completes the step. |
| More than one person | Add several people, then choose **All** (all of the required approvers) or **Any** (any of the required approvers; the first approval moves it on). |
| Optional approvers | Each person's chip has a **Required** checkbox. Uncheck it and that person is notified and can approve, but the step doesn't wait for them. A role step has one checkbox for all role holders. A step with no required people doesn't hold up the workflow. |
| Submittal types | Submittal steps are **Submitter** or **Approver**. Other modules keep Approver / Reviewer. |
| Review time | Days a step has to act, counted from when it becomes that step's turn. Blank means no deadline. |

Construction workflows have no concept of a role: no Role column and no **Anyone with role** option. Steps name specific people. **Type** is the second column, after Sequence, on both tabs.

"Add User" is renamed **Add Step**.

## Demo walkthrough

1. **Budget → Owner Budget:** step 1 needs Joel Finnerty **and** JD Martinez. Step 2 is anyone with the Cost Manager role. Step 3 is Hannah Cole as an optional reviewer (Required unchecked).
2. Click **Simulate approval**. Approve as Joel: step 1 stays open until JD also approves. Hannah Cole is notified at step 3 but doesn't hold anything up.
3. **Invoices:** step 2 is Joel **or** Marcus with a Greater than $ 50,000.00 limit. In the simulator, change the amount to see that step skipped or included.
4. **Construction → Submittals:** Tom Becker or Lena Brooks submits, then Rachel Kim and Omar Haddad both approve, then JD or Priya, with Sam Ortiz notified as optional.

## Open questions

- Overdue review time: flag only, or escalate (notify someone else, auto-skip optional steps)?
- Should one person be allowed to be both Submitter and Approver on the same submittal workflow?

## Where this maps in the product

- Financial settings: `frontend/src/io-modules/financial-approvals/settings/` (`SingleWorkflow.vue`, `SingleApprover.vue`)
- Construction settings: `frontend/src/io-modules/project-settings-construction-administration-approvals/`
- Approval engine: `app/Modules/FinancialApprovalsV2/`
