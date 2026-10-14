# Commit Message Guide: Conventional Commits

*A complete guide to the current industry-standard way of writing commit messages (feat, fix, chore, and more). Based on Conventional Commits 1.0.0.*

## 1. Summary

The industry standard today is **Conventional Commits 1.0.0**. It is a lightweight convention for writing commit messages so that both people and tools can read them. It is based on the Angular commit guidelines, and 1.0.0 is still the latest released version of the specification.

The basic format:

```
<type>[optional scope][!]: <description>

[optional body]

[optional footer(s)]
```

Example:

```
feat(auth): add refresh token endpoint

Access tokens now expire after 15 minutes. Clients can request a new
access token using the refresh token instead of logging in again.

Closes #142
```

The most important points:

- Start with a **type** such as `feat`, `fix`, `docs`, `refactor`, `test`, `build`, `ci`, `chore`.
- Add an optional **scope** in parentheses to show the area of the code, such as `(auth)` or `(api)`.
- Write a short **description** in the imperative mood: "add", not "added" or "adds".
- Use the **body** to explain *why* the change was made.
- Use **footers** for issue references and for `BREAKING CHANGE:`.
- Add `!` before the colon to mark a breaking change, such as `feat(api)!: remove v1 endpoints`.

## 2. Why Commit Messages Matter

A commit message is documentation that stays next to the code forever. Good messages help in these situations:

- **Code review:** reviewers understand the intent before reading the diff.
- **Debugging:** `git log`, `git blame`, and `git bisect` are only useful if the messages are clear.
- **Onboarding:** new team members can read the history to learn how the project evolved.
- **Automation:** tools can create changelogs, decide the next version number, and trigger releases.
- **Communication:** the history tells the team what changed and why, without opening every pull request.

A message like `fix stuff` or `updates` gives none of these benefits. A message like `fix(cart): prevent negative quantity on update` gives all of them.

## 3. The Standard: Conventional Commits 1.0.0

Conventional Commits became the de facto standard because it is simple, it works with Semantic Versioning (SemVer), and it has strong tool support. Many open source projects and companies use it, directly or with small changes.

The specification only requires two types:

- `fix` means a bug fix, and it maps to a **PATCH** release.
- `feat` means a new feature, and it maps to a **MINOR** release.
- Any commit with a **breaking change** maps to a **MAJOR** release, whatever its type.

All other types (`docs`, `chore`, `refactor`, and so on) are **not mandated by the specification**. They come from the Angular convention and from the `@commitlint/config-conventional` preset, and they are widely used. Teams are free to add their own types.

## 4. Message Anatomy

```
type(scope)!: short description      <- header (one line)
                                     <- blank line
Body paragraph(s) explaining what    <- optional body
and why.
                                     <- blank line
BREAKING CHANGE: details             <- optional footer(s)
Refs: #123
```

### Rules from the specification

1. The header starts with a **type** (a noun such as `feat` or `fix`), then an optional scope, an optional `!`, and then a **required colon and space**.
2. The **description** comes immediately after the colon and space.
3. A longer **body** may start one blank line after the description. It is free-form and can have several paragraphs.
4. One or more **footers** may come one blank line after the body.
5. Each footer is a word token, then ` :  ` or `  # `, then a value. Tokens use `-` instead of spaces (for example `Reviewed-by`), except `BREAKING CHANGE`, which is written with a space.
6. A breaking change is shown with `!` before the colon, with a `BREAKING CHANGE:` footer, or both.
7. `BREAKING CHANGE` must be in **uppercase**. `BREAKING-CHANGE` has the same meaning as `BREAKING CHANGE` in a footer.
8. Types other than `BREAKING CHANGE` are case-insensitive, but lowercase is the common practice and should be used consistently.

## 5. Commit Types

| Type | Use it for | SemVer effect |
| --- | --- | --- |
| `feat` | A new feature for the user or the API consumer | MINOR |
| `fix` | A bug fix | PATCH |
| `docs` | Documentation-only changes (README.md file, API docs, comments) | none |
| `style` | Formatting only: whitespace, semicolons, code style. No logic change | none |
| `refactor` | Code change that neither fixes a bug nor adds a feature | none |
| `perf` | A change that improves performance | none (many teams treat it as PATCH) |
| `test` | Adding or correcting tests | none |
| `build` | Build system or dependency changes (package manager, project files, Dockerfile) | none |
| `ci` | CI/CD configuration and scripts (GitHub Actions, Azure Pipelines, Jenkins) | none |
| `chore` | Maintenance work that does not fit another type and does not change source or tests behavior | none |
| `revert` | Reverts a previous commit | depends on what is reverted |

