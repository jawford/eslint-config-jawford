# Eslint + Prettier Config
Personal settings for ESLint and Prettier. Legacy methods ran the linter off the formatter but now the recomendation is the other way round. 
(prettier-eslint is not recommended anymore)

## Aims

* Extendable Rules based on airbnb
* Code quality fixed with TS/ESLint
* Format errors fixed with Prettier (conflicting format rules in the linter are disabled)
* Lints + Fixes for html script tags
* Lints + Fixes for React via eslint-config-airbnb
* [rules here](https://github.com/jawford/eslint-config-jawford/blob/master/.eslintrc.js)

## Outline Steps
Add an extension to the linting tool to format files with Prettier, ```eslint-plugin-prettier```. 
There's only a single command to process a file.

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

## This project's dependencies
Must get these packages into the end project hence use of dependencies not devDependencies

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

# Usage

## Installations on Target System
* eslint globally and/or locally per project.
* this package locally per project will allow project specific settings.
* this package globally, along with eslint, for projects or rogue JS files without their own specific setup.

## Local Installation
[eslint rules here](https://eslint.org/docs/rules/) see below`"rules"`.  
[prettier rules here](https://prettier.io/docs/en/options.html) see below `"prettier/prettier"`.

1. Install this package's peer dependencies into devDependencies, as well as the config itself:  
`npm i -D eslint-config-jawford`

2. Check node_modules contains dependencies, above

3. Create `.eslintrc`, at root next to package.json

4. Adding another Prettier rule in a local project's `.eslintrc` (e.g. useTabs below) 
will override the defaults from this package, so copy those over too.

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
				"useTabs": true,
				"trailingComma": "es5",
				"singleQuote": true,
				"printWidth": 120
			}
		]
	}
}```

Alternatively add this object directly in `package.json` under the property `"eslintConfig":`

```json
{
  "eslintConfig": {
    "extends": [
      "jawford"
    ]
  }
}
```

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

# VS Code Settings

To lint and fix on file save

1. Install the [Dirk Baeumer's ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
2. Add preference settings to make eslint run **on file save**  
By default VS Code tries to beautify code with its own built-in beautifier

* auto beautify on save? yes, but...  
...turn off the built-in version within JS & JSX language  
...and run the ESLint extension via autoFixOnSave  
If the VS Code prettier extension is used & enabled for other languages like CSS and HTML, 
then turn it off for JS since it will run via Eslint already. If not installed the last setting has no effect / is not recognised.

```json
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

1. Install `npm i -D eslint-config-jawford`

1. Edit `package.json` and replace `"extends": "react-app"` with `"extends": "jawford"`

# Something Wrong?

* Check versions of the installed packages and their compatibility

* A legacy package, `eslint-prettier`, is not working with ESLint version 6

* Try a re-install globally and locally

* Conflicts between Prettier and ESLint occur without the right ESLint or TSLint rules set, as explained in the 
[Prettier documentation](https://prettier.io/docs/en/integrating-with-linters.html).

Locally remove `package-lock.json` and `node_modules`

Globally
```bash
npm remove --global eslint-config-jawford babel-eslint eslint eslint-config-prettier eslint-config-airbnb eslint-plugin-html eslint-plugin-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react prettier eslint-plugin-react-hooks
```

# Resources

### [Integrating Prettier with linters](https://prettier.io/docs/en/integrating-with-linters.html)

### Prettier-VSCode 
specific settings for extension [Prettier-VSCode](https://github.com/prettier/prettier-vscode)

* `prettier.requireConfig` (default: false)  
Require a 'prettierconfig' to format

* `prettier.ignorePath` (default: .prettierignore)  
Supply the path to an ignore file such as .gitignore or .prettierignore. Files which match will not be formatted. Set to null to not read ignore files. Restart required.

* `prettier.disableLanguages` (default: ["vue"])  
A list of languages IDs to disable this extension on. Restart required. Note: Disabling a language enabled in a parent folder will prevent formatting instead of letting any other formatter to run
