# Design Doc: Removal of Negative Balance and Debit Entry Rules from Doctor

## Context
Paisa includes a "Doctor" feature that performs diagnostic checks on the ledger to identify potential data entry errors. Two of these rules have been identified as too restrictive for certain user workflows:
1. **Negative Balance**: Flags any Asset account that goes negative.
2. **Debit Entry (Expense Accounts)**: Flags any expense entry that isn't a credit (e.g., refunds).

## Requirements
- Remove the "Negative Balance" rule from the Doctor's diagnostic checks.
- Remove the "Debit Entry" rule from the Doctor's diagnostic checks specifically for Expense accounts.
- Clean up the associated code (predicates and rule definitions) in the backend.

## Proposed Changes

### Backend (`internal/server/doctor.go`)
- Remove `ruleAssetRegisterNonNegative` from the `rules` slice in the `init()` function.
- Remove `ruleNonDebitAccount` from the `rules` slice in the `init()` function.
- Delete the `ruleAssetRegisterNonNegative` function definition.
- Delete the `ruleNonDebitAccount` function definition.
- Remove unused imports resulting from the removals (e.g., `accounting`, `lo`).

## Verification Plan
### Automated Tests
- Run existing backend tests to ensure no regressions: `go test ./internal/server/...`
- (Optional) If there are specific tests for `GetDiagnosis`, they should be updated or verified.

### Manual Verification
- Open the Paisa Web UI.
- Navigate to the "Doctor" page.
- Verify that "Negative Balance" and "Debit Entry" (for Expenses) are no longer listed as issues, even if the ledger contains such data.
