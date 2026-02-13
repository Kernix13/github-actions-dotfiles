# Notes on Actions

<div id="back-to-top"></div>

## Table of Contents

1. [YAML Files](#yaml-files)
1. [GitHub Actions](#github-actions)
   1. [Video 1](#video-1)
   1. [Video 2](#video-2)
   1. [Video 3](#video-3)
   1. [Video 4](#video-4)
   1. [Video 5](#video-5)
   1. [Video 6](#video-6)
   1. [Super-Linter](#super-linter)
1. [GitHub Actions Categories](#github-actions-categories)
   1. [Without Verified creator filter](#without-verified-creator-filter)
1. [Work flow rules](#work-flow-rules)
1. [Example YAML Code](#example-yaml-code)
1. [GitHub Actions Crash Course](#github-actions-crash-course)
   1. [YML Code Blocks](#yml-code-blocks)

## YAML Files

YAML is a digestible data serialization language often used to create configuration files with any programming language.

Designed for human interaction, YAML is a strict superset of JSON, another data serialization language. But because it’s a strict superset, it can do everything that JSON can and more

The basic structure of a YAML file is a map. You might call this a dictionary, hash or object, depending on your programming language or mood.

- YAML: yet another markup language
- YAML uses space to define something. If we use a single space or double space, it has different meanings in YAML. Spaces change the meaning of data structure.
- YAML is case sensitive.

```yml
# Syntax for Actions
name: TheName
on: [event]

jobs:
  jobname:
  runs-on: os-system
  steps:
    uses: something-here
    uses: something-here
    run: filename.ext
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## GitHub Actions

Notes from various YouTube videos.

## Video 1

1. Video: [How to get started with GitHub Actions](https://youtu.be/-ZiuJdT1i0k) by Eddie Jaoude, 11/2/2022 |

- He created a simple repo on GitHub and he is doing everything in the browser
- He created `index.js` with a console log
- Create a `.github` folder > then create `workflow` folder inside that and finally add `build.yml`
- It starts with a `name` property
- Next: what will trigger this to run - a push, a PR, a manual trigger, a comment on an issue or anything on GitHub can trigger it - you can add filters and triggers later
- Next: a list of jobs to run when this is triggered
- This guy is not a good teacher - WTF - do I use ubuntu if I am on windows - yes/no? WTF - how about some more details and explanation???
- Save and commit then go to the _Actions_ tab or notice that the commit hash is orange and you can click on that -
- Common `workflow` files: `build`, `test`, `deploy`, `validate`, `labeler`, `upload`, `download`, `analysis`
- You can click on the file and see the log after it ran - I saw these on freeCodeCamp when I contributed changes - I saw the tests running
- Always have a `build` action for every push

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### Video 2

2. Video: [Let's create a GitHub Action (super easy tutorial)](https://youtu.be/COPS4VMfaUc) by Code with Ania Kubow, Aug 29, 2020 | [Her repo](https://github.com/kubowania/my-javascript-action)

- Very common to use actions for at least Pull Requests creating actions to perform (Continuous Integration Tests or Code Quality Checks) and based on their pass or fail, allow the PR to be merged with master/main
- That is all so you do not have to test each PR yourself
- She created `action.yml` in the root: see code at bottom
- And also `.github/workflows/main.yml`: see code at bottom
- Then `index.js`:

```js
const core = require('@actions/core');
const github = require('@actions/github');

try {
  // `who-to-greet` input defined in action metadata file
  const nameToGreet = core.getInput('who-to-greet');
  console.log(`Hello ${nameToGreet}!`);
  const time = new Date().toTimeString();
  core.setOutput('time', time);
  // Get the JSON webhook payload for the event that triggered the workflow
  const payload = JSON.stringify(github.context.payload, undefined, 2);
  console.log(`The event payload: ${payload}`);
} catch (error) {
  core.setFailed(error.message);
}
```

- She has 2 dependencies in [package.json](https://github.com/kubowania/my-javascript-action/blob/master/package.json): `@actions/core` and `@actions/github`

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Video 3

3. Video: [5 Ways to DevOps-ify your App - Github Actions Tutorial](https://youtu.be/eB0nUzAI7M8) by Fireship, 2020

- GitHub events to trigger an Action: star, issue, PR, ...
- There are community actions

#1 **Continuous Integration** (CI):

- Have devs submit their code to the main codebase in small maintainable chunks
- Those changes need to be tested against the codebase
- He uses Jest for testing
- Actions tab - once a again, have a test for every PR -
- He has `.github/workflows/integrate.yml` - github will pick up anything in `workflows` and automatically setup as a workflow in the cloud
- `name` property: always the first thing
- `on` object: tell it on which event(s) to run - e.g.:

```yml
on:
  pull_request:
    branches: [master]
```

- `jobs` object: a workflow should have 1 or more jobs, and first give it a name (test_pull_requests for this example) - then tell it which vm to run on (ubunto, windows or mac os)
- What about `runs-on`?
- next give it `steps` or a set of instructions: see code at end
- What about `uses`, `with` and `run`?
- Need to install node dependencies with `npm ci` then `npm test` and `npm build`

His `package.json` script:

```json
"scripts": {
  "test": "jest",
  "build": "webpack src/app.js -o public/bundle.js",
  "deploy": "firebase deploy --token \"$FIREBASE_TOKEN\" --only hosting"
}
```

#2 **Continuous Deployment** (CD):

> Holy cow this guy talks fast

- CI is about merging code into the codebase, CD is about pushing the code out to customers, `.github/workflows/deploy.yml`: see code at end
- CI/CD pipeline?
- Publish NPM packages: about 8:30 - SKIP
- Integrate Apps: discord, slack, trello, etc - Actions vs Apps
- Check out _crontab.guru_

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Video 4

4. Video: [Github Actions CI/CD - Everything you need to know to get started](https://youtu.be/mFFXuXjVgkU) by DevOps Journey, Mar 12, 2021

- YAML Files: specifies Events, Jobs, Runners, Steps and Actions
  - An _Event_ is a trigger for a workflow (PR)
  - That will trigger all the _jobs_ within the workflow
- So 1) event triggered, 2) the job name is ran, 3) runs on a container hosted on github, 4) goes thru the step(s)

Back on Github:

> they are always creating a new repo in github

- The naming for your workflow directory need to be **exactly** `.github/workflows/`
- The yellow status circle icon: means the workflow is currently running - it turns to a green check mark when the tests pass, or red ("X" in red circle) if they fail
- You can have your servers check the GitHub API and check the status of the repo
- Instead of writing the yml file from scratch, on Actions tab click New Workflow > you will see a bunch of templates
- I actually just see them on my Actions tab and I don't see a btn for New Workflow
- Check Marketplace > Actions

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Video 5

5. Video: [Automatic Deployment With Github Actions](https://youtu.be/X3F3El_yvFg) by Traversy Media, Oct 12, 2020

- He added and commited his changes, then push
- Wow, somehow pushing changes to GitHub updated his live page -> that is an Automatic Deployment
  - a GitHub action runs all the necessary scripts and if successful you can refresh your page to see the changes

#### Setup

His file I think: [yml file](https://github.com/graphyql/reactjs-tutorial/blob/master/.github/workflows/node.js.yml)

1. Create a new repo and tie it to your local project
1. Back on GitHub you need to set up a workflow, Actions >
1. He chose `node.js` since he has a React app which is one of the starter actions
1. Then he customized it

Note: You can run multiple versions of node by changing the version number

- He needs a "runner" on his self-hosted server ???
- He went to Settings > Actions > and selected "Enable local and third party Actions for this repository"
- Then click Add Runner > from the drop-down list select the OS you are on
- There are notes on that page ... a lot of things to do but the code for the terminal is all you want - d\l a `tar.gz` file - WTF
- Then he mentions that you may run into an error (10:00)...blah-blah-blah...create a new user acct on your server - HUH???
- I'm stopping - here is what the rest of the video has:

```
14:37 - Setting up NGINX
21:09 - Pointing domain to our Server
22:17 - React Routing with NGINX
22:52 - Setting up Expressjs API
30:14 - Proxy Passing with NGINX
39:04 - Setting up Server Block
40:53 - Securing our Website
```

> WTF? Where is the super simple Actions tutorial for beginners?

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Video 6

6. Video: New guy, Getting Started with GitHub Actions Tutorial from his playlist [Master GitHub Actions Tutorial](https://www.youtube.com/playlist?list=PL_RrEj88onS-um2xFy01sY46ik_2yt_EQ), 11 videos

- On Actions tab choose _Static HTML_ under **Pages**
- Change the file name and `name` property if you want
- Spacing is crucial!
- NO - I went to Marketplace and chose [Super-Linter](https://github.com/marketplace/actions/super-linter)

#### Super-Linter

```yml
name: Lint Code Base

# Start the job on all push #
on:
  push:
    branches-ignore: [master, main]
    # Remove the line above to run when pushing to master
  pull_request:
    branches: [master, main]

# Set the Job #
jobs:
  build:
    # Name the Job
    name: Lint Code Base
    # Set the agent to run on
    runs-on: ubuntu-latest

    # Load all steps #
    steps:
      # Checkout the code base #
      - name: Checkout Code
        uses: actions/checkout@v3
        with:
          # Full git history is needed to get a proper
          # list of changed files within `super-linter`
          fetch-depth: 0

      # Run Linter against code base #
      - name: Lint Code Base
        uses: github/super-linter@v4
        env:
          VALIDATE_ALL_CODEBASE: false
          DEFAULT_BRANCH: master
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

- I merged that new file from GitHub which I added to a branch called `linter-action` and I saw the checks running. Then I merged it.
- But I had changes in `main` locally and I was able to push them without an error
- That may have been because it was not merged yet.
- I tried `git pull upstream master` but got an error that there was no upsteam. I read an Atlassian article, but then tried `git pull` and that worked (I have a lot more to learn about git)
- Pushed the changes - didn't trigger anything, then I removed `main` from the followingpart of the yml file:

```yml
on:
  push:
    branches-ignore: [master, main]
    # Remove the line above to run when pushing to master
  pull_request:
    branches: [master, main]
```

- Added an unused variable to `index.js` and the linting FAILED! Removed it, pushed again and it FAILED AGAIN

```js
// I tried changing to single quotes on for prettier in settings.json
const logString = `Dotfiles must be important because it is hard getting good notes on them`;
console.log(logString);
```

> OKAY, I LIKE ACTIONS!

Things to check in Super-Linter `linter.yml`:

`env` section:

- `DEFAULT_BRANCH`: changed mine from `master` to `main`
- you don't want your PR to fail because of the linter: add `DISABLE_ERRORS: true` but this is not recommended
- use `VALIDATE_ALL_CODEBASE: false` so that it doesn't check your entire codebase which would tale a long time for large repos - When set to `false`, only new or edited files will be parsed for validation

Why are things failing? Check out thie [Super-Linter on GitHub](https://github.com/github/super-linter)

This: `VALIDATE_ALL_CODEBASE: false` broke the action???

> Settings > Code and automation > Branches > Require a pull request before merging > Reuire approvals (1)

Pages: For the default actions, there are ones for Gatsby, Hugo and Astro

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## GitHub Actions Categories

Consider selecting the filter _Verification : Verified creator_. That is what I chose for all the specific categories/actions listed below:

1. **API management**: Check out [action-github-app-token](https://github.com/marketplace/actions/action-github-app-token), uses GitHub Apps to fetch a GitHub auth token for a GitHub App installation. The GitHub App is used to authorize API access across multiple repositories - `yarn` only?
1. **Chat**: Check out [slack-send](https://github.com/marketplace/actions/slack-send) and maybe [Twillio SMS](https://github.com/marketplace/actions/twilio-sms)
1. **Code quality**: Check out [action-git-diff-suggestions](https://github.com/marketplace/actions/action-git-diff-suggestions) (_issue with this one_), [validate-license-action](https://github.com/marketplace/actions/validate-license-action) (Validate a license file is one of the allowed licenses), [IntelliCode Team Completions](https://github.com/marketplace/actions/intellicode-team-completions), and [**Super-Linter**](https://github.com/marketplace/actions/super-linter)
1. **Code review**: `action-git-diff-suggestions` is also in this category, [SecureStack Application Bill of Materials (ABOM/SBOM)](https://github.com/marketplace/actions/securestack-application-bill-of-materials-abom-sbom), [Kubernetes Security Config Watch](https://github.com/marketplace/actions/kubernetes-security-config-watch), and [**Release-Notes-Preview**](https://github.com/marketplace/actions/release-notes-preview)
1. **Continuous integration**: 166 results, too many to go through although Git Version looks good
   1. Container CI: 41 results
   1. Game CI: 1 result
   1. Mobile CI: 2 results
1. **Dependency management**: [Cache](https://github.com/marketplace/actions/cache) and [Dependency Review](https://github.com/marketplace/actions/dependency-review)
1. **Deployment**: 113 results, too many to review
1. **IDEs**: `IntelliCode Team Completions` is also here
1. **Learning**: only 2 results with Verified Creator filter
1. **Localization**: no results with Verified Creator filter
1. **Mobile**: 5 results, no idea what they do
1. **Monitoring**: Check out [Load runner information](https://github.com/marketplace/actions/load-runner-information)
1. **Project management**: A lot dealing with Jira, also [Teamwork GitHub Sync](https://github.com/marketplace/actions/teamwork-github-sync), [**Issue comment tag**](https://github.com/marketplace/actions/issue-comment-tag), and [Add to GitHub projects](https://github.com/marketplace/actions/add-to-github-projects)
1. **Publishing**: 17 results, a few each for Google, Microsoft, and Github Pages of which [Deploy GitHub Pages site](https://github.com/marketplace/actions/deploy-github-pages-site) and [Configure GitHub Pages](https://github.com/marketplace/actions/configure-github-pages) look good
1. **Security**: 100 results, look into [Kubernetes Security Config Watch](https://github.com/marketplace/actions/kubernetes-security-config-watch) and [Authenticate to Google Cloud](https://github.com/marketplace/actions/authenticate-to-google-cloud)
1. **Support**: 7 Jira results
1. **Testing**: 26 results, 7 dealing with Parasoft, check out [RapidAPI Testing Trigger](https://github.com/marketplace/actions/rapidapi-testing-trigger)
1. **Utilities**: 72 results, the most interesting Action titles: [Close stale issues](https://github.com/marketplace/actions/close-stale-issues), [**First interaction**](https://github.com/marketplace/actions/first-interaction), [Setup Node.js environment](https://github.com/marketplace/actions/setup-node-js-environment), [GitHub GraphQL API Query](https://github.com/marketplace/actions/github-graphql-api-query), [**GitHub API Request**](https://github.com/marketplace/actions/github-api-request), [Release-Notes-Preview](https://github.com/marketplace/actions/release-notes-preview), [Issue comment tag](https://github.com/marketplace/actions/issue-comment-tag), [Labeler](https://github.com/marketplace/actions/labeler), and [GitHub Script](https://github.com/marketplace/actions/github-script)
   1. Backup utilities: 0 results

**NOTES**: `validate-license-action` has not been updated in 4 years, `Release-Notes-Preview` in 3 years (I added it anyway), `Issue comment tag` requires a GitHub App.

### Actions I used

1. Super Linter: It works but I it keeps failing. Look into the docs.
1. Release-Notes-Preview: Did not run: _This workflow has no runs yet_
1. First Interaction: Failed / did not even run. File: `actions.yml`, see code below - removed `actions.yml`.

### Without Verified creator filter

I tried 3 Actions, 1 had issues, and 1 did not run. Now I'm sorting by Most Stars (1st page results only). There should be more search parameters to limit the number of results:

1. **API management**: 307 results, [**Fetch API Data Action**](https://github.com/marketplace/actions/fetch-api-data)
1. **Chat**: 320 results, a lot of Slack Actions, [Issues Translator](https://github.com/marketplace/actions/issues-translator), [Actions for Discord](https://github.com/marketplace/actions/actions-for-discord), [Action Status Discord](https://github.com/marketplace/actions/actions-status-discord), [Twitter Action](https://github.com/marketplace/actions/twitter-action)
1. **Code quality**: 1352 results, [**GraphQL Inspector**](https://github.com/marketplace/actions/graphql-inspector), [Check PHP syntax errors](https://github.com/marketplace/actions/check-php-syntax-errors), [MegaLinter](https://github.com/marketplace/actions/megalinter), [Lint Action](https://github.com/marketplace/actions/lint-action)
1. **Code review**: [Changed Files](https://github.com/marketplace/actions/changed-files), [semantic-pull-request](https://github.com/marketplace/actions/semantic-pull-request), [dotenv-linter](https://github.com/marketplace/actions/dotenv-linter), [Auto Assign Action](https://github.com/marketplace/actions/auto-assign-action)
1. **Continuous integration**: 5354 results, [Setup PHP Action](https://github.com/marketplace/actions/setup-php-action), [SSH Remote Commands](https://github.com/marketplace/actions/ssh-remote-commands), [Cypress.io](https://github.com/marketplace/actions/cypress-io)
   1. Container CI: 934 results
   1. Game CI: 87 result
   1. Mobile CI: 157 results
1. **Dependency management**: 713 results, [NPM or Yarn install with caching](https://github.com/marketplace/actions/npm-or-yarn-install-with-caching), [Dependency review](https://github.com/marketplace/actions/dependency-review), [Install PHP Dependencies with Composer](https://github.com/marketplace/actions/install-php-dependencies-with-composer), [The PHP Security Checker](https://github.com/marketplace/actions/the-php-security-checker)
1. **Deployment**: 3076 results, [**Astro Deploy**](https://github.com/marketplace/actions/astro-deploy), [GitHub Pages Action](https://github.com/marketplace/actions/github-pages-action), [Deploy to GitHub Pages](https://github.com/marketplace/actions/deploy-to-github-pages)
1. **IDEs**: 36 results, [**TODO to Issue**](https://github.com/marketplace/actions/todo-to-issue), [Visual Studio shell](https://github.com/marketplace/actions/visual-studio-shell), [Try in Web IDE](https://github.com/marketplace/actions/try-in-web-ide)
1. **Learning**: 117 results, [NodeJS Action Template](https://github.com/marketplace/actions/nodejs-action-template)
1. **Localization**: 70 results, [Translation Action](https://github.com/marketplace/actions/translation-action), [Pot File Generator For WordPress](https://github.com/marketplace/actions/pot-file-generator-for-wordpress) and [WordPress .pot File Generator](https://github.com/marketplace/actions/wordpress-pot-file-generator)
1. **Mobile**: 94 results, [Send Push Notification](https://github.com/marketplace/actions/send-push-notification)
1. **Monitoring**: 311 results, `GraphQL Inspector` also in this category, [**Lighthouse check**](https://github.com/marketplace/actions/lighthouse-check), [Send email](https://github.com/marketplace/actions/send-email), [github-repo-stats](https://github.com/marketplace/actions/github-repo-stats), [**Profile readme stats**](https://github.com/marketplace/actions/profile-readme-stats)
1. **Project management**: 913 results, [Create release](https://github.com/marketplace/actions/create-release), [GitHub deployments](https://github.com/marketplace/actions/github-deployments), [GitHub Project Automation+](https://github.com/marketplace/actions/github-project-automation) and [Add To GitHub projects](https://github.com/marketplace/actions/add-to-github-projects), [Pull request stats](https://github.com/marketplace/actions/pull-request-stats)
1. **Publishing**: 1723 results, [Blog post workflow](https://github.com/marketplace/actions/blog-post-workflow), [WordPress Plugin Deploy](https://github.com/marketplace/actions/wordpress-plugin-deploy), [Install SSH Key](https://github.com/marketplace/actions/install-ssh-key), [Deploy to Heroku](https://github.com/marketplace/actions/deploy-to-heroku)
1. **Security**: 769 results, [Is Website vulnerable](https://github.com/marketplace/actions/is-website-vulnerable), [Authenticate to Google Cloud](https://github.com/marketplace/actions/authenticate-to-google-cloud),
1. **Support**: 214 results, [**Pull Request title rules**](https://github.com/marketplace/actions/pull-request-title-rules), [**Git Commit Data**](https://github.com/marketplace/actions/git-commit-data)
1. **Testing**: 1142 results, [markdown-link-check](https://github.com/marketplace/actions/markdown-link-check)
1. **Utilities**: 4320 results, [Metrics embed](https://github.com/marketplace/actions/metrics-embed), [Setup PHP action](https://github.com/marketplace/actions/metrics-embed), [Setup Node.js environment](https://github.com/marketplace/actions/setup-node-js-environment), [**GitHub-Profile-Summary-Cards**](https://github.com/marketplace/actions/github-profile-summary-cards)
   1. Backup utilities: 87 results, [google-drive-upload-git-action](https://github.com/marketplace/actions/google-drive-upload-git-action), [Mirror to BitBucket GitHub Action](https://github.com/marketplace/actions/mirror-to-bitbucket-github-action), [repo-backup-arweave](https://github.com/marketplace/actions/repo-backup-arweave)

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Work flow rules

Notes from the Discord server for the repo `contrast-ratio-repo` that I did a few contributions to.

> It took me an entire Saturday to go through a week of Discord messages. Why wasn't a discussion on GitHub created and pinned?

1. All discussions for new features will be in the `discussions` tab on github. Check that periodically and leave your comments.
1. Once we have decided on features, then it will be broken into `issues` for people to pick up and work on.
   1. Make sure to break the work down into tickets for everyone to work on.
1. Once you pick up an issue, make sure to
   1. Create a new branch and make your changes there.
   1. Then you will need to push your changes to GitHub and create a new PR.
   1. Then request a review so we can review changes and approve it and merge it into `main`.
   1. Also make sure to link the issue you are working on to that PR.
1. Once your PR is merged, you are free to delete the branch from both the remote and local repos. Then make sure to update your main branch locally by using git pull.
1. Then update your main branch using `git fetch` && `git rebase` if you feel comfortable with that option.
1. While you are waiting for a review on your PR, feel free to pick up any new issue that hasn't been assigned yet.
1. Please make sure to [link the issue you are working on to the PR](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue) you created. That will automatically close the issue when the PR is merged so we don't have to remember to do that ourselves and have old issues on the board that has been completed.
1. When you create a PR, you can request a review from someone on the team and they will be alerted through email to leave a review

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Example YAML Code

From Ania's video:

```yml
# action.yml
name: 'Hello World'
description: 'Greet someone and record the time'
inputs:
  who-to-greet: # id of input
    description: 'Who to greet'
    required: true
    default: 'World'
outputs:
  time: # id of output
    description: 'The time we greeted you'
runs:
  using: 'node12'
  main: 'index.js'
```

```yml
# .github/workflows/main.yml
on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      - name: Hello world action step
        id: hello
        uses: Kernix13/my-javascript-action@v1
        with:
          who-to-greet: 'Jim'
      # Use the output from the `hello` step
      - name: Get the output time
        run: echo "The time was ${{ steps.hello.outputs.time }}"
```

From video #3:

```yml
# .github/workflows/deploy.yml
name: Firebase Continuous Deployment

on:
  push:
    branches: [master]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - uses: actions/setup-node@master
        with:
          node-version: 12
      - run: npm ci
      - run: npm run build
      - uses: w9jds/firebase-action@master
        with:
          args: deploy --only hosting
        env:
          FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
```

```yml
# .github/workflows/integrate.yml
name: Node Continuous Integration

on:
  pull_request:
    branches: [master]

jobs:
  test_pull_request:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12
      - run: npm ci
      - run: npm test
      - run: npm run build
```

The YAML file I got fom Eddie I think

```yml
# .github/workflows/integrate.yml
name: Build
on: [push]

jobs:
  build:
  # WTF
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v3
    - uses: actions/setup-node@v3
    - with:
        node-version: '16'
    - run: node index.js
```

First Interaction `action.yml`

```yml
name: 'First interaction'
description: 'Greet new contributors when they create their first issue or open their first pull request'
author: 'GitHub'
inputs:
  repo-token:
    description: 'Token for the repository. Can be passed in using {{ secrets.GITHUB_TOKEN }}'
    required: true
  issue-message:
    description: 'Congratulations for posting your first issue!'
  pr-message:
    description: 'Congratulations, this is your first pull request!'
runs:
  using: 'docker'
  image: 'Dockerfile'
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## GitHub Actions Crash Course

[Video](https://youtu.be/1oJQRlz1v94) from the YouTube Channel Laith Academy

- From 7:30, Add a new workflow - click the New workflow button - that takes you to boilerplate Actions which you can pick
- He grabbed a Node.js action - clicked on Configure for one of the actions > copied all the YAML code and hit the back button to cancel adding that action
- instead he added it to a file in `.github` > `workflows` > `filename.yml` - his file runs on a Pr and to run a build job and to run tests
- He says this is an integration workflow so he named it `integration.yml` and he pasted in all the code - delete the comments, give it a new `name`, indent for nesting, e.g., an `on` blick, then inside `push` and `pull_request` blocks - inside of both is a `branches` key/value pair

### YML Code Blocks

- The `on` block specifies when to run these jobs, like _on_ a `pull_request`
- The `jobs` block is what will run based on the `on` code blocks - one he has is `build`

#### build block

- in this block is nested `runs-on`, `strategy`, and `steps`
- `runs-on`: this job has to run on a server which is hosted by GitHub
- `steps`: the steps needed in order to run the job - inside are keys of `uses`, `name`, `with`, and `run`
  - Check the keywords in the `uses` value and search for them and you will find a repo on it like `actions/checkout`
  - Checkout the account `actions` then search for `checkout`, `setup`, etc.
  - `with`: specifies the different versions of Node (for this example) you want to use and this is where `strategy` comes in
- `strategy`: aloows you to specify multiple Node versions - they are in an array nested in `matrix` > `node-version` and inside `with` > `node-version` he has `matrix.node-version`
- `run` commands: an example line would be:

```yml
- run: npm install
- run: npm run build
```

He also mentions writing the units test block so he copied the build block, pasted it inside the `jobs` block and then edited it. He litterally used all the same code except changed `npm run build` to `npm run test`.

### Test your action

- He created a new branch, add, commit, push - on GitHub create a PR, and then you action will start running since it was set up for PR's
- If you click on the action while it is running you will see the steps running - the first few are "health" checks - then you see your `run` blocks
- They will either pass or fail
- He has it set up for PR's and pushes/merges with the `main` branch
- Back in VS Code checout to `main`, do `git pull`, create a new branch to test failure
- He made a small change to his source code to cause a failure - now you can track what has passed and what has failed
- But even though the tests failed, you can still merge the PR - but you may want to block that ability - see _24:50_ in video, but:
  - Go to the actions repo > braches > Vranch protection rules > set it up there
- Wait, he did all that somewhere else (watch again)

### Docker Image

- He created a new YAML file, copied the code from `integration.yml` and edited it - SKIP (29:00)

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## New Notes

> Ignore everything above because I have not edited yet. Hre are notes from ChatGPT

### YAML spacing rule

> linter.yml

- Spaces only. Never tabs. GitHub will absolutely punish you for tabs.
- Use 2 spaces per indent. That’s the common convention.

### custom action definitions vs workflow file

- custom action definitions live in a /actions folder and contain:
  - name
  - description
  - runs
- workflow file line in .github/
  - .github/workflows/something.yml
- A workflow runs automation
- An action is something a workflow uses

### Structure Breakdown (The Only Keywords You Need to Remember)

Top-level:

- name: Just a label
- on: What triggers it.
- jobs: You must define at least one job - Each job needs:
  - a job name (you invent it)
  - runs-on
  - steps
- runs-on: ubuntu-latest -> means "Run this job on a virtual machine provided by GitHub"
  - Common options: ubuntu-latest, windows-latest, macos-latest
- steps: Steps are just a list
- Each step either:
  - runs a shell command (run)
  - uses a prebuilt action (uses)
- a workflow can have multiple jobs, but it doesn’t have to

```
jobs:
  build:
    ...
  deploy:
    ...
```

> Once you add branches, push must become an object, not a string
> You must always specify runs-on for each job

- push -> branches: [push, pull_request]
- push -> branches-ignore [main]
- branches expects a list

```yml
branches: [main]
# or the long version:
branches:
  - main
```

> You see the messages: GitHub → Your Repo → Actions tab → Click the workflow → Click the job → View logs

Other easy yaml files for:

```yaml
on:
  issues:

on:
  pull_request:

# filter by activity type
on:
  issues:
    types: [opened]

on:
  pull_request:
    types: [opened, closed]
```

### Common Keywords in Workflows

These are the core ones you’ll see over and over:

- name = label
- on = trigger
- jobs = container for work
- runs-on = required machine
- steps = list
- uses = external action
- run = shell command
- with
- env

> You don’t add workflows just because you can. You add them when they automate something useful.

### Informational / Logging

Example:

- Log when someone opens an issue
- Log when a PR is merged
- Echo something for visibility

This is mostly learning/demo value.

### Automation

More useful:

- Add labels automatically
- Assign reviewers
- Close stale issues
- Block PR if conditions fail
- Enforce naming conventions
- Check commit messages

### Interaction / Community

That one responds when someone opens their first issue or PR and posts a welcome message. That’s useful for:

- Open source projects
- Public repos
- Growing a community

## actions for issues and PRs

When someone contributes to open source:

1. They don’t know your conventions
2. They don’t know if their PR was seen
3. They don’t know what happens next

Automation gives:

- Immediate feedback
- Clear structure
- Professional feel
- Less manual work for you

`uses: actions/github-script@v7`

Format always looks like:

```yaml
uses: owner/repo@version
```

- owner → actions
- repo → github-script
- version → v7

Search for:

- "comment on issue"
- "label PR"
- "lint"
- "auto assign"

### Why with: and script:?

Every action can accept inputs.

```yaml
- uses: owner/repo@version
  with:
    inputName: value

  with:
  script: |
    github.rest.issues.createComment(...)
```

- The with: block passes parameters to that action.
- In github-script, one of its inputs is called script.

### Run npm install + build on PR

Because your local machine passing ≠ someone else’s environment passing.

CI proves it builds cleanly.

Example idea (not writing full file):

- checkout repo
- setup node
- run npm ci
- run npm run build

### Professional

- Lint on PR
- Run tests on PR
- Require status checks before merge

> Linkters only take care of linting if someone runs them locally - GitHub Actions enforces it.

The most practical beginner workflow to learn next is:

- 👉 Run npm ci
- 👉 Run npm run lint
- 👉 Run npm run build

On pull_request.

> `|` allows multiline text

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

<!--
```yaml
---
name: Lint Code Base

#############################
# Start the job on all push #
#############################
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

###############
# Set the Job #
###############
jobs:
  build:
    # Name the Job
    name: Lint Code Base
    # Set the agent to run on
    runs-on: ubuntu-latest

    ##################
    # Load all steps #
    ##################
    steps:
      ##########################
      # Checkout the code base #
      ##########################
      - name: Checkout Code
        uses: actions/checkout@v3
        with:
          # Full git history is needed to get a proper
          # list of changed files within `super-linter`
          fetch-depth: 0

      ################################
      # Run Linter against code base #
      ################################
      - name: Lint Code Base
        uses: github/super-linter@v4
        env:
          VALIDATE_ALL_CODEBASE: false
          DEFAULT_BRANCH: main
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          VALIDATE_MARKDOWN: false
```
-->
