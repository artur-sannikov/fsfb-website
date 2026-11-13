# How to build and preview the website

_Note_: The following instructions have been tested on desktop Linux. They
should work on Windows Subsystem for Linux and macOS.

1. Install [Quarto](https://docs.posit.co/resources/install-quarto.html) and
   [R](https://www.r-project.org/).
   1. [Install](https://git-scm.com/downloads/linux) git.
   2. [Generate](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
      and
      [add](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
      an SSH key to your GitHub account.
   3. Clone this repo to your machine: `git clone
git@github.com:Finnish-Society-for-Bioinformatics/website.git`
   4. Enter the repository `cd website`.
   5. When the website code updates, you only need to run `git pull` from the
      repository to receive new updates.
   6. Run `quarto preview`. The website should open in browser.
   7. If needed, install required packages:
      - Enter R console with `R` command.
      - Run `install.packages(c("knitr", "rmarkdown", "yaml"))`.

# Checks before approving a pull request

Every pull request runs the **render-check** job from
`.github/workflows/publish.yml`. It installs the R dependencies and renders the
whole site with Quarto, so a broken page or R chunk fails the check.

Maintainers should approve and merge a pull request only after the
`Quarto Publish / render-check` check has passed. To enforce this in GitHub,
open **Settings → Branches → Branch protection rules** for `main`, enable
**Require status checks to pass before merging**, and select
**Quarto Publish / render-check**.

To run the same check locally before opening a pull request:

```bash
quarto render
```

# Publishing a branch preview on GitHub Pages

The `Website preview` workflow renders a branch and copies it to a
subdirectory of `gh-pages`, leaving the live site at the repository root
untouched. The preview path is derived from the branch name, with characters
that are not URL-friendly replaced by `-`:

```text
https://bioinf.fi/preview/<branch-name>/
```

For example, `bioconductor-communities` is published at:

```text
https://bioinf.fi/preview/bioconductor-communities/
```

The workflow runs automatically when any branch other than `main` is pushed.
To run it manually or choose a custom preview path, open
**Actions → Website preview → Run workflow**. The workflow must exist on the
default branch before the manual trigger is available.

A later deployment of `main` force-pushes `gh-pages` and removes the preview.
Re-run the preview workflow when you need it again.

# Testing the Bioconductor map issue forms in a fork

GitHub reads issue templates and runs `issues` workflows from the repository's
default branch. In the upstream repository the templates are not on `main`, so
the preview page's Add me link opens an empty form.

To test the whole flow without changing the upstream `main` branch:

1. Fork the repository to your GitHub account.
2. In the fork's **Settings → Features**, enable **Issues**.
3. In **Settings → Actions → General**, allow actions and set **Workflow
   permissions** to **Read and write permissions**. Enable **Allow GitHub
   Actions to create and approve pull requests**.
4. Push `bioconductor-communities` to the fork. The **Website preview** workflow
   publishes the site to the fork's `gh-pages` branch, removes the inherited
   `bioinf.fi` CNAME, and points the Add me, Update, and Remove buttons at the
   fork's issues automatically.
5. In the fork's **Settings → General → Default branch**, set
   `bioconductor-communities` as the default branch.
6. In the fork's **Settings → Pages**, publish from the `gh-pages` branch and
   the `/ (root)` folder. If Pages already has the `bioinf.fi` custom domain,
   remove it there.
7. Run **Actions → Labels → Run workflow** once so the `bioc-map` labels exist.
8. Open the fork's preview page, submit the Add me form, add `approved` as a
   maintainer, and merge the pull request opened by the bot.

When rendering locally in a fork, set `bioc_repo_url` in `_variables.yml` to
`https://github.com/your-name/website` manually.

A fork named `website` publishes to:

```text
https://your-name.github.io/website/preview/bioconductor-communities/
```

After the bot's data pull request is merged, the next preview build shows the
new person on the map.

# How to Add New Events?

Adding/modifying events on the Events pages is a straightforward but a bit
delicate process.

If you need to **modify** an event, just modify the _text_, not the code.

If you need to **add** an event, scroll down until `<!-- Use the second event
from here as template to keep the structure -->`. If you insert this event,
it's going it be the Hackathon even.

Next, scroll until `<!-- Use the first  event from here as template to keep the
structure -->`. If you insert the event under this comment, it's going to be
BioBeer.

Just keep on inserting the "first" event after the "second" event to keep the
structure. The "first" event refers to the event with the image to the left,
and the "second" even refers to the event with the image to the right.
