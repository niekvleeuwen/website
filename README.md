# Website

Personal website build with [Hugo](https://gohugo.io/) and [Congo](https://jpanther.github.io/congo/).

## Usage

### Requirements
Install the following requirements:

- Hugo  ([link](https://gohugo.io/installation/))
- Go ([link](https://golang.org/dl/))

Start Hugo’s development server to view the site.

### Previewing the website

```
hugo server
```

### Add content 

Add a new page to the website.

```
hugo new content posts/my-first-post/index.md
```

Make sure to include draft content while previewing new articles.

```
hugo server --buildDrafts
```

### Publishing

With the following command, Hugo generates the static files in the `public` directory.

```
hugo
```