Notes:

- Any type becomes a **MAJOR** change when it includes `!` or a `BREAKING CHANGE:` footer.
- Tools differ in which types appear in the changelog. By default, many tools show only `feat` and `fix` (plus breaking changes), and sometimes `perf` and `revert`. This is configurable.
- Do not invent too many custom types. The more types you have, the harder it is for the team to choose correctly.

### Quick decision guide

Ask these questions in order:

1. Does the user or API consumer see **new behavior**? Use `feat`.
2. Does the user or API consumer see **corrected behavior**? Use `fix`.
3. Is it only **documentation**? Use `docs`.
4. Is it only **tests**? Use `test`.
5. Is it **formatting only**, with no logic change? Use `style`.
6. Is it a **performance improvement**? Use `perf`.
7. Is the code **restructured without changing behavior**? Use `refactor`.
8. Does it change the **build or dependencies**? Use `build`.
9. Does it change the **CI/CD pipeline**? Use `ci`.
10. Anything else (housekeeping, tooling, config)? Use `chore`.

### Commonly confused types

- **`chore` vs `build`:** Updating a dependency version in a project file is `build(deps)`. General housekeeping, such as updating the .gitignore file, is `chore`. Many teams use `chore(deps)` for dependency updates, which is also acceptable. Choose one and be consistent.
- **`refactor` vs `style`:** `style` changes only how the code looks (formatting, whitespace). `refactor` changes how the code is structured.
- **`fix` vs `refactor`:** If the behavior changes because a bug is corrected, it is `fix`. If the behavior stays exactly the same, it is `refactor`.
- **`feat` vs `perf`:** Faster performance of an existing feature is `perf`. A new capability is `feat`.
- **`docs` vs `chore`:** Changes to documentation content are `docs`. Changes to the tools that build the documentation are usually `build` or `chore`.
- **`ci` vs `build`:** `ci` is for the pipeline definition files. `build` is for how the software is compiled and packaged.

## 6. Scope

The scope tells the reader which part of the project is affected. It is a noun in parentheses directly after the type.

```
feat(parser): add support for arrays
fix(auth): handle expired session token
docs(readme): add setup instructions
build(deps): bump Newtonsoft.Json to 13.0.3
```

Guidelines:

- Use a **short noun**, in lowercase, with no spaces. Examples: `api`, `auth`, `ui`, `db`, `deps`, `docker`.
- Base scopes on **modules, layers, packages, or features**, not on file names or ticket numbers.
- Keep a **short, agreed list** of scopes for the team, and write it in the contributing guide.
- Scope is **optional**. If a change touches the whole project or many areas, omit it.
- For monorepos, use the package or service name as the scope, such as `feat(billing-service): ...`.

## 7. The Description (Subject Line)

The description is the most important part. Many tools and screens show only this line.

- **Use the imperative mood**, as if giving a command: "add", "fix", "remove", "update". A useful test: the sentence *"If applied, this commit will ..."* should make sense.
- **Keep it short.** A common limit is **50 characters** as a target and **72 characters** as the maximum for the whole header. The `@commitlint/config-conventional` preset allows up to 100 characters for the header.
- **Start with a lowercase letter** after the colon (for Conventional Commits).
- **Do not end with a period.**
- **Be specific.** Describe what changed, not how you feel about it or how hard it was.
- **Do not repeat the type** in the description. Write `fix: handle null user`, not `fix: fix null user bug`.

Examples:

| Weak | Better |
| --- | --- |
| `fix: fixed bug` | `fix: handle null reference in order total` |
| `feat: new stuff` | `feat(search): add filter by date range` |
| `chore: updates` | `chore: update .gitignore file for build output` |
| `refactor: changed code.` | `refactor(orders): extract pricing logic to service` |
| `docs: Updated README` | `docs: add local setup steps to README.md file` |

## 8. The Body

The body answers **why**. The diff already shows *what* changed, so use the body to give the context that the diff cannot show.

A good body explains:

