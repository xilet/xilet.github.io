# Hugo FixIt Blog Template (Git)

👉 English | [简体中文](README.md)

This is a quick start template for Hugo theme [FixIt](https://github.com/hugo-fixit/FixIt). It uses [Git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules) feature to load the theme.

It comes with a basic theme structure and configuration. GitHub action has been set up to deploy the blog to a public GitHub page automatically. Also, there's a cron job to update the theme automatically everyday.

## Directory structure

```bash
▸ .github/       # GitHub configuration
▸ archetypes/    # page archetypes (like scaffolds of archetypes)
▸ assets/        # css, js, third-party libraries etc.
▸ config/        # configuration files
▸ content/       # markdown files for hugo project
▸ data/          # blog data (allow: yaml, json, toml), e.g. friends.yml
▸ public/        # build directory
▸ static/        # static files, e.g. favicon.ico
▸ themes/        # theme submodules
```

## Quick Start

For a complete quick start, see this [page](https://fixit.lruihao.cn/documentation/getting-started/).

### Prerequisites

[Hugo](https://gohugo.io/installation/): >= 0.109.0 (extended version)

### Use this Template

1. Click **Use this template**, and create your repository on GitHub.

    <img width="913" alt="image" src="https://github.com/hugo-fixit/hugo-fixit-blog-git/assets/33419593/d5fbd940-3ffd-4750-b1e6-4e87b50b0696">

2. Once the repository is created, just clone and enjoy it!

    ```bash
    # Clone with your own repository url
    git clone --recursive https://github.com/<your_name>/<your_blog_repo>.git
    ```

### Launching the Site

```bash
# Development environment
hugo server
# Production environment
hugo server -e production
```

### Build the Site

When your site is ready to deploy, run the following command:

```bash
hugo
```

### Update theme

Afterwards you can upgrade the theme with the following command:

```bash
# Update theme manually
git submodule update --remote --merge themes/FixIt
```

<details>
  <summary>Start via NPM script</summary>

  ```bash
  npm install
  # build the blog
  npm run build
  # run a local debugging server with watch
  npm run server
  # run a local debugging server in production environment
  npm run server:production
  # update theme submodules
  npm run update:theme
  ```

</details>

## Troubleshooting

<details>
  <summary>remote: Permission to git denied to github-actions[bot].</summary>
  Head to Setting => Actions => General => Workflow permissions => Check "Read and write permissions".
</details>

<!-- This project was generated with [hugo-fixit-blog-git](https://github.com/hugo-fixit/hugo-fixit-blog-git). Documentation about the original structure can be found [here](https://github.com/hugo-fixit/hugo-fixit-blog-git#directory-structure). -->
