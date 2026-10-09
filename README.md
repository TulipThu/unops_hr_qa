# UNOPS HR QA

Umbrella repository for UNOPS HR quality-assurance test suites. Each test suite lives in its **own Git repository** and is linked here as a **Git submodule**. This lets teams develop, review, and release tests independently while keeping a single entry point for the whole QA program.

## Submodules

| Submodule | Description | Repository |
|-----------|-------------|------------|
| `careersmarketplace-tests` | Playwright E2E tests for the Careers Marketplace | [TulipThu/careersmarketplace-tests](https://github.com/TulipThu/careersmarketplace-tests) |

## First-Time Setup (Maintainers)

Before collaborators can clone with submodules, each child repo must exist on GitHub. One-time steps for this repo:

1. **Create the child repo** on GitHub: [TulipThu/careersmarketplace-tests](https://github.com/new) (empty, no README).
2. **Push the child** from your local clone:

   ```bash
   cd careersmarketplace-tests   # your standalone clone
   git add README.md
   git commit -m "Add README and project documentation"
   git push -u origin main
   ```

3. **Push the parent** (from a clone of `unops_hr_qa` that includes the submodule):

   ```bash
   cd unops_hr_qa
   git commit -m "Add careersmarketplace-tests submodule and collaboration docs"
   git push origin main
   ```

After this, others can use `git clone --recurse-submodules` as documented below.

## Getting Started

### Clone everything (parent + all submodules)

```bash
git clone --recurse-submodules https://github.com/TulipThu/unops_hr_qa.git
cd unops_hr_qa
```

### Clone parent only, then fetch submodules later

```bash
git clone https://github.com/TulipThu/unops_hr_qa.git
cd unops_hr_qa
git submodule update --init --recursive
```

### Pull latest changes (parent + submodules)

```bash
git pull origin main
git submodule update --remote --merge
```

## Working with Submodules

Each subdirectory (e.g. `careersmarketplace-tests/`) is a full Git repo. **Commit and push inside the submodule directory**, not from the parent root.

```bash
# Example: work on Careers Marketplace tests
cd careersmarketplace-tests
git checkout -b feature/new-test
# ... edit, test, commit ...
git push origin feature/new-test
# Open PR in the child repo (careersmarketplace-tests)
```

After a child repo PR is merged, update the parent to pin the new commit:

```bash
cd careersmarketplace-tests
git checkout main
git pull origin main
cd ..
git add careersmarketplace-tests
git commit -m "Update careersmarketplace-tests submodule"
git push origin main
```

## Adding Your Own Test Suite (Collaborators)

Other teams can add their repo under this parent as a new submodule.

### Prerequisites

1. Your test project must already be a **Git repository on GitHub** (under your account or an org you control).
2. You need **write access** to this parent repo (`unops_hr_qa`), or open a PR with the submodule addition.

### Steps

1. **Push your repo to GitHub** (if not already):

   ```bash
   git remote add origin https://github.com/YOUR_USER/your-qa-suite.git
   git push -u origin main
   ```

2. **Add it as a submodule** from a clone of this parent repo:

   ```bash
   git clone https://github.com/TulipThu/unops_hr_qa.git
   cd unops_hr_qa
   git checkout -b add-your-qa-suite
   git submodule add https://github.com/YOUR_USER/your-qa-suite.git your-qa-suite
   ```

3. **Update this README** — add a row to the [Submodules](#submodules) table with your project name, description, and repo link.

4. **Commit and push** (or open a PR):

   ```bash
   git add .gitmodules your-qa-suite README.md
   git commit -m "Add your-qa-suite submodule"
   git push origin add-your-qa-suite
   ```

5. Open a **Pull Request** against `main` in `unops_hr_qa`. A maintainer will review and merge.

### Submodule naming

- Use a short, descriptive folder name (e.g. `careersmarketplace-tests`, `recruitment-api-tests`).
- Keep one test suite per submodule repo.

## Collaboration Overview

```
unops_hr_qa (parent)          ← submodule pointers, team onboarding docs
├── careersmarketplace-tests  ← child repo: own issues, PRs, CI
├── your-qa-suite             ← child repo: added by collaborators
└── ...
```

| Action | Where to do it |
|--------|----------------|
| Write or fix tests | **Child repo** — branch, PR, merge in the child |
| Pin a new test release | **Parent repo** — update submodule commit after child merge |
| Add a new test suite | **Parent repo** — PR adding submodule + README row |
| Report a bug in tests | **Child repo** issues (preferred) or parent if cross-cutting |

## Common Commands

```bash
# See submodule status
git submodule status

# Update one submodule to latest remote main
git submodule update --remote careersmarketplace-tests

# Run tests in a submodule (example)
cd careersmarketplace-tests && npm ci && npm test
```

## Maintainers

- Keep submodule URLs in `.gitmodules` pointing to stable GitHub repos.
- Review PRs that add or update submodules.
- Do not commit secrets or `.env` files in the parent or child repos.

## License

Each submodule may have its own license. See the individual repository for details.
