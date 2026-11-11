name: Profile automation

on:
  schedule:
    - cron: "0 */6 * * *"
  workflow_dispatch:
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  snake:
    runs-on: ubuntu-latest
    steps:
      - name: Generate contribution snake
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: Push snake to output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  activity:
    runs-on: ubuntu-latest
    steps:
      - name: Update recent activity in README
        uses: jamesgeorge007/github-activity-readme@master
        with:
          MAX_LINES: 8
          COMMIT_MSG: "chore: update recent activity"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
