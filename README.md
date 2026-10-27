
name: Generate 3D Contributions

on:
  workflow_dispatch:
  schedule:
    - cron: "0 18 * * *"

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - name: Generate contribution graph
        uses: yoshi389111/github-profile-3d-contrib@latest
        env:
          USERNAME: ${{ github.repository_owner }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Commit generated images
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add -A
          git diff --cached --quiet || git commit -m "Update 3D contributions"
          git push