- The **problem** or reason for the change.
- Why you chose **this solution** over others.
- **Side effects**, limits, or things reviewers should know.
- Any **migration steps** or follow-up work.

Guidelines:

- Leave **one blank line** between the header and the body.
- **Wrap lines at about 72 characters** (the Git convention). Some teams allow 100.
- Use plain paragraphs. Short bullet lists are acceptable for several related points.
- Write in the **imperative or present tense**, and stay consistent.

Example:

```
fix(cart): prevent negative quantity on update

The quantity input accepted negative numbers, which produced negative
order totals. The API now validates the value and returns 400 when the
quantity is lower than 1.

The UI validation was not changed because the API is the source of
truth for this rule.
```

## 9. Footers

Footers hold structured metadata. They come after the body, separated by a blank line.

Common footers:

```
Closes #123
Fixes #124
Refs: #125, #126
Reviewed-by: Name
Co-authored-by: Name <name@example.com>
BREAKING CHANGE: the id field is now a string
```

- **Issue links:** `Closes #123`, `Fixes #123`, and `Resolves #123` close the issue automatically on GitHub and GitLab when the commit reaches the default branch. Use `Refs #123` to link without closing. For Jira or Azure Boards, use the work item key your team agreed on, such as `Refs: PROJ-123`.
- **Co-authors:** use `Co-authored-by:` so each person gets credit. GitHub shows it in the commit.
- **Sign-off:** `Signed-off-by:` is used by projects that require the Developer Certificate of Origin (`git commit -s`).
- **Breaking changes:** see the next section.

## 10. Breaking Changes

A breaking change is any change that forces users or consumers to change their code or setup. It triggers a **MAJOR** version bump. There are two ways to mark it, and they can be used together.

**Option 1: `!` before the colon**

```
feat(api)!: remove deprecated v1 endpoints
```

**Option 2: `BREAKING CHANGE:` footer**

```
feat(api): change response format of /orders

BREAKING CHANGE: the response now returns an object with an items
array instead of a plain array. Clients must read response.items.
```

**Both together (recommended for important changes):**

```
feat(api)!: change response format of /orders

BREAKING CHANGE: the response now returns an object with an items
array instead of a plain array. Clients must read response.items.
```

Notes:

- The footer should explain **what breaks and how to migrate**. This text is used in release notes.
- When `!` is used without a footer, the description itself must describe the breaking change.
- Any type can be breaking, such as `refactor!:` or `build!:`.

## 11. Semantic Versioning Mapping

Given a version `MAJOR.MINOR.PATCH`:

| Commit | Version bump | Example |
| --- | --- | --- |
| `fix:` | PATCH | 1.4.2 to 1.4.3 |
| `feat:` | MINOR | 1.4.2 to 1.5.0 |
| Any type with `!` or `BREAKING CHANGE:` | MAJOR | 1.4.2 to 2.0.0 |
| `docs:`, `chore:`, `ci:`, `test:`, `style:`, `refactor:` | No release by default | none |

For versions below 1.0.0 (`0.y.z`), tools often treat breaking changes as MINOR bumps. Check the setting of your release tool.

## 12. Good and Bad Examples

### Good

```
feat: add dark mode toggle to settings page
```

```
fix(payment): round tax to two decimals before total
```

```
perf(search): cache category list for 5 minutes

The category query ran on every keystroke and caused high database load.
Caching the result reduces the average response time from 220 ms to 30 ms.
```

```
refactor(orders): split OrderService into pricing and shipping
```

```
build(deps): bump Polly from 8.2.0 to 8.4.0
```

```
ci: run unit tests on pull requests
```

```
docs: add API authentication guide to docs folder
```

```
revert: feat(search): add filter by date range

This reverts commit 1a2b3c4d. The filter caused timeouts on large
datasets. It will return after the query is optimized.
```

### Bad

| Bad message | Problem |
| --- | --- |
| `update` | No type, no information |
| `fix` | Does not say what was fixed |
| `WIP` | Not meaningful in shared history |
| `fixed the bug that John reported yesterday` | Wrong tense, not specific, relies on outside context |
| `feat: Added new feature and fixed 3 bugs and updated docs` | Several unrelated changes in one commit |
| `FEAT: ADD LOGIN.` | Uppercase, period at the end |
| `chore: stuff` | Vague |
| `final version v2 (really final)` | Informal, no information |

## 13. Special Cases

