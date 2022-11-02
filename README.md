[![GitHub Super-Linter](https://github.com/<OWNER>/<REPOSITORY>/workflows/Lint%20Code%20Base/badge.svg)](https://github.com/marketplace/actions/super-linter)

# Tutorial on GitHub Actions and Dotfiles

My attempt at answering:

1. What are GitHub actions?
1. What are they used for?
1. How do I get them or create them?
1. What are all the dotfiles in GitHub repositories?
1. What are they used for?
1. How and why do I use them?
1. What are boilerplate starter files for each?

## Files

I have some sample/template code for each file/folder type

### .github folder

In the root I have `pull_request_template.md`. In `/workflows` I have `build.yml` as an example (DELETED build.yml, added the code at bottom of actions.md).

### .husky

I have a `.gitignore` which has what everyone has in it: `_`. Then I have `pre-commit` which also has the same code that everyone has for that file.

### .vscode

I have `settings.json` for a sample from who-knows-where.

### Root folder dotfiles

I have the following:

1. `.editorconfig`: do you need this if you have `.vscode`?
1. `.eslintrc.json`: I've also seen `.eslintignore` but I do not have that file
1. `.prettierignore`: They always seem to have the `build` folder in here but freeCodeCamp has a massive one
1. `.prettierrc.json`: I've also seen this without any extension and with `.js`

### Markdown files

1. `actions.md`: my notes from the videos I watched
1. `dotfiles.md`: notes on the various files
