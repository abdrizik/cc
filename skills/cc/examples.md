# Conventional Commits — Examples

## Multi-paragraph Body with Footers

```
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are
obsolete now.

Reviewed-by: Z
Refs: #123
```

## Breaking Change with Both `!` and Footer

```
feat!: drop support for Node 6

BREAKING CHANGE: use JavaScript features not available in Node 6.
```

## Revert Commit

```
revert: let us never again speak of the noodle incident

Refs: 676104e, a215868
```

---

## Bad Commit Messages

| Bad                      | Problem                   | Fix                                               |
| ------------------------ | ------------------------- | ------------------------------------------------- |
| `updated stuff`          | No type, vague            | `fix(auth): resolve token refresh race condition` |
| `feat - add search`      | Wrong separator           | `feat: add search`                                |
| `feat:add search`        | Missing space after colon | `feat: add search`                                |
| `feat: Add search.`      | Capital, trailing period  | `feat: add search`                                |
| `feat: added search`     | Past tense                | `feat: add search`                                |
| `fix: various fixes`     | Vague, bundles changes    | Split into separate commits                       |
| `feat(api) add endpoint` | Missing colon             | `feat(api): add endpoint`                         |
| `feat(): add search`     | Empty scope               | `feat: add search`                                |

---

## Branch Naming

| Branch                      | Use Case      |
| --------------------------- | ------------- |
| `feat/user-avatar-upload`   | New feature   |
| `fix/auth-token-refresh`    | Bug fix       |
| `docs/api-reference`        | Documentation |
| `refactor/auth-middleware`  | Restructuring |
| `chore/update-dependencies` | Maintenance   |

Rules: `<type>/<short-kebab-description>`, 2-4 words, ticket if applicable: `feat/PROJ-123-avatar`.

---

## Validate Output

### Passing

```
✓ abc1234 feat(auth): add OAuth2 login
✓ def5678 fix(api): handle null response

2 of 2 commits pass
```

### With violations

```
✓ abc1234 feat(auth): add OAuth2 login
✗ def5678 "updated stuff" — missing type prefix
✗ ghi9012 "feat:add search" — missing space after colon

1 of 3 commits pass
```

---

## Squash Message

```
feat(auth): add OAuth2 login flow

- add OAuth2 provider configuration
- implement token exchange endpoint
- add login button to UI
- write integration tests for auth flow
```

---

## PR Output

### Title

```
feat(auth): add OAuth2 login flow
```

### Body

```
## Summary
Add OAuth2 login support with Google and GitHub providers.

## Changes
### Features
- add OAuth2 provider configuration
- implement token exchange endpoint

### Tests
- add integration tests for auth flow
```

---

## Revert Output

```
revert: add OAuth2 login flow

Refs: abc1234
```
