# Eslint + Prettier Config
Personal settings for ESLint and Prettier

## Aims
* JS Rules based on best practice, and personal taste
* JS Format errors fixed with Prettier
* Lints + Fixes for html script tags
* Lints + Fixes for React via eslint-config-airbnb
* [rules here](https://github.com/jawford/eslint-config-jawford/blob/master/.eslintrc.js) - Overwritable per project.

## Installing

Install eslint globally and/or locally per project.

Install this locally per project to allow project specific settings.

Install this globally, along with eslint, for projects or rogue JS without their own specific setup.

## Local Install

1. npm initialisation

2. Install this package's peer dependencies into devDependencies, as well as the config itself:

```
npx install-peerdeps --dev eslint-config-jawford
```

3. Check local devDependencies contain those peer-deps

4. Create `.eslintrc`, at root next to package.json:

```json
{
  "extends": [
    "jawford"
  ]
}
```

Alternatively add this object directly in `package.json` under the property `"eslintConfig":`

5. Optionally add two scripts to package.json to manually lint and/or fix by running `npm run lint`, or `npm run lint:fix`:

```json
"scripts": {
  "lint": "eslint .",
  "lint:fix": "eslint . --fix"
},
```

The preference is probably to let the editor do this. See below

## Global Install
After testing the local per-project setup this package can also be added to the global npm setup for ad-hoc usage

1. Global install

```
npx install-peerdeps --global eslint-config-jawford
```

2. Minimal global `~/.eslintrc` (or `C:\Users\username\.eslintrc`)

```json
{
  "extends": [
    "jawford"
  ]
}
```

3. CLI invocation is `eslint .` — or use an editor/IDE to run this e.g. on file saves

## Local Project Settings

Extend / override these settings with local rules in a `.eslintrc` file.  
[eslint rules here](https://eslint.org/docs/rules/) see below`"rules"`.  
[prettier rules here](https://prettier.io/docs/en/options.html) see below `"prettier/prettier"`.

Prettier rules will overwrite anything in this config (trailing comma, and single quote), so you'll need to include those as well. 

```json
{
  "extends": [
    "jawford"
  ],
  "rules": {
    "no-console": 2,
    "prettier/prettier": [
      "error",
      {
        "trailingComma": "es5",
        "singleQuote": true,
        "printWidth": 120,
        "tabWidth": 8,
      }
    ]
  }
}
```

## VS Code

To lint and fix on file save

1. Install the [Dirk Baeumer's ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
2. Preference settings to **run on save**  
By default VS Code tries to beautify code with its own built-in beautifier
  ```js
  // auto beautify on save? yes, but...
  "editor.formatOnSave": true,
  // ...within JS & JSX turn off the built-in
  "[javascript]": {
    "editor.formatOnSave": false
  },
  "[javascriptreact]": {
    "editor.formatOnSave": false
  },
  // ...and run the ESLint extension on save
  "eslint.autoFixOnSave": true,
  // If the prettier extension is used & enabled for other languages like CSS and HTML, then turn it off for JS since it will run via Eslint already
  "prettier.disableLanguages": ["javascript", "javascriptreact"],
  ```

## With Create React App

1. Eject `npm run eject`
1. Install `npx install-peerdeps --dev eslint-config-wesbos`
1. Edit `package.json` and replace `"extends": "react-app"` with `"extends": "jawford"`


## NOT WORKING?

Try a re-install globally and locally.

```bash
npm remove [--global] eslint-config-wesbos babel-eslint eslint eslint-config-prettier eslint-config-airbnb eslint-plugin-html eslint-plugin-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react prettier eslint-plugin-react-hooks
```

Locally remove `package-lock.json` and `node_modules/`
