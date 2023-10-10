name: Build Markdown to HTML

on:
  push:
    paths:
    - '**.md'

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout repository
      uses: actions/checkout@v2
    
    - name: Convert Markdown to HTML (example using pandoc)
      run: |
        sudo apt-get install pandoc
        for file in $(find . -name "*.md"); do
            pandoc $file -o ${file%.md}.html
        done
    
    - name: Push to repository
      run: |
        git config --local user.email "action@github.com"
        git config --local user.name "GitHub Action"
        git add *.html
        git commit -m "Add generated HTML files"
        git remote set-url origin https://x-access-token:${{ secrets.MY_GITHUB_TOKEN }}@github.com/MufengNiu/JupyerTest.git
        git push
      env:
        GITHUB_TOKEN: ${{ secrets.MY_GITHUB_TOKEN }}
