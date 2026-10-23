# 01 - Test
Identify untested behaviors in the target, then write and iterate tests until they pass with modern testing practices.

## Input
The scope to cover, a feature, module, or file glob, from the arguments.

## Output
The behaviors found and, per behavior, a passing test with its file path and the break it fails under, marked `run` or `reasoned`, or a `pending` entry with a one-line reason when deliberately skipped.

## Process
1. **List.** Enumerate the untested behaviors in the target area. Reason from the existing ones, score each from 0 (not needed) to 5 (critical core flow), group by section, prioritize by score and impact, and show a minimal bullet list.
2. **Approve.** Wait for user approval before generating any test. A behavior the user declines becomes a `pending` entry with a one-line reason, not tested.
3. **Generate.** Write the initial test for the highest-priority behavior, applying current testing practices and project conventions.
4. **Iterate.** Run the test. On failure, analyze it, improve the test, and repeat. On pass, check it against every quality criterion below and improve it until it meets them all.
5. **Next.** Move to the next behavior and repeat from Generate until the list is exhausted.

## Quality criteria
The project's testing commands, frameworks and conventions stay authoritative; these criteria hold within them, whatever the language.

- **Observable behavior.** Assert what a caller or user can observe: a returned value, a state change, an output, an error. A test that checks only internal structure, or only which collaborators were called when that call is not the behavior, is not done.
- **Real subject.** The behavior under test runs for real. Never replace its own logic with a mock, stub, fake or spy; the boundaries it crosses follow Doubles at seams.
- **Doubles at seams.** Introduce a double only at a boundary: network, filesystem, clock, randomness, a third-party service, or a seam the project already defines. Everywhere else, use real domain and pure objects.
- **Interaction as behavior.** Assert that a collaborator was called only when that call is the behavior itself, such as sending a message or emitting an event, and let the test's name say so.
- **No tautology.** Reject a test whose expected value only echoes what a double was configured to return, or whose only assertion is that a double was called when that call is not the behavior. It passes whatever the subject does.
- **Effectiveness.** Name the break that would defeat the intended behavior, such as a removed branch, an inverted condition, or an input returned unchanged, and check that the test fails under it. When it is cheap, apply the break, watch the test fail, revert it, then rerun the test and confirm it passes on the unchanged subject; this is a `run` check, anything less is `reasoned`. A test no break can fail protects nothing: strengthen it. Use mutation testing only when the project already configures it; never install a tool for this.

## Test
- Every behavior in the approved list has a corresponding test in the project suite that passes, with its file path recorded.
- Every such test meets each quality criterion and names the break it fails under, marked `run` or `reasoned`.
- Each deliberately skipped behavior has a `pending` entry with a one-line reason.
