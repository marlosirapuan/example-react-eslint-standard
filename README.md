# Project example in React with eslint/standardjs/babel-plugin-root-import on VScode

How to configure VSCode to work with eslint, standardjs and babel-plugin-root-import and react-app-rewired.

### Requisites
```
yarn 1.16.0+
node 10.13+
create-react-app
```

To install **create-react-app cli**:
```
yarn global add create-react-app
```

### Essential Extensions

* [Prettier-Standard](https://marketplace.visualstudio.com/items?itemName=numso.prettier-standard-vscode)
* [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

### Useful Extensions

* [indent-rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)
* [Import Cost](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost)
* [Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer)


### Steps

```
yarn add eslint -D
yarn eslint --init

[x] To check syntax, find problems, and enforce code style
[x] JavaScript modules (import/export)
[x] React
[x] Browser
[x] Use a popular style guide
[x] Standard
[x] JavaScript
```

Confirm with `"Y"`

Now, remove `package-lock.json` and run `yarn install` again.

Install these libs as devDependecies:
```
yarn add eslint-plugin-react-hooks eslint-plugin-jsx-a11y eslint-import-resolver-babel-plugin-root-import babel-plugin-root-import customize-cra react-app-rewired -D
```

Create `config-overrides.js` file on root project and add this code:

```
const { override, addBabelPlugins } = require('customize-cra')

module.exports = override(
  ...addBabelPlugins([
    'babel-plugin-root-import',
    {
      rootPathPrefix: '~',
      rootPathSuffix: 'src'
    }
  ])
)
```

Create `jsconfig.json` file on root project and add this code:
```
{
  "compilerOptions": {
    "allowSyntheticDefaultImports": true,
    "baseUrl": "src",
    "paths": {
      "~/*": [
        "./*"
      ]
    }
  }
}
```

Create `.env` file on root project and add:
```
SKIP_PREFLIGHT_CHECK=true
```
This avoids ESLint error:

```
The react-scripts package provided by Create React App requires a dependency:

  "eslint": "^5.16.0"
```

And change `.eslintrc.js` to this:

```
module.exports = {
  env: {
    browser: true,
    jest: true,
    es6: true
  },
  extends: ['standard'],
  globals: {
    Atomics: 'readonly',
    SharedArrayBuffer: 'readonly',
    __DEV__: true
  },
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 2018,
    sourceType: 'module'
  },
  plugins: ['react', 'jsx-a11y', 'import', 'react-hooks'],
  rules: {
    'import/prefer-default-export': 'off',
    'global-require': 'off',
    'no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
    'no-console': ['error', { allow: ['tron'] }],
    'no-param-reassign': 'off',
    'no-underscore-dangle': 'off',
    'no-duplicate-imports': ['error', { includeExports: true }],
    'react/jsx-uses-react': 1,
    'react/jsx-filename-extension': ['error', { extensions: ['.js', '.jsx'] }],
    'react/jsx-one-expression-per-line': 'off',
    'react/prop-types': 'off',
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'warn',
    'react/jsx-uses-react': 'error',
    'react/jsx-uses-vars': 'error'
  },
  settings: {
    'import/resolver': {
      'babel-plugin-root-import': {
        rootPathSuffix: 'src'
      }
    }
  }
}
```

Change `scripts` on `package.json` to this (**start**, **build** and **test** only!):

```
  "scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
  },
```

Save, restart VSCode and run your project `yarn start`. See in the `app.js` file how to import a file.
