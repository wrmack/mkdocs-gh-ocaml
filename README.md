# README

Enter the virtual environment:

```bash
source venv/bin/activate
```

To start the local server:

```bash
zensical serve -a <ip address>:<port>
```

To build the site:

```bash
zensical build
```

To deploy to Github pages just commit and push to main:
- we now use Github actions with .github/workflows/docs.yml
- in Github settings:
  - Pages: under 'Build and deployment' set to Github actions
  - Environment: gh-pages and main are allowed

[Github doc](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow)


[Zensical doc](https://zensical.org/docs/get-started/)