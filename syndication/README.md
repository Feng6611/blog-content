# syndication

Translations kept for cross-posting (Qiita / Zenn / note), not for kkuk.dev.

blog-tem only reads `home/**`, `daily/**` and root `about*.md` (see `src/lib/content-index.mjs` `contentFiles` and `src/content.config.ts`), so nothing in this folder is built, listed, or added to RSS/sitemap. Exclusion comes from the directory; `draft: true` in frontmatter is only a marker (the template has no draft field).

- `ja/<slug>.ja.md`: Japanese, translated with Gemini from the Chinese post in `translationOf`. `canonical` / `originalUrl` point to the Chinese post.
- Images and wiki links use Obsidian `![[...]]` syntax and must be converted to absolute image URLs before posting elsewhere.
