name: Generate Snake Animation

on:
  # Run automatically every 24 hours
  schedule:
    - cron: "0 0 * * *" 
  # Allows you to run this job manually from the Actions tab
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - uses: Platane/snk@v3
        with:
          # github user name to read the contribution graph from (**required**)
          github_user_name: ${{ github.repository_owner }}
          
          # These are the files that will be generated
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: Push to Output Branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
