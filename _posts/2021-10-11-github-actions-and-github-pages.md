## GitHub Actions & GitHub Pages

[In 2008](https://github.blog/2008-12-18-github-pages/) GitHub launched [GitHub Pages](https://pages.github.com/), a simple way for users to translate a simple repository into a basic static website hosted by GitHub itself. The concept itself is remarkably simple - you create the bare bones of a site's content, and GitHub will translate it into HTML and CSS using popular Ruby static site generator [Jekyll](https://jekyllrb.com/). The basics of building a Jekyll website have been explained numerous times by coders far more experienced than me; I leaned heavily on [Chad Baldwin](https://github.com/chadbaldwin)'s excellent [simple-blog-bootstrap](https://github.com/chadbaldwin/simple-blog-bootstrap/). GitHub automatically updates your Page with any commit pushed to branch you designate as the site's Source (in the Pages section of the repository's Settings tab), so if you're happy to push changes directly to a branch there's no need to complicate that.

A decade after the launch of Pages, GitHub launched [GitHub Actions](https://github.com/features/actions), a workflow automation tool. Actions isn't intended to supplant traditional cloud computing services; for my purposes, it's a simple and useful way to try out workflow automation. Since my Pages repository is extremely simple and requires rote tests and workflow that doesn't change much, it's an ideal candidate for testing out Actions.

---


---

### Aims & Objectives

My aims for this process are simple enough. I want:

1. To keep my work in development branches until I think it's ready to deploy.
2. To deploy it simply by pushing completed work to a branch.
3. To have automated tests run before that commit appears on my live Page.
4. To receive a warning if those tests aren't passed.
5. Not have to pull down a new commit before resuming work on my local machine.

If that's not asking too much, I'd also like to work in a familiar environment and keep my security exposure to a minimum!

---

TABLE OF CONTENTS

---

### GitHub Actions Workflows



#### Actions Marketplace

#### Environment Variables

#### Authentication

---

### Setting Up the Environment

#### Installing Ruby

#### Installing Gems

---

### Running the Test Suite

#### Failing Tests

---

### Deploying to Pages

#### Appropriate Commit Messaging

#### Commit Strategies

---

### Conclusions
