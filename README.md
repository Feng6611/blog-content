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

## Front matter for search

| Field | Use |
| --- | --- |
| `description` | 120–160 characters, written for the search result. Without it the page falls back to the opening text. |
| `topics` | One or more ids from `site.json` `topics`, primary topic first. Required on Chinese posts; translations inherit them and must not repeat them. |
| `updated` | Optional `YYYY-MM-DD` for a substantial revision (new section, corrected facts). It feeds the sitemap `lastmod`, `dateModified` and IndexNow. Leave it out for typo fixes. |
| `keywords` | Comma-separated terms. Google ignores them; they drive related-article ranking. |

## Topics and internal links

Topic ids in `site.json` are public URLs (`/topics/<id>/`, `/en/topics/<id>/`); add new ones rather than renaming. Each topic page has a short description and lists every article in it; each article shows its topics under the date and ends with up to three related articles (shared topics weigh most, then shared keywords).

Linking strategy:

1. **Hub, then spokes.** Every article links up to its topic through the header tag; the topic page links down to all its articles. Nothing else is needed for an article to be reachable in two clicks.
2. **Link in the prose where an earlier post already answers something.** Use `[[文件名]]` (it follows the reader's language) or a Markdown link. One or two links per article, on a phrase that names what the target is about; not "点这里".
3. **Link backwards when publishing.** A new post that continues an older one links to it; then add one sentence to the older post pointing forward if the new one supersedes or extends it, and set its `updated`.
4. **Series stay together.** Year reviews (`year-review`) and the writing-block posts (`writing`) read as series; mention the previous one in the opening paragraph.
5. **Keep topics few.** Six topics for about 30 articles; a topic with fewer than three articles is better folded into a neighbour.

## Publishing

Pushing to `main` runs [`.github/workflows/content-updated.yml`](.github/workflows/content-updated.yml). It sends the exact commit SHA to `Feng6611/blog-tem`; that repository builds the site and deploys it to Cloudflare Pages.

The cross-repository deployment pipeline was verified on 2026-07-25.

Set `TEMPLATE_DISPATCH_TOKEN` as a repository secret. It must be a fine-grained GitHub token allowed to send repository dispatch events to `Feng6611/blog-tem`.
