# cltk.github.io
Static website for the CLTK organization, built with Jekyll and deployed via GitHub Pages Actions.

## Local development

Prereqs: Ruby and Bundler.

1) Install deps

```
bundle install
```

2) Serve locally

```
bundle exec jekyll serve
```

Site builds to `_site` and serves at `http://127.0.0.1:4000`.

## Deployment

Pushes to the `master` branch trigger the GitHub Actions Pages workflow, which builds with the `github-pages` gem and deploys to GitHub Pages.

Custom domain is configured via `CNAME` for `https://cltk.org`.

## CI: Link checking

Pull requests run an internal link check using `html-proofer` against the built `_site`.
