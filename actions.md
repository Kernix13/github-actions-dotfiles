# Notes on Actions

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

## GitHub Actions

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

---

2. Video: [Let's create a GitHub Action (super easy tutorial)](https://youtu.be/COPS4VMfaUc) by Code with Ania Kubow, Aug 29, 2020 | [Her repo](https://github.com/kubowania/my-javascript-action)

- Very common to use actions for at least Pull Requests creating actions to perform (Continuous Integration Tests or Code Quality Checks) and based on their pass or fail, allow the PR to be merged with master/main
- That is all so you do not have to test each PR yourself
- She created `action.yml` in the root: see code at bottom
- And also `.github/workflows/main.yml`: see code at bottom
- Then `index.js`:

```js
const core = require("@actions/core");
const github = require("@actions/github");

try {
  // `who-to-greet` input defined in action metadata file
  const nameToGreet = core.getInput("who-to-greet");
  console.log(`Hello ${nameToGreet}!`);
  const time = new Date().toTimeString();
  core.setOutput("time", time);
  // Get the JSON webhook payload for the event that triggered the workflow
  const payload = JSON.stringify(github.context.payload, undefined, 2);
  console.log(`The event payload: ${payload}`);
} catch (error) {
  core.setFailed(error.message);
}
```

- She has 2 dependencies in [package.json](https://github.com/kubowania/my-javascript-action/blob/master/package.json): `@actions/core` and `@actions/github`

---

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

#1 **Continuous Deployment** (CD):

> Holy cow this guy talks fast

- CI is about merging code into the codebase, CD is about pushing the code out to customers, `.github/workflows/deploy.yml`: see code at end
- CI/CD pipeline?
- Publish NPM packages: about 8:30 - SKIP
- Integrate Apps: discord, slack, trello, etc - Actions vs Apps
- Check out _crontab.guru_

3. Video: [Github Actions CI/CD - Everything you need to know to get started](https://youtu.be/mFFXuXjVgkU) by DevOps Journey, Mar 12, 2021

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

4. Video: [Automatic Deployment With Github Actions](https://youtu.be/X3F3El_yvFg) by Traversy Media, Oct 12, 2020

- He added and commited his changes, then push
- Wow, somehow pushing changes to GitHub updated his live page -> that is an Automatic Deployment
  - a GitHub action runs all the necessary scripts and if successful you can refresh your page to see the changes

### Setup

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

## Example YAML Code

From Ania's video:

```yml
# action.yml
name: "Hello World"
description: "Greet someone and record the time"
inputs:
  who-to-greet: # id of input
    description: "Who to greet"
    required: true
    default: "World"
outputs:
  time: # id of output
    description: "The time we greeted you"
runs:
  using: "node12"
  main: "index.js"
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
          who-to-greet: "Jim"
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
        node-version: "16"
    - run: node index.js
```