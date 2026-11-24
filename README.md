name: Generate Snake

on:
  schedule:
    - cron: "0 */12 * * *"
  workflow_dispatch:
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - name: Generate snake SVGs
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/snake.svg?color_snake=#FF3030&color_dots=#1a1a1a,#4d0000,#8B0000,#cc1f1f,#FF3030
            dist/snake-dark.svg?palette=github-dark&color_snake=#FF3030&color_dots=#161b22,#4d0000,#8B0000,#cc1f1f,#FF3030

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
