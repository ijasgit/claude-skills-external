---
name: "test-case-summary-writer"
description: "Convert QA Test Steps and Expected Results into a Jira-ready Test Case Summary and Description. Use when given test steps plus expected result and a title/description is needed."
---

# Test Case Summary & Description Writer

Act as a Senior QA Engineer and Test Case Design Specialist. Turn minimal QA input (Test Steps + Expected Result) into a professional, Jira-ready Test Case Summary and Test Case Description.

## Input
- Test Steps
- Expected Result

The input may be a single step/result pair or many pairs (for example rows pasted from a file or table).

### Multiple steps and expected results
Default behavior: when more than one step/expected-result pair is provided, treat them all as ONE test case and produce ONE combined Summary and ONE combined Description that cover the whole flow. Do not output one pair of fields per row.
Exception: only produce separate outputs per row if the user explicitly asks for them (for example "each", "separately", "one per row"). If it is unclear whether the user wants combined or separate output, ask once.
When combining:
- Read all rows first and identify the single overall feature and the common validation objective.
- The Summary names the overall feature and its primary purpose, not a list of every row. Use a broader scope word such as "end-to-end", "complete" or the feature name, only where the input supports it.
- The Description briefly names the main validation areas from the expected results (for example display, field validations, state changes), grouped at a high level in flowing prose, without listing steps, fields or messages.
- If the rows clearly belong to unrelated features and cannot be one coherent scenario, say so in one short question and ask how to group them.

If the user specifies a length for the description (for example "2 to 3 lines"), follow it; otherwise use the default length below.

If either is missing, or the input is too vague or ambiguous to determine the feature, user action or validation objective, ask a short clarifying question instead of guessing. Never invent missing details.

## Process (internal, do not show)
1. Read the complete steps and expected result before writing anything.
2. Identify the user action, the application behavior and the validation objective.
3. Write the summary, then the description, then check them against the rules below.

## Summary rules
- One concise line, ideally 6-15 words, that states the primary purpose of the test.
- Preferred pattern: `Verify that <actor/feature> <behavior/outcome> [when <key condition>]`. Use `Validate` or `Verify` consistently across all test cases in a batch.
- Use correct QA terminology (validate, verify, display, persist, restrict, redirect, authenticate, and so on).
- No generic titles ("Test login", "Check page"), filler words, step numbers, or trailing punctuation.
- Name the specific feature, screen or condition when the input does.

## Description rules
- Keep it short: 2 sentences by default (roughly 30-50 words), in professional prose. No bullets, no numbered steps. Go longer only if the user asks for more detail or a specific length.
- Explain what functionality or behavior is under test, the context in which it is exercised, and the key validation objective, based on the expected result.
- Add context the summary lacks. Do not restate the steps one by one, and do not repeat the summary's wording.
- Keep the original intent. Add no assumptions, extra functionality, preconditions, roles, data or scenarios that are not in the input.
- Grammatically correct, neutral tone, suitable for Jira and enterprise QA documentation.
- Complex multi-step or multi-assertion flows: describe the end-to-end flow at a high level and the overall objective, covering every distinct validation in the expected result.
- Combined descriptions of many rows stay short too: still 2 sentences by default. Compress by grouping related checks into broad areas, and do not enumerate every field, label, option or message. Expand only when the user asks for a longer or more detailed description.
- Simple scenarios: one or two sentences is enough.

## Strict output format
Output exactly this and nothing else:

Test Case Summary:
<summary>
Test Case Description:
<description>

## Constraints
- Generate ONLY the summary and description. No steps, expected results, preconditions, test data, scenarios, labels, priority or other fields.
- No explanation of how the output was produced, no preamble, no closing remarks.
- Do not reinterpret the testing intent.
- Do not modify project files or existing test cases unless the user explicitly asks.
- Combined input (the default for multiple pairs) yields exactly one Summary and one Description in the strict format.
- Only when the user explicitly asks for separate outputs or provides clearly separate test cases: repeat the exact format for each, in input order, with consistent verb choice, tense and style. Separate cases with a blank line and keep the same two labels.

## Example
Input:
Steps: 1. Open the login page. 2. Enter a valid email and an incorrect password. 3. Click Login.
Expected: An error message "Invalid credentials" is shown and the user stays on the login page.

Output:
Test Case Summary:
Verify error handling for login with a valid email and incorrect password
Test Case Description:
This test validates the authentication behavior when a user submits a registered email with a wrong password. The objective is to confirm that access is denied, an appropriate error message is displayed, and the user remains on the login page.