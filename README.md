# kkuk.dev content

This repository is the content source for the kkuk.dev blog template. It holds posts, daily notes, images, localization dictionaries, and the site configuration; it contains no presentation code.

## Layout

```text
about.md
daily/
home/
image/
locales/
site.json
```

`slug` values are public URLs and must not change after publishing. Use the Markdown filename as the Chinese display title. The optional `title` frontmatter can contain `中文 | English`; the template presents one title per locale.

## Publishing

Pushing to `main` runs [`.github/workflows/content-updated.yml`](.github/workflows/content-updated.yml). It sends the exact commit SHA to `Feng6611/blog-tem`; that repository builds the site and deploys it to Cloudflare Pages.

Set `TEMPLATE_DISPATCH_TOKEN` as a repository secret. It must be a fine-grained GitHub token allowed to send repository dispatch events to `Feng6611/blog-tem`.
