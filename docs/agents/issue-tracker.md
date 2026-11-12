# Issue tracker: GitHub

Issues and specs live in GitHub Issues for `wzt3309/nor-studio`.

Use the `gh` CLI from this repository. For multiline bodies, write the content
to a file and pass `--body-file`.

## Operations

- Create: `gh issue create --title "..." --body-file <path>`
- Read: `gh issue view <number> --comments`
- List: `gh issue list --state open --json number,title,body,labels`
- Comment: `gh issue comment <number> --body-file <path>`
- Apply labels: `gh issue edit <number> --add-label "<label>"`
- Remove labels: `gh issue edit <number> --remove-label "<label>"`
- Close: `gh issue close <number>`

When a skill says "publish to the issue tracker", create a GitHub issue.
When it says "fetch the relevant ticket", read the issue and its comments.

## Pull requests as a triage surface

**PRs as a request surface: no.**