### Reverts

The specification does not define revert behavior. Common practice:

- Git's default message is `Revert "original subject"`, and it includes `This reverts commit <hash>.` in the body.
- To keep the history parseable, many teams write `revert: <original header>` and add `Refs: <hash>` or the sentence `This reverts commit <hash>.` in the body.

### Merge commits

Git creates messages like `Merge branch 'x' into main`. These are usually left as they are. Many teams prefer a **linear history** and use squash or rebase merges instead, so merge commits do not appear.

### Squash merges

When a pull request is squash-merged, the **PR title becomes the commit message**. For this reason:

- Write the **PR title in Conventional Commits format**, such as `feat(auth): add refresh token endpoint`.
- Put the PR description in the commit body.
- Use a check (for example a GitHub Action that validates PR titles) to enforce the format.
- On GitHub, set the repository option so squash merges use the PR title as the default commit message.

With this approach, developers can use any style in their working branches, and only the final squashed commit must follow the standard.

### Work in progress, fixup, and temporary commits

- Use `git commit --fixup <hash>` and `git rebase -i --autosquash` to clean up commits **before** the pull request is merged.
- Do not leave `WIP`, `temp`, or `asdf` commits in the shared main branch.

### Dependency updates by bots

- **Dependabot:** set `commit-message.prefix` in the dependabot.yml file, such as `chore(deps)` or `build(deps)`.
- **Renovate:** can use semantic commit prefixes (`chore(deps): update ...`) through its configuration.

### Release commits

Release tools often create commits such as `chore(release): 2.1.0`. Let the tool write these. Do not edit them by hand.

### Initial commit

The common convention is `chore: initial commit` or `feat: initial project setup`.

### Multiple types in one change

If one change needs two types, it is usually **two commits**. If this is not possible, use the type with the highest impact (`feat` before `fix`, and a breaking change above all), and describe the rest in the body.

## 14. Atomic Commits

Conventional Commits work best with **atomic commits**: one logical change per commit.

- Each commit should **build and pass tests** on its own.
- Do not mix a refactor, a bug fix, and a feature in one commit.
- Commit formatting-only changes separately, so they do not hide real changes in the diff.
- Use `git add -p` to stage parts of a file when one file has several unrelated changes.
- If you cannot describe the commit in one short line without using "and", it is probably too big.

## 15. Tools and Automation

A convention only works if it is easy to follow and is checked automatically.

### Writing the message

- **Git commit template:** create a .gitmessage file with the format and hints, then run `git config commit.template .gitmessage`. Git will open the template every time you commit without `-m`.
- **Commitizen** (`cz-git`, `commitizen`): an interactive prompt that asks for type, scope, and description.
- **IDE support:** extensions for VS Code and JetBrains IDEs can help build the message.

Example .gitmessage file:

```
# <type>(<scope>)!: <short description, imperative, max 72 chars>
#
# Types: feat fix docs style refactor perf test build ci chore revert
#
# Body: explain WHY the change was made (wrap at 72 chars)
#
# Footer: Closes #123 | BREAKING CHANGE: <what breaks and how to migrate>
```

### Checking the message

- **commitlint** with `@commitlint/config-conventional`: validates each message against the rules.
- **Husky** or a similar Git hook tool: runs commitlint on the `commit-msg` hook, so a wrong message is rejected before it is saved.
- **CI check:** run commitlint in the pipeline as well, because local hooks can be skipped with `--no-verify`.

Basic setup with Node.js tooling (it works in any repository, including non-JavaScript projects):

```
npm install --save-dev @commitlint/cli @commitlint/config-conventional husky
```

Create a commitlint.config.js file:

```
module.exports = { extends: ['@commitlint/config-conventional'] };
```

Then add a `commit-msg` hook in the .husky folder that runs `npx --no -- commitlint --edit $1`.

### Using the messages

- **semantic-release:** fully automatic versioning, changelog, and publishing.
- **release-please** (Google): opens a release pull request with the version bump and the CHANGELOG.md file update.
- **conventional-changelog:** generates the CHANGELOG.md file from the history.
- **git-cliff:** a configurable changelog generator.
- **cocogitto (`cog`):** a Conventional Commits toolbox written in Rust, with checks, changelog, and version bump.
- **For .NET repositories:** Versionize and GitVersion can use Conventional Commits to calculate versions and generate changelogs. Check their current documentation for the exact setup.

