# Test and visualize

You are running a test analytics workflow for this repository. The user invoked `/test-and-visualize`.

## Goal

Run the test suite, then report:

- total tests run
- total successes
- total failures
- top 5 failing tests (if any)

Present results in a compact visual format.

## Steps

1. Run tests in machine-readable mode first:
   - Preferred: `yarn vitest run --reporter=json --outputFile .cursor/tmp/vitest-report.json`
   - If that fails due to argument support, run `yarn test` and parse stdout summary instead.
2. Always continue and produce the report even when tests fail.
3. From available output, compute:
   - `total`
   - `passed`
   - `failed`
4. Build `topFails` list (up to 5 items), each with:
   - test name
   - file path (if available)
   - short error/message snippet (single line)

## Output format

Use this exact section structure:

### Test Visualization

- Total: `<N>`
- Passed: `<N>`
- Failed: `<N>`

`Passed` bar: `[##########----------] <percent>%`  
(20-character bar: `#` for each 5% passed, `-` for remainder)

`Failed` bar: `[####----------------] <percent>%`  
(20-character bar: `#` for each 5% failed, `-` for remainder)

### Top 5 Failing Tests

- If there are failures: numbered list (max 5) with `name`, `file`, and short `error`.
- If there are no failures: `No failing tests.`

### Notes

- Include the test command used.
- If parsing was partial or fallback parsing was used, mention that explicitly.

## Quality guardrails

- Keep the report concise and scannable.
- Do not dump full stack traces unless the user asks.
- If JSON output exists but fields differ, adapt defensively and state assumptions in Notes.
