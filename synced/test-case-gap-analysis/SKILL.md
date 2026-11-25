---
name: "test-case-gap-analysis"
description: "Compare a user's existing test cases against a user story (Given/When/Then scenarios), find missing coverage, and return the full set with all newly added steps, data and results in bold."
---

# Test case gap analysis

Use when the user provides existing test cases plus a user story (attached or pasted) and wants missing coverage identified and added.

## Inputs
- Existing test cases (pasted table, TSV/Excel paste, or attachment). Treat them as the style sample: match their column layout, wording and level of detail.
- User story: scenarios in Given/When/Then form, often with field tables (Field | Type | Placeholder/Default | Validation) and option lists.
Read every attachment fully before starting. Attached files may be duplicates; treat identical files as one.

## Procedure
1. Split the user story into atomic requirement lines: every Given/When/Then/And line, every table row (field, type, placeholder/default, mandatory/optional), every option in a list, every message text, every conditional rule.
2. Build a coverage map: for each requirement line, find the existing test step (if any) that verifies it. Match on meaning, not exact wording. A requirement is covered only if both the action and the expected outcome are verified (for example the exact message text, default value, or option list).
3. Mark each line as Covered, Partially covered (add the missing detail), or Missing. Keep this marking: it decides which scenario headings and story lines are bolded in the output.
4. Do not modify or remove existing test cases unless needed for correctness (wrong message text, wrong default, contradicts the story). If something must change, change it minimally and say so in the change notes.
5. Add only what is missing. Never duplicate an existing step or re-test the same thing under different wording.
6. Place each new step at the position that follows the order of the lines in the user story, inside the most relevant existing test case. Create a new test case only when a whole scenario or feature has no home.

## Output format
- Return the complete set: existing test cases unchanged plus the additions.
- Bold every newly added item: the test step, the test data and the expected result of each new row (wrap the cell text in **...**). Existing text stays unbolded. When an existing row is edited for correctness, bold only the changed words and list it in the change notes.
- Before the test case tables, reproduce the user story with the changed parts in bold, so the user can see which requirements had gaps. Keep the story's original order and wording. Bold the heading of every scenario that needed an added or corrected test row, and bold the exact Given/When/Then/And lines or table rows that led to the addition. Leave scenarios and lines that were already fully covered in plain text. State above the story: "Bold = requirement with missing or partial coverage." If a scenario has no gaps, leave its heading plain.
- Use one table per test case with a heading line `## TC<n>: <name> (<Scenario ref>)` followed by `### Test Steps and Expected Results`.
- Columns: Test Steps | Test Data | Expected Results. If the user's existing cases use more columns (TCID, Description, Summary, Priority, Reporter, Labels, Component, Step, Data, Expected Result), keep that layout and leave the columns the user leaves blank.
- Deliver inline in the reply unless the user asks for a file. If they ask for a file, make an .xlsx in their column layout, with new cells in bold.
- After the tables, add a short Coverage summary: number of requirement lines checked, number already covered, number added, and a list of the added rows by scenario, and the scenarios whose story lines were bolded. Add a line for each assumption (sample test data, behaviour the story does not state).

## Writing rules
- Steps: one simple sentence per row, plain language, start with an action; use "Verify" instead of "Check". Combine related field checks into one step rather than one row per field, but keep every detail (placeholder, default, mandatory, type) in the expected result.
- Expected results start with "User should be able to ..." (for example view, select, enter, identify, verify).
- Test data: concrete and specific. Use values from the story when given (placeholders, defaults, messages, option lists). Where none is given, use clear sample values and mention them as assumptions. Use "Blank" for empty-input cases.
- Quote validation and error messages exactly as written in the story. If the story gives no message, say the validation is generic and do not invent text.
- Cover positive, negative (empty, invalid, duplicate, unauthorised), default-state and boundary cases that the story's lines imply. Do not add features the story does not mention; if an extra check seems valuable, list it separately as optional.
- Keep order the same as the user story lines within each scenario.

## Final check
Re-walk the coverage map: every bolded story line has a matching new bold row, every new bold row traces back to a bolded story line, every requirement line has a step, no step is repeated, new rows are bold, existing rows are untouched, and all message texts match the story character for character.