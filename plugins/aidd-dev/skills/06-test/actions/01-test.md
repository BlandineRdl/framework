# 01 - Test
Identify untested behaviors in the target, then write and iterate tests until they pass with modern testing practices.

## Input
The scope to cover, a feature, module, or file glob, from the arguments.

## Output
The behaviors found and, per behavior, a passing test with its file path and its effectiveness check, `run` or `reasoned` with its reason, or a `pending` entry with a one-line reason when deliberately skipped or blocked by a missing seam.

## Process
1. **List.** Enumerate the untested behaviors in the target area. Reason from the existing ones, score each from 0 (not needed) to 5 (critical core flow), group by section, prioritize by score and impact, and show a minimal bullet list.
2. **Approve.** Wait for user approval before generating any test. A behavior the user declines becomes a `pending` entry with a one-line reason, not tested.
3. **Generate.** Write the initial test for the highest-priority behavior, applying current testing practices and project conventions.
4. **Iterate.** Run the test. On failure, analyze it, improve the test, and repeat. On pass, check it against every quality criterion below and improve it until it meets them all.
5. **Next.** Move to the next behavior and repeat from Generate until the list is exhausted.

## Quality criteria
The project's testing commands, frameworks and conventions stay authoritative; these criteria hold within them, whatever the language.

- **Observable behavior.** Assert what a caller or user can observe: a returned value, a state change, an output, an error. A test that checks only internal structure is not done; which calls may be asserted is settled by Stub queries, mock commands.
- **Real subject.** The behavior under test runs for real. Never replace its own logic with a double; the boundaries it crosses follow Doubles at seams.
- **Doubles at seams.** Introduce a double only at a boundary: network, filesystem, clock, randomness, a third-party service, or a seam the project already defines, such as a port. Replacing the whole module that owns a boundary is such a seam; replacing part of the subject is not. Everywhere else, use real domain and pure objects. When a boundary offers no seam, never patch the subject to make one: the behavior becomes `pending` with `missing seam at <boundary>` as its reason, and the design change is left to its owner.
- **Stub queries, mock commands.** A double that feeds the subject data is a stub: never assert on it. A double that receives the subject's outgoing command, such as sending a message or writing a record, is a mock: asserting that call is the behavior, and the test's name says so.
- **No tautology.** Reject a test whose expected value only echoes what a stub was configured to return. It passes whatever the subject does.
- **Effectiveness.** Name the break that would defeat the intended behavior, such as a removed branch, an inverted condition, or an input returned unchanged. For a behavior scored 4 or 5, the check is `run`: copy the files the break touches, apply it and watch the test fail, restore them, confirm they match the copy, then rerun and watch the test pass. Below 4, a `reasoned` check may replace it, stating why the test fails under the break. A test no break can fail protects nothing: strengthen it. Use mutation testing only when the project already configures it; never install a tool for this.

## Test
- Every behavior in the approved list has a corresponding test in the project suite that passes, with its file path recorded.
- Every such test meets each quality criterion and records its effectiveness check: `run` for a behavior scored 4 or 5, `run` or `reasoned` with its reason below.
- Each behavior deliberately skipped or blocked by a missing seam has a `pending` entry with a one-line reason.
