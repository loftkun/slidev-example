# slidev-example

An example of using Slidev to generate presentation from Markdown.

Please read my blog post: 

- [Slidevを導入してMarkdownで美しいスライドを書こう - Qiita](https://qiita.com/loftkun/items/2fbeddc9449eb5d85dfd)

## live demo

SPA on [Netlify](https://loftkun-slidev-example.netlify.app/)

## usage

```bash
git clone https://github.com/loftkun/slidev-example.git
cd slidev-example
npm install
npm run dev -- slides.md
```

Visit `http://localhost:3030/` and confirm the slides showed as SPA.

## publish SPA

```bash
# For GitHub Pages, require setting assets path on server with "--base" param.
npm run build -- --base slidev-example/dist/
ls -Fla dist/
```