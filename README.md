# jupyter-to-pages

This action transforms the latest run of each Jupyter notebook in a repository into markdown files,
builds a static site with them and push it at the `gh-pages` branch. We can then use that
branch to trigger publishing into the repository Github pages.

## Parameters

| Name           | Description                                                                        |
| -------------- | ---------------------------------------------------------------------------------- |
| `show-cells`   | Whether or not to show cells contents. Omit to hide code in Jupyter notebook cells |
| `dependencies` | Python dependencies required to run the notebooks. Usually: `requirements.txt`     |
| `source`       | Path to the Jupyter notebook's folder                                              |

## How to use this action

Say we want to publish all notebooks under the `src/notebooks` folder, showing the cells code, making use
of the python packages specified at `deps.txt` The workflow would look like this:

```
on:
  workflow_dispatch:
permissions:
  contents: write
jobs:
  build-documentation:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: Publish Jupyter Notebooks as Github pages
        uses: dada-public/jupyter-to-pages@v1
        with:
          show-cells: true
          source: src/notebooks
          dependencies: deps.txt
```

This will create a static website with the contents of each Jupyter notebook under `src/notebook`. Where the navigation
of this site will mirror our internal folder structure. And it will publish it at the `gh-pages` branch.

To actually publish to repository's pages, we need to configure it to use that branch as a source for publications.

Go to `Settings > Pages` and instruct GIT to publish the contents of `gh-pages` on each push:

![Configure Git Pages](./settings.png)

There is a full example in this [repository](https://github.com/dada-public/jupyter-pages-demo).

## Requirements

- The conversion from Jupyter notebooks format to Markdown is done using `nbconvert`. This tool is usually packaged within
  your Jupyter notebooks distribution, and matched the Jupyter format version. I.e., you need to record your Jupyter's installation
  in the dependencies file for this action to work, or provide a fallback.

- The action will build a site from your Jupyter notebooks on top of [Mkdocs](https://www.mkdocs.org/) and the [Mkdocs Material](https://squidfunk.github.io/mkdocs-material/) theme. Thus, you
  need to configure them by providing a `mkdocs.yml` file.

## Sponsor me

<iframe src="https://github.com/sponsors/victor-public/card" title="Sponsor victor-public" height="225" width="600" style="border: 0;"></iframe>
