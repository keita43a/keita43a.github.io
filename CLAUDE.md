# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Site overview

This is a static personal academic website for Keita Abe (Professor, Musashi University), hosted at keita43a.github.io via GitHub Pages. There is no build step beyond Jekyll's theme layer — content is plain HTML/CSS.

## Architecture

The site is **not a Jekyll content site** despite having `_config.yml`. The config only sets `theme: jekyll-theme-slate`; all actual pages are hand-written HTML files, not Markdown or Liquid templates. GitHub Pages renders them as-is.

**Bilingual structure:** English and Japanese are parallel but independent:
- `/index.html` + `/style.css` — English site
- `/jp/index.html` + `/jp/style.css` — Japanese site (阿部景太)

Both pages share the same Google Analytics tag (`G-F4SV7F8871`) and jQuery CDN. When updating content (bio, research, teaching), edits typically need to be made in **both** language versions.

**Static assets:**
- `/images/` — logos, profile photos, SVG icons
- `/files/` — PDF syllabi and presentation slides (Japanese filenames are fine)

**Other pages:**
- `keita_cv.html` — standalone CV page (large file, ~4.5MB)
- `gyoku_mid.html` — standalone page (玉川大学 mid-term related)

## Deployment

Push to `master` → GitHub Pages auto-deploys. No CI, no build commands needed.

## Previewing locally

```bash
bundle exec jekyll serve
```

(Requires Ruby/Bundler with `github-pages` gem. If not set up, opening `index.html` directly in a browser works for most edits since there are no Liquid templates.)

## デバッグの方針

CSS/レイアウトの問題が発生した場合、コードを変更する前に以下を確認する：

1. **HTML構造を先に疑う**: CSSの症状（背景が表示されない、z-indexが効かないなど）は
   多くの場合、要素の配置やネスト構造が原因。まず DOM ツリーを確認する。

2. `position: fixed` の要素は `overflow: hidden/scroll` を持つ祖先要素の
   内側に置かない。モーダルは `</body>` 直前（body直下）に配置すること。

3. **変更は1つずつ、根拠を持って**: 「試してみる」式の変更を重ねない。
   1つの変更には明確な仮説が必要。変更が効かなければ原因を再考する。

4. **要素を移動したときは継承CSSを確認**: 親要素から継承していた
   スタイル（text-align など）が失われる可能性がある。
