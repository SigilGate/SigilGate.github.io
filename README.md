# SigilGate.github.io

Source repository for the [SigilGate GitHub Pages](https://sigilgate.github.io/) site, built with [Hugo](https://gohugo.io/) and the [Compose](https://github.com/info-tech-io/compose) theme.

## Branch structure

| Branch | Purpose |
|--------|---------|
| `main` | Content (`content/`), workflow, README, LICENSE |
| `hugo` | Hugo config, theme submodule, static assets |
| `gh-pages` | Built site (deployed automatically) |

## Local development

```bash
# Clone with submodules from hugo branch
git clone -b main https://github.com/SigilGate/SigilGate.github.io.git
cd SigilGate.github.io
git fetch origin hugo
git checkout origin/hugo -- config/ themes/ .gitmodules
git submodule update --init --recursive

# Run Hugo dev server
hugo server -D
```

## Deployment

Pushing changes to `content/**` on `main` triggers the GitHub Actions workflow that builds and deploys the site to the `gh-pages` branch. Use `workflow_dispatch` for manual builds after config changes on the `hugo` branch.
