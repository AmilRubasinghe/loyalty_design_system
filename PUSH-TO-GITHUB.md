# Push this repo to GitHub

Step-by-step from zero to a live GitHub repository.

---

## Step 1 — Create the repo on GitHub

1. Go to [github.com/new](https://github.com/new) (make sure you're logged in as `sathila-18`)
2. Fill in:
   - **Repository name:** `loyalty_design_system`
   - **Visibility:** Public *(required so the release `.tgz` download URLs work without auth)*
   - **Initialize this repository with:** leave all checkboxes **off** (no README, no .gitignore, no licence — the repo already has them)
3. Click **Create repository**
4. GitHub shows a "Quick setup" page. Copy the repo URL — you'll use it in Step 3.

---

## Step 2 — Stage and commit everything

Open a terminal in the repo folder and run:

```bash
git add .
git status
```

Review the list — make sure no `.env` files or secrets are staged. Then commit:

```bash
git commit -m "Initial commit"
```

---

## Step 3 — Point the repo at GitHub

### Option A — HTTPS (simpler, no SSH key needed)

```bash
git remote add origin https://github.com/sathila-18/loyalty_design_system.git
```

When you push, GitHub will ask for your username and password.  
> Use a **Personal Access Token** as the password (GitHub no longer accepts your account password).  
> Create one at: **GitHub → Settings → Developer settings → Personal access tokens → Classic → New** with `repo` scope.

### Option B — SSH (no password prompts after setup)

```bash
git remote add origin git@github.com:sathila-18/loyalty_design_system.git
```

SSH must be set up first — see [GitHub SSH docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) if you haven't done it.

> **Multiple GitHub accounts on this machine?** You may have a custom SSH host alias (e.g. `github.com-work`). Use it instead:
> ```bash
> git remote add origin git@github.com-work:sathila-18/loyalty_design_system.git
> ```
> Check your alias in `~/.ssh/config`.

---

## Step 4 — Rename branch to `main` and push

GitHub defaults to `main`. Rename the local branch first, then push:

```bash
git branch -m master main
git push -u origin main
```

The `-u` flag sets `origin/main` as the upstream, so future pushes are just `git push`.

---

## Step 5 — Verify

Open `https://github.com/sathila-18/loyalty_design_system` in your browser.  
You should see all the files and the `main` branch.

---

## Step 6 — Enable Actions write permissions

The release workflow creates GitHub Releases. It needs write access.

1. Go to your repo → **Settings → Actions → General**
2. Under **Workflow permissions** → select **Read and write permissions**
3. Click **Save**

---

## Step 7 — Create the first release

```bash
git tag v0.1.0
git push --tags
```

Go to the **Actions** tab and watch the `Release` workflow run (~2 minutes).  
When it finishes, the release with the `.tgz` files appears at:

```
https://github.com/sathila-18/loyalty_design_system/releases/tag/v0.1.0
```

---

## Done — consumer install URLs

Once the release is live, anyone installs the packages with:

```bash
pnpm add https://github.com/sathila-18/loyalty_design_system/releases/download/v0.1.0/finspace-loyalty-tokens-0.1.0.tgz
pnpm add https://github.com/sathila-18/loyalty_design_system/releases/download/v0.1.0/finspace-loyalty-ui-0.1.0.tgz
```

No login. No `.npmrc`. No tokens.

---

## Checklist

- [ ] Repo created on GitHub (public, no init files)
- [ ] `git add .` and `git commit -m "Initial commit"`
- [ ] `git remote add origin <url>`
- [ ] `git branch -m master main`
- [ ] `git push -u origin main` — repo is live
- [ ] Actions write permissions enabled
- [ ] `git tag v0.1.0 && git push --tags` — first release triggered
- [ ] Release visible with both `.tgz` files attached
