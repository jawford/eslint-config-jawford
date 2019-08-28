# Eslint + Prettier Config
Personal settings for ESLint and Prettier

## Aims

* Extendable Rules based on best practice
* Code quality fixed with TS/ESLint
* Format errors fixed with Prettier (ought to disable any conflicting format rules in the linter)
* Lints + Fixes for html script tags
* Lints + Fixes for React via eslint-config-airbnb
* [rules here](https://github.com/jawford/eslint-config-jawford/blob/master/.eslintrc.js) - Overwritable per project.

## Outline Steps
Either add an extension to the linting tool to format files with Prettier, ```eslint-plugin-prettier```, 
so there's only a single command to process a file, or run the linter and then Prettier as separate steps.

### ```eslint-config-prettier``` Disable ESLint formatting
This config disables ESLint's formatting rules that conflict with Prettier. It is extended from within .eslintrc configuration, 
In .eslintrc this is position sensitive so that it overrides other configs like ```eslint-config-airbnb```. 

```json
  "extends": ["airbnb", "prettier"]
```

### ```eslint-plugin-prettier``` Uses ESLint to run Prettier
This plugin adds a rule that formats content using Prettier. 
In .eslintrc this enables the plugin and sets the rule. Prettier will be run by ESLint.

```json
  "plugins": ["prettier"],
  "rules": { "prettier/prettier": "error" }
```

### Single "Recomended" Configuration, Combination Step
The eslint-plugin-prettier can be extended like a config. 
Here both ```eslint-plugin-prettier``` and ```eslint-config-prettier``` are configured together in a single "recomended" step.

```json
  "extends": ["plugin:prettier/recommended"]
}
```

### TSLint
```tslint-config-prettier``` and ```tslint-plugin-prettier``` are analogous to the above for Typescript formatting.

## Installing
After an npm install there are an abundance of dependencies in node_modules:

```json
	"dependencies": {
		"babel-eslint": "^9.0.0",
		"eslint": "^5.14.1",
		"eslint-config-airbnb": "^17.1.0",
		"eslint-config-prettier": "^4.1.0",
		"eslint-plugin-html": "^5.0.3",
		"eslint-plugin-import": "^2.16.0",
		"eslint-plugin-jsx-a11y": "^6.2.1",
		"eslint-plugin-prettier": "^3.0.1",
		"eslint-plugin-react": "^7.12.4",
		"eslint-plugin-react-hooks": "^1.3.0",
		"prettier": "^1.16.4"
	}
```

Also install
* eslint globally and/or locally per project.
* this package locally per project, to allow project specific settings.
* this package globally, along with eslint, for projects or rogue JS files without their own specific setup.

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
2. Preference settings to make eslint run **on file save**  
By default VS Code tries to beautify code with its own built-in beautifier
  ```js
  // auto beautify on save? yes, but...
  // ...turn off the built-in version within JS & JSX language
  // ...and run the ESLint extension autoFixOnSave
  // If the prettier extension is used & enabled for other languages like CSS and HTML, then turn it off for JS since it will run via Eslint already

  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.formatOnSave": false
  },
  "[javascriptreact]": {
    "editor.formatOnSave": false
  },
  "eslint.autoFixOnSave": true,
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