## 16. Writing Commit Messages with AI Tools

AI coding tools such as Claude Code, GitHub Copilot, and others can draft commit messages. This is useful, but it needs review.

- **Give the format in your instructions.** Put the rules (Conventional Commits, your scope list, your language) in the project instructions file, such as the CLAUDE.md file, so every generated message follows the standard.
- **Always read the result.** AI describes what the diff does, but it may miss *why* the change was made. Add the reason to the body yourself.
- **Check the type.** AI often chooses `feat` or `chore` too easily. Confirm it matches the real effect of the change.
- **Keep commits small.** AI messages are much better when the diff is one logical change.
- **Do not accept long generic text.** Remove filler such as "this commit improves the codebase".
- **Follow your team's policy** on AI attribution lines such as `Co-authored-by:` footers.

## 17. Alternatives and Related Conventions

| Convention | Description | When it fits |
| --- | --- | --- |
| **Conventional Commits** | Structured `type(scope): description` format | Default choice for most teams, especially with automated releases |
| **Angular commit guidelines** | The original source of the types and scopes | Projects that follow Angular rules |
| **Gitmoji** | Uses emojis as the type, such as a bug emoji for fixes | Teams that like a visual style (harder for tools and for some terminals) |
| **Plain imperative style (Git / Linux kernel style)** | Short imperative subject, detailed body, no type prefix | Large projects that value long explanations over automation |
| **Ticket-prefix style** | `PROJ-123: description` | Companies that track all work in Jira or Azure Boards (can be combined with Conventional Commits in the footer) |

Conventional Commits and ticket references work well together:

```
fix(orders): handle null shipping address

Refs: PROJ-123
```

## 18. Team Adoption Guide

1. **Agree on the standard.** Choose Conventional Commits 1.0.0 and the list of allowed types.
2. **Define scopes.** Write a short list in the CONTRIBUTING.md file.
3. **Document the rules.** Include 5 to 10 real examples from your own project.
4. **Add a .gitmessage template** and share the setup command.
5. **Add commitlint and a Git hook,** and repeat the check in CI.
6. **Validate pull request titles** if you use squash merges.
7. **Automate releases** with semantic-release, release-please, or a similar tool once the habit is stable.
8. **Review messages in code review,** the same way you review code.
9. **Be flexible at the start.** The specification itself says it is not the end of the world if one commit does not follow the format. Aim for steady improvement, not perfection.

## 19. Common Mistakes

- Using the past tense ("added", "fixed") instead of the imperative.
- Writing vague descriptions such as "update code" or "fix issue".
- Using `feat` for everything, which breaks changelogs and version bumps.
- Forgetting to mark a breaking change, so consumers get a surprise.
- Mixing unrelated changes in a single commit.
- Using `chore` as a place for anything unclear.
- Writing a very long subject line with all details in it, instead of using the body.
- Putting the ticket number as the only information.
- Describing *how* (the code) and not *why* (the reason).
- Using inconsistent scope names, such as `auth`, `authentication`, and `login` for the same area.
- Skipping the blank line between header, body, and footers (tools can not parse the message correctly).

## 20. Cheat Sheet

**Format**

```
type(scope)!: imperative description under 72 chars

Body explaining why, wrapped at 72 chars.

Closes #123
BREAKING CHANGE: what breaks and how to migrate
```

**Types**

| Type | One-line meaning |
| --- | --- |
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting only |
| `refactor` | Restructure, same behavior |
| `perf` | Faster or lighter |
| `test` | Tests only |
| `build` | Build system, dependencies |
| `ci` | CI/CD pipeline |
| `chore` | Other maintenance |
| `revert` | Undo a previous commit |

**Version impact:** `fix` = PATCH, `feat` = MINOR, breaking change (`!` or `BREAKING CHANGE:`) = MAJOR.

**Remember:** one logical change per commit, imperative mood, lowercase, no period, explain *why* in the body.

## 21. Sources

- Conventional Commits 1.0.0 specification: conventionalcommits.org/en/v1.0.0
- Conventional Commits specification repository on GitHub (conventional-commits/conventionalcommits.org)
- Angular commit message guidelines (the origin of the type list)
- commitlint and `@commitlint/config-conventional` documentation
- Git documentation on commit message conventions and the 50/72 rule
