# Using ESLINT and DOTFILES

Check out Eddie Jaoude repo [docs-1](https://github.com/eddiejaoude/docs-1) for various dot folders

What is the npm package `browser-sync` used for? In place of the LiveServer extension?

<div id="back-to-top"></div>

## Table of Contents

1. [Video 1](#video-1)
1. [Video 2](#video-2)
1. [Video 3](#video-3)
1. [Dotfiles](#dotfiles)
   1. [dot github folder](#dot-github-folder)
   1. [dot editorconfig](#dot-editorconfig)
   1. [dot prettier files](#dot-prettier-files)
   1. [dot browserslistrc file](#dot-browserslistrc-file)
   1. [Wes Bos dot eslint](#wes-bos-dot-eslint)
   1. [dot vscode folder](#dot-vscode-folder)
1. [contrast ratio package json file](#contrast-ratio-package-json-file)

## Video 1

Video title: 5 Reasons to IMMEDIATELY Turn On ESLint in VS Code

1. **Consistency**

- formatting in your code may be different than another contributor which can result in problems git commits or pushes
- Setting up ESLINt so you have rules in place so the formatting on either end was not conflicting

2. **Auto-Formatting**

- turn on the flags

3. Finding Bugs

- he is working with a VS Code extension I think

4. **Learning JS more**

- eslint rc file???
- Wes Bos ESLint config: https://github.com/wesbos/eslint-config-wesbos which is named `eslintrc.js`

5. **Developing opinions**

- overriding some of the rules

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Video 2

Video title: ESLint Quickstart - find errors automatically

ESLint helps having clean code and be free of errors - he creating a folder named `eslint-demo` which maybe I should do.

He is using a node package. He also is doing `git add` and `git commit` over and over without pushing!!!

Next: `npm install --sve-dev eslint`

- eslint comes with a built in way of initializing itself: `../node_modules/.btn/eslint --init` - answer the questions and that creates `eslintrc.json`
- ESLint has built-in recommended rules
- Basically, it will catch all sorts of errors or warnings
- you can also use `// eslint-disable-next-line` or be more specific with `// eslint-disable-next-line no-console` for a console.log line
- cmd line eslinting: use `npm run lint` to see the errors in the node cmd line

```js
// in package.json
"script": {
  "Lint": "eslint ./"
}
```

> What is airbnb in terms of linting?

## Video 3

Video title: VSCode ESLint, Prettier & Airbnb Guide Setup

By Brad Traversy

- there is a Github Airbnb style guide - single quotes for strings
- go t o the website to check any of the rules you see in the terminal
- `eslint-config-airbnb` - but it requires a react plugin
- he is installing ESLint extension and Prettier
- npm packages: `npm i -D eslint prettier eslint-plugin-prettier eslint-config-prettier` - he also installed `eslint-plugin-node eslint-config-node`
- to use the Airbnb styleguide: `npx install-peerdeps --dev eslint-config-airbnb` and that will instal `eslint`, `eslint-plugin-import`, `eslint-plugin-react`, `eslint-plugin-react-hooks`, and `eslint-plugin-jsx-a11y`

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### Config files

After all that create the config files

- `.prettierrc`: it's a json object: add any other rules/options you want

```json
{
  "singleQuote": true
}
```

- for ESLint, you can create the file manually as `.eslintrc.json` or install eslint globally and generate it - use the cms `sudo npm i -g eslint` then enter your password (what password?) - then use `eslint --init` to generate the file - his file:

```json
{
  "extends": ["airbnb", "prettier", "plugin:node/recommended"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error",
    "no-unused-vars": "warn",
    "no-console": "off",
    "func-names": "off",
    "no-process-exit": "off",
    "object-shorthand": "off",
    "class-methods-use-this": "off"
  }
}
```

- `"prettier/prettier": "error",`: need to enforce rules for Prettier
- `func-names`: has to do with the use of anonymous functions
- `no-process-exit`: used for node
- `object-shorthand`: ignore, I wouldn't have that one set to off
- `class-methods-use-this`: you get an error for classes that have a method that doesn't use the `this` keyword

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

## Dotfiles

dotfiles:

- `.editorconfig`
- `.eslintrc.json` | see [ESLint docs](https://eslint.org/docs/latest/)
- `.prettierrc` | see [Prettier Configuration File](https://prettier.io/docs/en/configuration.html) - I see this one a lot
- `.github` folder
- `.vscode` folder
- `.husky` folder

Here are some of those file from the repo I was briefly contributing to: [contrast-ratio-repo](https://github.com/jdwilkin4/contrast-ratio-repo)

### dot github folder

It contain markdown template files, and GitHub actions in YAML files in a `workflows` folder. Check out the [freeeCodeCamp repo](https://github.com/freeCodeCamp/freeCodeCamp) for some more detailed files.

`pull_request_template.md`:

```md
# Description

<!-- Please include a detailed summary of the changes made. Also please link the issue that this PR is fixing. -->

Fixes # (issue)

<!-- Please place an x in the [ ] to check off the type of change for this PR -->

## Type of change

- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Chore items (this includes basic clean up of files, or updates to packages)
- [ ] Updates to documentation

<!-- Please make sure to go through the entire checklist before requesting a review. Place an x in the [ ] to check off all of the items completed -->

## Checklist

- [ ] I have a descriptive title for my PR
- [ ] I have linked this PR to the corresponding issue. [See how to do that here.](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)
- [ ] I have run the project locally and reviewed the code changes
- [ ] My changes generate no new warnings
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### dot husky folder

Check out the [Husky docs](https://typicode.github.io/husky/#/). You can use it to lint your commit messages, run tests, lint code, etc... when you commit or push. Prettier

- `pre-commit`:
- Also has a `.gitignore` in the folder with only `_` in it

```sh
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```

- Check out the [Husky docs](https://typicode.github.io/husky/#/) and [Getting Started with Git Hooks and Husky](https://www.git-tower.com/blog/git-hooks-husky/)

## dot editorconfig

Check out [EditorConfig](https://editorconfig.org/).

```s
# EditorConfig is awesome: https://EditorConfig.org

# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
insert_final_newline = true

# Matches multiple files with brace expansion notation
# Set default charset
[*.{js,html}]
charset = utf-8

# 2 space indentation
[*.{js,html,css}]
indent_style = space
indent_size = 2

# Tab indentation (no size specified)
[Makefile]
indent_style = tab

# Indentation override for all JS under lib directory
[lib/**.js]
indent_style = space
indent_size = 2

# Matches the exact files either package.json or .travis.yml
[{package.json,.travis.yml}]
indent_style = space
indent_size = 2
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### dot prettier files

Check out the [Prettier docs](https://prettier.io/docs/en/install.html) for more detailed information. Prettier mentions in their docs you can use Huskt to "...have Prettier run before each commit".

**.prettierignore**:

```sh
# Ignore artifacts:
build
coverage
```

**.prettierrc.json**

```json
{
  "trailingComma": "es5",
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true,
  "printWidth": 9999,
  "arrowParens": "avoid"
}
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### dot browserslistrc file

This may be only for use with Vue.js projects and the vue-cli package. Check out [Vue CLI Browser Compatibility page](https://cli.vuejs.org/guide/browser-compatibility.html) for more information:

> You will notice a `browserslist` field in `package.json` (or a separate `.browserslistrc` file) specifying a range of browsers the project is targeting. This value will be used by @babel/preset-env and autoprefixer to automatically determine the JavaScript features that need to be transpiled and the CSS vendor prefixes needed.

I found different settings values for this file. Here is what you may see in `package.json`:

```json
"browserslist": [
    "defaults and supports es6-module",
    "maintained node versions"
  ]
```

And one version of what is in `.browserslistrc`:

```bash
# Browsers that we support

defaults and supports es6-module
maintained node versions
```

And here is what is in my file from a VueJS app:

```bash
# Browsers that we support

> 1%
last 2 versions
not dead
not ie 11
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### Wes Bos dot eslint

Also check out [ESLint docs](https://eslint.org/docs/latest/user-guide/getting-started).

```js
module.exports = {
  extends: ['airbnb', 'prettier'],
  parser: '@babel/eslint-parser',
  parserOptions: {
    requireConfigFile: false,
    babelOptions: {
      presets: ['@babel/preset-react'],
    },
  },
  env: {
    browser: true,
    node: true,
    jquery: true,
    jest: true,
  },
  rules: {
    'no-debugger': 0,
    'no-use-before-define': 'off',
    'import/no-cycle': 'off',
    'no-alert': 0,
    'no-await-in-loop': 0,
    'no-return-assign': ['error', 'except-parens'],
    'no-restricted-syntax': [
      2,
      'ForInStatement',
      'LabeledStatement',
      'WithStatement',
    ],
    'no-unused-vars': [
      1,
      {
        ignoreRestSiblings: true,
        argsIgnorePattern: 'res|next|^err',
      },
    ],
    'prefer-const': [
      'error',
      {
        destructuring: 'all',
      },
    ],
    'arrow-body-style': [2, 'as-needed'],
    'no-unused-expressions': [
      2,
      {
        allowTaggedTemplates: true,
      },
    ],
    'no-param-reassign': [
      2,
      {
        props: false,
      },
    ],
    'no-console': 0,
    'import/prefer-default-export': 0,
    import: 0,
    'func-names': 0,
    'space-before-function-paren': 0,
    'comma-dangle': 0,
    'max-len': 0,
    'import/extensions': 0,
    'no-underscore-dangle': 0,
    'consistent-return': 0,
    'react/display-name': 1,
    'react/no-array-index-key': 0,
    'react/react-in-jsx-scope': 0,
    'react/prefer-stateless-function': 0,
    'react/forbid-prop-types': 0,
    'react/no-unescaped-entities': 0,
    'react/function-component-definition': 0,
    'jsx-a11y/accessible-emoji': 0,
    'jsx-a11y/label-has-associated-control': [
      'error',
      {
        assert: 'either',
      },
    ],
    'react/require-default-props': 0,
    'react/jsx-filename-extension': [
      1,
      {
        extensions: ['.js', '.jsx', '.ts', '.tsx', '.mdx'],
      },
    ],
    radix: 0,
    'no-shadow': [
      2,
      {
        hoist: 'all',
        allow: ['resolve', 'reject', 'done', 'next', 'err', 'error'],
      },
    ],
    quotes: [
      2,
      'single',
      {
        avoidEscape: true,
        allowTemplateLiterals: true,
      },
    ],
    'prettier/prettier': [
      'error',
      {
        trailingComma: 'es5',
        singleQuote: true,
        printWidth: 80,
        // below line only for windows users facing CLRF and eslint/prettier error
        // non windows users feel free to delete it
        endOfLine: 'auto',
      },
    ],
    'jsx-a11y/href-no-hash': 'off',
    'jsx-a11y/anchor-is-valid': [
      'warn',
      {
        aspects: ['invalidHref'],
      },
    ],
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'warn',
    '@typescript-eslint/comma-dangle': ['off'],
    'react/jsx-props-no-spreading': 'off',
  },
  plugins: ['html', 'prettier', 'react-hooks'],
};
```

- Check out the [freeCodeCamp eslint.json](https://github.com/freeCodeCamp/freeCodeCamp/blob/main/.eslintrc.json) file and this [example on github](https://github.com/i-ron-y/eslintrc-starter-files/blob/master/.eslintrc.json)
- consider havine a `.eslintignore` file

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>

### dot vscode folder

I think this is just your `settings.json` file, possible just for thr project folder it is in

- ...to share settings, task configuration and debug configuration with the team. I think generally it makes sense to share settings

## contrast ratio package json file

> Look into `browser-sync`

```json
{
  "name": "contrast-ratio-repo",
  "version": "1.0.0",
  "description": "This is an app that checks the color contrast for web accessibility.",
  "main": "index.js",
  "scripts": {
    "start": "browser-sync start --server --files .",
    "format": "prettier --write .",
    "prepare": "husky install"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/jdwilkin4/contrast-ratio-repo.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/jdwilkin4/contrast-ratio-repo/issues"
  },
  "homepage": "https://github.com/jdwilkin4/contrast-ratio-repo#readme",
  "devDependencies": {
    "husky": "^8.0.1",
    "lint-staged": "^13.0.3",
    "prettier": "2.7.1"
  },
  "lint-staged": {
    "*.{js,css,md,html,json}": "prettier --write"
  },
  "dependencies": {
    "browser-sync": "^2.27.10"
  }
}
```

<div align="right">&#8673; <a href="#back-to-top" title="Table of Contents">Back to Top</a></div>
