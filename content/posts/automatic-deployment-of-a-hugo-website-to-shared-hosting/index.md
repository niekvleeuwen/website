---
title: "Continuous deployment of a Hugo website to shared hosting with Github Actions"
date: 2022-10-02T11:02:38.458914+02:00
description: "Automatically deploying a Hugo website to a shared hosting provider using Github Actions."
categories: ["CI/CD"]
tags: ["Hugo", "Github Actions"]
aliases: [
  "/blog/ci-cd/automatic-deployment-of-a-hugo-website-to-shared-hosting",
  "/2020/09/continuous-deployment-of-a-hugo-website-to-shared-hosting"
]
---

After I converted my personal website to the static site generator [Hugo](https://gohugo.io/), I ran into a problem. Every time I wanted to make an adjustment to my site I had to go through the following steps:

1. Make the change
2. Commit and push to Github
3. Build the site with Hugo
4. Upload the output of the `public` folder over FTP to my webhost.

To speed up this process I started using Github Actions. As soon as I push the changes to the `master` branch the site needs to be built and uploaded. 

### Creating a Github Action
The first step is to create a Github Action. To achieve this, you need to create a file in the .github/workflows folder. The name of this file is not important, but the metadata filename must be either `action.yml` or `action.yaml`. I use `build-and-deploy.yml`. 

### Building and deploying

#### 1. Checkout code
First we checkout our repository so that our workflow can access it.

```yaml
on: push
name: 🚀 Deploy website on push
jobs:
  web-deploy:
    name: Deploy
    runs-on: ubuntu-latest
    steps:
    - name: Get latest code
      uses: actions/checkout@v2

```

#### 2. Build with Hugo
The next step is to build our Hugo site. We achieve this using [actions-hugo](https://github.com/peaceiris/actions-hugo). I use the `--minify` flag, but it can be replaced by `-D` for example if you want to include drafts or just `hugo` is also sufficient.

```yaml
- name: Setup Hugo
  uses: peaceiris/actions-hugo@v2
  with:
    hugo-version: 'latest'

- name: Build
  run: hugo --minify
```

#### 3. Deploy with FTP

Finally, the content of the `public` must be uploaded via FTP to the webhost. It is nessesary to add the username (FTP_USERNAME) and password (FTP_PASSWORD) as [secrets](https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets) on your Github repository.

```yaml
- name: Sync files to server
  uses: SamKirkland/FTP-Deploy-Action@4.3.2
  with:
    server: ftp.niekvanleeuwen.nl
    username: ${{ secrets.FTP_USERNAME }}
    password: ${{ secrets.FTP_PASSWORD }}
    local-dir: public/
```

If you want to exclude a file or folder, this is also possible by adding the following entry

```yaml
exclude: |
  **/.git*
  **/.git*/**
  **/node_modules/**
  fileToExclude.txt
```

### Result
If we merge the above code snippets we reach the following result
```yaml
on: push
name: 🚀 Deploy website on push
jobs:
  web-deploy:
    name: Deploy
    runs-on: ubuntu-latest
    steps:
    - name: Get latest code
      uses: actions/checkout@v2

    - name: Setup Hugo
      uses: peaceiris/actions-hugo@v2
      with:
        hugo-version: 'latest'

    - name: Build
      run: hugo --minify
    
    - name: Sync files to server
      uses: SamKirkland/FTP-Deploy-Action@4.3.2
      with:
        server: ftp.niekvanleeuwen.nl
        username: ${{ secrets.FTP_USERNAME }}
        password: ${{ secrets.FTP_PASSWORD }}
        local-dir: public/
```

Thanks for reading! If you have found a mistake, want to ask a question or have a comment, please send me an [email](mailto:ik@niekvanleeuwen.nl).