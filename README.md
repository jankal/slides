# @jankal's slide collection

I build my slides using [marp](https://marp.app/). See the [marpit reference](https://marpit.marp.app/) for syntax guidance.

To develop slides, use
```shell
npm install
npx @marp-team/marp-cli@latest -s ./
```

To export slides, use
```shell
# Convert slide deck into HTML
npx @marp-team/marp-cli@latest slide-deck.md
npx @marp-team/marp-cli@latest slide-deck.md -o output.html

# Convert slide deck into PDF
npx @marp-team/marp-cli@latest slide-deck.md --pdf
npx @marp-team/marp-cli@latest slide-deck.md -o output.pdf

# Convert slide deck into PowerPoint document (PPTX)
npx @marp-team/marp-cli@latest slide-deck.md --pptx
npx @marp-team/marp-cli@latest slide-deck.md -o output.pptx
```