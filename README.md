# Automatic Deployment on React App: Github Actions

When commit or merge to the master branch. Automatically deploy to gh-pages branch.



[Live](https://ferdiozer.github.io/github-actions-react/)


- .github/workflows/deploy.yml 
```
name: Build & deploy

on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - master

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v2
    
    - name: Install Node.js
      uses: actions/setup-node@v1
      with:
        node-version: 13.x
    
    - name: Install NPM packages
      run: npm ci
    
    - name: Build project
      run: npm run build
    
    - name: Run tests
      run: npm run test

    - name: Upload production-ready build files
      uses: actions/upload-artifact@v2
      with:
        name: production-files
        path: ./build
  
  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/master'
    
    steps:
    - name: Download artifact
      uses: actions/download-artifact@v2
      with:
        name: production-files
        path: ./build

    - name: Deploy to gh-pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./build
```

This project was bootstrapped with reactjs. [React Readme Docs](https://github.com/ferdiozer/github-actions-react/blob/master/README_react.md).

