# release_to_ORDA

![](https://img.shields.io/badge/ORDA--DOI-10.15131%2Fshef.data.17113328-lightgrey)

Copy releases of software to ORDA

```{YAML}
name: Release to ORDA
on:
  workflow_dispatch:
  release:
    types: [published]
jobs:
  upload:
    runs-on: ubuntu-latest
    env:
      ARCHIVE_NAME: ${{ github.event.repository.name }}-${{ github.event.release.tag_name }}
    steps:
      - name: prepare-data-folder
        run : mkdir 'data'
      - name: download-archive
        run: |
          curl -sL "${{ github.event.release.zipball_url }}" > "$ARCHIVE_NAME".zip
          curl -sL "${{ github.event.release.tarball_url }}" > "$ARCHIVE_NAME".tar.gz
      - name: move-archive
        run: |
          mv "$ARCHIVE_NAME".zip data/
          mv "$ARCHIVE_NAME".tar.gz data/
      - name: upload-to-figshare
        uses: figshare/github-upload-action@v1.1
        with:
          FIGSHARE_TOKEN: ${{ secrets.FIGSHARE_TOKEN }}
          FIGSHARE_ENDPOINT: 'https://api.figshare.com/v2'
          FIGSHARE_ARTICLE_ID: <<YOUR ARTICLE ID>>
          DATA_DIR: 'data'
```

## How to generate and store your `FIGSHARE_TOKEN`

1. Follow the [instructions from Figshare](https://help.figshare.com/article/how-to-get-a-personal-token) to generate a personal access token.
2. Follow these [instructions from GitHub](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) to store the value of the token with the name `FIGSHARE_TOKEN` in the repository from which you want to publish releases.

## How to find you article ID

TBD
