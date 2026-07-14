# ⚙️ Setting up `lowlighter/metrics` (one-time, ~5 min)

This makes `github-metrics.svg` in the README auto-update every day with your real stats — languages, contribution isocalendar, recent activity, achievements, starred repos, and lines-of-code changed.

## 1. Create a Personal Access Token (PAT)

1. Go to **GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)** → [direct link](https://github.com/settings/tokens/new)
2. Note: `metrics-token`
3. Expiration: choose **No expiration** or a long duration (you'll need to regenerate it when it expires)
4. Scopes to check:
   - `repo` (full)
   - `read:user`
   - `read:org` *(optional — only if you want org stats)*
5. Click **Generate token** and **copy it immediately** (you won't see it again)

> ⚠️ Do **not** use a fine-grained token — the metrics action currently only supports classic PATs.

## 2. Add the token as a repo secret

1. Go to your `Haransinka0/Haransinka0` repo → **Settings → Secrets and variables → Actions**
2. Click **New repository secret**
3. Name: `METRICS_TOKEN`
4. Value: paste the token you copied
5. Save

## 3. Add the workflow file

In your repo, create the file **`.github/workflows/metrics.yml`** and paste in the contents of the `metrics.yml` file provided alongside this guide.

- GitHub UI path: **Add file → Create new file** → type `.github/workflows/metrics.yml` as the filename → paste content → commit.

## 4. Run it

1. Go to the **Actions** tab of your repo
2. Select the **Metrics** workflow on the left
3. Click **Run workflow** (top right) to trigger it manually the first time
4. Wait ~30–60 seconds — it will commit a new file, `github-metrics.svg`, to your repo root

## 5. Confirm the README picks it up

The README already references it as:

```md
<img src="./github-metrics.svg" width="100%" alt="Haran's GitHub metrics"/>
```

Once `github-metrics.svg` exists at the repo root, this will render automatically — no further edits needed. It will silently refresh once a day via the `schedule` trigger in the workflow, and also re-runs every time you push to `main`.

## Customizing later

All options live in `.github/workflows/metrics.yml` under the `with:` block. Some good ones to try:

| Plugin | What it adds |
|---|---|
| `plugin_isocalendar` | 3D-style isometric contribution calendar |
| `plugin_languages` | Language breakdown pie/bar |
| `plugin_activity` | Recent commits/PRs/issues feed |
| `plugin_achievements` | GitHub "achievement" badges |
| `plugin_stars` | Your top starred repos |
| `plugin_lines` | Lines of code added/removed over time |
| `plugin_habits` | Coding habits (active hours, commit size) — needs `read:user` scope only |

Full plugin catalog: https://github.com/lowlighter/metrics/blob/master/README.md

## Troubleshooting

- **Workflow fails with "bad credentials"** → the `METRICS_TOKEN` secret is missing, misnamed, or the PAT expired — regenerate and re-add it.
- **SVG doesn't show in README** → make sure the workflow actually ran successfully (check the Actions tab) and that `github-metrics.svg` exists at the repo root, not in a subfolder.
- **"fine-grained personal access tokens are unsupported"** → you generated the wrong token type; go back to step 1 and use **Tokens (classic)**.
