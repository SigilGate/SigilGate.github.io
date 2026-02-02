# SigilGate.github.io

Source repository for the [SigilGate GitHub Pages](https://sigilgate.github.io/) site, built with [Hugo](https://gohugo.io/) and the [Congo](https://github.com/jpanther/congo) theme.

## Branch structure

| Branch | Purpose |
|--------|---------|
| `main` | Content (`content/`), workflow, README, LICENSE |
| `hugo` | Hugo config, theme submodule, static assets |
| `gh-pages` | Built site (deployed automatically) |

## License

See [LICENSE](LICENSE) for details.

## Deployment

Pushing changes to `content/**` on `main` triggers the GitHub Actions workflow that builds and deploys the site to the `gh-pages` branch. Use `workflow_dispatch` for manual builds after config changes on the `hugo` branch.
