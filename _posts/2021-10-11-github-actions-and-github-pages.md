## GitHub Actions & GitHub Pages

[In 2008](https://github.blog/2008-12-18-github-pages/) GitHub launched [GitHub Pages](https://pages.github.com/), a simple way for users to translate a simple repository into a basic static website hosted by GitHub itself. The concept itself is remarkably simple - you create the bare bones of a site's content, and GitHub will translate it into HTML and CSS using popular Ruby static site generator [Jekyll](https://jekyllrb.com/). The basics of building a Jekyll website have been explained numerous times by coders far more experienced than me; I leaned heavily on [Chad Baldwin](https://github.com/chadbaldwin)'s excellent [simple-blog-bootstrap](https://github.com/chadbaldwin/simple-blog-bootstrap/). GitHub automatically updates your Page with any commit pushed to branch you designate as the site's Source (in the Pages section of the repository's Settings tab), so if you're happy to push changes directly to a branch there's no need to complicate that.

A decade after the launch of Pages, GitHub launched [GitHub Actions](https://github.com/features/actions), a workflow automation tool. Actions isn't intended to supplant traditional cloud computing services; for my purposes, it's a simple and useful way to try out workflow automation. Since my Pages repository is extremely simple and requires rote tests and workflow that doesn't change much, it's an ideal candidate for testing out Actions.

---

TABLE OF CONTENTS

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

### GitHub Actions Workflows

GitHub Actions workflows are designed to be simple and easy to use. There's a pretty obvious flow to the work I want to do; first, I have to set up my environment, then run my tests, then deploy (or not, if the tests don't pass) my code to the branch Pages publishes. Here's the first steps of setting up the workflow - creating the environment. I started by creating [a file in the repository](https://github.com/JoshSinyor/JoshSinyor.github.io/blob/main/.github/workflows/deployment_ci.yml) in a following folder structure: `ROOT/.github/workflows/deployment_ci.yml`. This folder structure is mandatory.

```
name: Test & Deploy

on:
  push:
    branches:
      - main

jobs:
  test_deploy:
    runs-on: macos-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Setup Environment
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 2.7.3
```

⚠️ **Warning: GitHub Actions won't trigger on workflow files outside the default branch.** If you're using a different branch for testing changes to the Actions file(s), you'll need to change the default branch to that test branch.

1. First, the workflow is named (`Test & Deploy`).
2. Second, I define the condition (`on:`) that triggers the workflow - here, a push to the `main` branch. When I want to deploy content to my GitHub Pages, I'll push a commit to my `main` branch, which will trigger this workflow.
3. Third, I define the container that GitHub will use to run the workflow. I'm used to working with OSX, so I chose `macos-latest`.
4. Fourth, I set up my environment. Installed are `checkout@v2` - an interface between GitHub and the container - and Ruby. I specify the version of Ruby to match the version of Ruby contemporaneously [used by GitHub pages](https://pages.github.com/versions/).

#### Environment Variables

Next I'll need to set and get some variables for later reference. A [fairly extensive list](https://docs.github.com/en/actions/learn-github-actions/environment-variables) of environmental variables are made available to users of GitHub Actions. In particular I'll want to set here (once, so I don't have to change each reference to them in the Action if I decide to use different branches or tokens later) the branches I'm pulling from (`main`, defined as `EMPLOY_BRANCH`) and the branch I'm pushing to (`gh-pages`, defined as `DEPLOY_BRANCH`). I'll also need to create an access token (defined as `DEPLOY_TOKEN`) to authorise the Ruby container to push to my repository; more information on this in the next section.

GitHub Actions environment variables are accessible at the workflow, job and step levels. I need these variables to be accessible through different steps, so they need to be defined at the job level (`jobs.test_deploy.env`).

```
  env:
    EMPLOY_BRANCH: main
    DEPLOY_BRANCH: gh-pages
```

GitHub Action environment variables need to be defined under the `env` keyword.

#### Authentication

Each run of my Action workflow requires setting up a container which I will use to push commits to my repository. For GitHub to accept pushes, that container will need to authenticate itself. There are several ways to do this, but the simplest is to use a [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) ('PAT') with a very limited scope. I could have also chosen an even more specialised authentication, like a [Deploy Key](https://docs.github.com/en/developers/overview/managing-deploy-keys). There are a few things to consider when allowing Actions to push to a repository:

⚠️ **This choice has security implications.** This Action will be running automatically and without direct oversight. It's important that the access granted is as limited in scope as possible; I don't want a bad actor exploiting it to gain access to my GitHub account. This is the number one reason I chose to write this aspect of my Action myself, rather than using any of the numerous off-the-shelf Action components available on the [Marketplace](https://github.com/marketplace?type=actions): I want complete oversight of the code I allow authenticated access to my repository.

⚠️ **Personal Access Tokens can only authenticate via HTTPS, not SSH.** My Action's Ruby instance is therefore configured to push via HTTPS, not SSH. However, this doesn't affect any other local instances of my repository - my local machines can continue to authenticate pushes and pulls to and from this repository using SSH. Deploy Keys can use SSH or OAuth tokens if HTTPS isn't appropriate.

For this situation, a limited-scope PAT was ideal. To create it, I:

```
  env:
    DEPLOY_TOKEN: ${{ secrets.ACTIONS_PAT }}
```

---

### Setting Up the Environment

#### Installing Ruby

#### Installing Gems

---

### Running the Test Suite

#### Failing Tests

---

### Deploying to Pages

#### Actions Marketplace

#### Appropriate Commit Messaging

#### Commit Strategies

---

### Conclusions
