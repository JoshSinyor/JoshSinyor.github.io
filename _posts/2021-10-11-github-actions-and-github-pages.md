# GitHub Actions and GitHub Pages

[In 2008](https://github.blog/2008-12-18-github-pages/) GitHub launched [GitHub Pages](https://pages.github.com/), a simple way for users to translate a simple repository into a basic static website hosted by GitHub itself. The concept itself is remarkably simple - you create the bare bones of a site's content, and GitHub will translate it into HTML and CSS using popular Ruby static site generator [Jekyll](https://jekyllrb.com/). The basics of building a Jekyll website have been explained numerous times by coders far more experienced than me; I leaned heavily on [Chad Baldwin](https://github.com/chadbaldwin)'s excellent [simple-blog-bootstrap](https://github.com/chadbaldwin/simple-blog-bootstrap/). GitHub automatically updates your Page with any commit pushed to branch you designate as the site's Source (in the Pages section of the repository's Settings tab), so if you're happy to push changes directly to a branch there's no need to complicate that.

A decade after the launch of Pages, GitHub launched [GitHub Actions](https://github.com/features/actions), a workflow automation tool. Actions isn't intended to supplant traditional cloud computing services; for my purposes, it's a simple and useful way to try out workflow automation. Since my Pages repository is extremely simple and requires rote tests and workflow that doesn't change much, it's an ideal candidate for testing out Actions.

---

TABLE OF CONTENTS

---

## Aims & Objectives

My aims for this Action are simple enough. I want:

1. To keep my work in development branches until I think it's ready to deploy.
2. To deploy it simply by pushing completed work to the default branch.
3. To have automated tests run before that commit appears on my live Page.
4. To receive a warning if those tests aren't passed.
5. Not to have to pull down a new commit before resuming work on my local machine.

If that's not asking too much, I'd also like to work in a familiar environment and keep my security exposure to a minimum!

---

## GitHub Actions Workflows

GitHub Actions workflows are designed to be simple and easy to use. There's a pretty obvious flow to the work I want to do; first, I have to set up my environment, then run my tests, then deploy (or not, if the tests don't pass) my code to the branch Pages publishes. Here's the first steps of setting up the workflow - creating the environment. I started by creating [a file in the repository](https://github.com/JoshSinyor/JoshSinyor.github.io/blob/main/.github/workflows/deployment_ci.yml) in a following folder structure: `ROOT/.github/workflows/deployment_ci.yml`. This folder structure is mandatory.

⚠️ **Warning: GitHub Actions won't trigger on workflow files outside the default branch.** If you're using a different branch for testing changes to the Actions file(s), you'll need to change the default branch to that test branch.

Here's the beginning of my workflow file:

```
name: Test & Deploy

on:
  push:
    branches:
      - main

jobs:
  test_deploy:
    runs-on: macos-latest
```

1. First, the workflow is named (`Test & Deploy`).
2. Second, I define the condition (`on:`) that triggers the workflow - here, a push to the `main` branch. When I want to deploy content to my GitHub Pages, I'll push a commit to my `main` branch, which will trigger this workflow.
3. Third, I define the container that GitHub will use to run the workflow. I'm used to working with OSX, so I chose `macos-latest`.

---

## Environment Variables

Next I'll need to set and get some variables for later reference. A [fairly extensive list](https://docs.github.com/en/actions/learn-github-actions/environment-variables) of environmental variables are made available to users of GitHub Actions. In particular I'll want to set here (once, so I don't have to change each reference to them in the Action if I decide to use different branches or tokens later) the branches I'm pulling from (`main`, designated as `EMPLOY_BRANCH`) and the branch I'm pushing to (`gh-pages`, designated as `DEPLOY_BRANCH`). I'll also need to create an access token (designated as `DEPLOY_TOKEN`) to authorise the Ruby container to push to my repository; more information on this is in the next section. GitHub Action environment variables need to be defined under the `env` keyword:

GitHub Actions environment variables are accessible at the workflow, job and step levels. I need these variables to be accessible in several different steps, so they need to be defined at the job level (`jobs.test_deploy.env`).

```
  env:
    EMPLOY_BRANCH: main
    DEPLOY_BRANCH: gh-pages
    DEPLOY_TOKEN: ${{ secrets.ACTIONS_PAT }}
```

### Authentication

Each run of my Action workflow requires setting up a container which I will use to push commits to my repository. For GitHub to accept pushes, that container will need to authenticate itself. There are several ways to do this, but the simplest is to use a [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) ('PAT') with a very limited scope. There are a few things to consider when allowing Actions to push to a repository:

⚠️ **This choice has security implications.** This Action will be running automatically and without direct oversight. It's important that the access granted is as limited in scope as possible; I don't want a bad actor exploiting it to gain access to my GitHub account. This is the number one reason I chose to write this aspect of my Action myself, rather than using any of the numerous off-the-shelf Action components available on the [Marketplace](https://github.com/marketplace?type=actions): I want complete oversight of the code I allow authenticated access to my repository.

⚠️ **Personal Access Tokens can only authenticate via HTTPS, not SSH.** My Action's Ruby instance is therefore configured to push via HTTPS, not SSH. This doesn't affect any other instances of my repository - my local machines can continue to authenticate pushes and pulls to and from this repository using SSH.

To create a new PAT, navigate to the [Personal access tokens subsection](https://github.com/settings/tokens) of the Developer settings section of your profile's Settings tab. Then, select Generate new token.

![Generating a New Personal Access Token](/images/2021-10-11/personal_access_token_01.png)

Make sure to name the token something that accurately describes what you'll use it for, and to set an expiration date. You can easily update a token's hash later, so resist the urge to create a token with no expiry date - it's a security risk. Give the token `repo` scope but no other - it doesn't need any others, and if it's trying to use others, it shouldn't be allowed to do so.

Next, click Generate token. You'll be presented with your new token's hash; this is the only time you'll be able to access it, so copy it to clipboard.

![A New Personal Access Token](/images/2021-10-11/personal_access_token_02.png)

⚠️ **Never reveal the hash of any security token!** It will allow anyone with access to the hash access to whatever your token's scope is. This token was revoked immediately after I took these illustrative screenshots. **Never save or share a token's hash anywhere!**

⚠️ **Do not share PATs between repositories!** If a single PAT is somehow compromised, you can delete it and update the single dependent repository. Using a repository-specific PAT allows you to limit the PAT's scope to the absolute minimum required by that individual repository. We will actually end up restricting the scope of the PAT even further, to just the GitHub Pages environment within this individual repository.

Having copied the token's hash, navigate to the Secrets subsection of your repository's Settings tab, and select New repository secret.

![Adding a Secret](/images/2021-10-11/personal_access_token_03.png)

In the New secret page, name the token how you'll want to refer to it in the workflow (matching the `DEPLOY_TOKEN` environmental variable), paste the PAT's hash in the Value box, and select Add secret.

![New Secret](/images/2021-10-11/personal_access_token_04.png)

You'll see the token listed in the repository's Secrets.

![Repository Secret](/images/2021-10-11/personal_access_token_04.png)

Finally, as above I'll need to add the token to the environmental variables already listed in the workflow.

⚠️ **We will return to this PAT later on to further restrict their scope.**

---

## Setting Up the Environment

My workflow will run in a Ruby environment inside the OSX container, so I need to install Ruby and a way for with workflow to interface with it within OSX.

### Installing Ruby

Installed is `checkout@v2` - an interface between GitHub and the container - and Ruby. I specify the version of Ruby to match the version of Ruby contemporaneously [used by GitHub pages](https://pages.github.com/versions/). Here are the first steps of the job:

```
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Setup Environment
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 2.7.3
```

### Installing Gems

Having installed Ruby, it's time to install the gems required for my testing (which will rely on Rubocop and RSpec) to work. `setup-ruby@v1` automatically installs the Bundler gem, so the next step can simply command the installation of the repository's gems. This repository's Gemfile includes Jekyll strictly for local development - it's not required for testing, so it can be specifically excluded from the Action's installation. Here's the next step of the job:

```
      - name: Install Gems
        run: |
          bundle config set --local without 'jekyll jekyll_plugins'
          bundle install
```

---

## Running the Test Suite

Running the test suite is as simple as running the commands as you would in any local machine's terminal. Here's the next step of the job:

```
      - name: Run Tests
        run: |
          rubocop
          rspec
```

---

## Deploying to Pages

Having set up our environment and tested the commit, we can now deploy it - by pushing it to the `DEPLOY_BRANCH`.

### Commit Strategies

  One of the most annoying situations to find yourself in is pushing a commit which triggers an action which makes a further commit on the branch you're working on, but failing to pull that commit down before continuing to work on the branch and having to merge your work to with the automated commit first. My Action is designed to deploy the commit I've pushed to the `main` branch and shouldn't make any changes to that branch, so I don't want the Action to modify the commit history of the `EMPLOY_BRANCH` branch, just the `DEPLOY_BRANCH`.

I also want to restrict pushes to the `DEPLOY_BRANCH` to this Action - I don't want other commits being pushed without going through testing, nor being inadvertently overwritten by later Action pushes, nor conflicts when Actions tries to push a commit to `DEPLOY_BRANCH`.

Further, I want Action's pushes to be the last word: I manage the `EMPLOY_BRANCH`, and I expect the `DEPLOY_BRANCH` to mirror it exactly. The `DEPLOY_BRANCH`'s commit history is irrelevant: I want it overwritten, not merged. Therefore I use the nuclear option: `reset --hard`.

### Appropriate Commit Messaging

Although the `DEPLOY_BRANCH` won't have much of a commit history - since it will be reset each time a commit is pushed to it - I want the commit to reflect the fact that the commit was pushed by an Action, and that it reflects a specific commit from a specific branch. Since I'll be pushing a commit to the `DEPLOY_BRANCH` branch, I'll edit the last commit on the `EMPLOY_BRANCH`, and go from there.

Here's the next step of the job:

```
      - name: Edit Commit
        run: git commit --amend --author="GitHub Action <github-actions@github.com>" -m "Automated post-CI deployment commit. Reflects $(git rev-parse --abbrev-ref ${{ github.ref }}) branch commit $(git rev-parse --short ${{ github.sha }})."
```

This command edits the author and message of the commit. Now it's suitable for a push to the `DEPLOY_BRANCH`.

### Resetting the `DEPLOY_BRANCH`

Within the Actions container, only the default branch is loaded, and that only within the locality. Therefore it's necessary to set up a remote connection to the hosted repository, and fetch the `DEPLOY_BRANCH`. This is where the PAT is required; it authenticates the HTTPS connection to the remote repository.

```
- name: Fetch Deploy Branch
  run: |
    git remote set-url origin "https://x-access-token:${{ env.DEPLOY_TOKEN }}@github.com/${{ github.repository }}"
    git fetch origin ${{ env.DEPLOY_BRANCH }}
```

Having fetched the `DEPLOY_BRANCH`, it can be overwritten with the tested `EMPLOY_BRANCH`. However, the loading of the Gems in the Actions' configuration (without the Jekyll-related gems) will often generate changes in the `EMPLOY_BRANCH`'s Gemfile; it's necessary to discard these before switching to the `EMPLOY_BRANCH`.

```
- name: Reset Deploy Branch
  run: |
    git restore Gemfile.lock
    git checkout ${{ env.DEPLOY_BRANCH }}
    git reset --hard ${{  env.EMPLOY_BRANCH }}
```

### Pushing to the `DEPLOY_BRANCH`

Finally, the reset `DEPLOY_BRANCH` can be pushed to the remote repository. As in the `reset --hard` situation, this deployment needs to be pushed forcefully to prevent merge conflicts. However, there is one situation where overwriting the existing commit is undesirable: where another commit has been pushed in the interim. The `-with-lease` flag will prevent an accidental overwrite of a newer commit with an older one, should such a situation arise.

```
- name: Deploy
  run: git push --force-with-lease origin ${{ env.DEPLOY_BRANCH }}
```

---

## Conclusions

GitHub Actions are a useful tool for a variety of things - deploying to GitHub Pages is just a very simple example of their utility. If you'd like to get started with a simple task, I hope this proved useful!

Here's the contents of my complete GitHub Action.

```
name: Test & Deploy

on:
  push:
    branches:
      - main

jobs:
  test_deploy:
    runs-on: macos-latest

    env:
      EMPLOY_BRANCH: main
      DEPLOY_BRANCH: gh-pages
      DEPLOY_TOKEN: ${{ secrets.ACTIONS_PAT }}

    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Setup Environment
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 2.7.3

      - name: Install Gems
        run: |
          bundle config set --local without 'jekyll jekyll_plugins'
          bundle install

      - name: Run Tests
        run: |
          rubocop
          rspec

      - name: Edit Commit
        run: git commit --amend --author="GitHub Action <github-actions@github.com>" -m "Automated post-CI deployment commit. Reflects $(git rev-parse --abbrev-ref ${{ github.ref }}) branch commit $(git rev-parse --short ${{ github.sha }})."

      - name: Fetch Deploy Branch
        run: |
          git remote set-url origin "https://x-access-token:${{ env.DEPLOY_TOKEN }}@github.com/${{ github.repository }}"
          git fetch origin ${{ env.DEPLOY_BRANCH }}

      - name: Reset Deploy Branch
        run: |
          git restore Gemfile.lock
          git checkout ${{ env.DEPLOY_BRANCH }}
          git reset --hard ${{  env.EMPLOY_BRANCH }}

      - name: Deploy
        run: git push --force-with-lease origin ${{ env.DEPLOY_BRANCH }}
```
