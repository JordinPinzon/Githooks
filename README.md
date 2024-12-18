# GitHooks Project

This project demonstrates the use of Git hooks, specifically leveraging Husky and lint-staged to enforce coding standards and automate checks before committing changes. It includes a pre-commit hook that runs ESLint on staged files to ensure code quality.

## Features
- Automatically runs ESLint on staged files during the pre-commit phase.
- Integrates Jest for unit testing.
- Enforces consistent coding practices using ESLint.

## Prerequisites
Before setting up the project, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (version 14 or higher)
- [Git](https://git-scm.com/)

## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/JordinPinzon/Githooks.git
   cd Githooks
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

## Project Setup
1. Install Husky for managing Git hooks:
   ```bash
   npm install husky --save-dev
   ```

2. Set up Husky:
   ```bash
   npx husky install
   ```

3. Add a pre-commit hook:
   ```bash
   npx husky add .husky/pre-commit "npx lint-staged"
   ```

4. Install lint-staged for running ESLint on staged files:
   ```bash
   npm install lint-staged --save-dev
   ```

5. Configure lint-staged by adding the following to your `package.json`:
   ```json
   "lint-staged": {
     "**/*.{js,jsx}": [
       "eslint"
     ]
   }
   ```

6. Install Jest and ESLint plugins:
   ```bash
   npm install jest eslint eslint-plugin-jest @eslint/js eslint-plugin-lint-staged --save-dev
   ```

7. Create an `eslint.config.mjs` file with the following content:
   ```javascript
   import globals from "globals";
   import pluginJs from "@eslint/js";
   import pluginJest from "eslint-plugin-jest";

   /** @type {import('eslint').Linter.FlatConfig[]} */
   export default [
     {
       files: ["**/*.js"],
       languageOptions: {
         sourceType: "commonjs",
         globals: {
           ...globals.browser,
           ...globals.jest,
         },
       },
     },
     pluginJs.configs.recommended,
     {
       plugins: {
         jest: pluginJest,
       },
       rules: {
         ...pluginJest.configs.recommended.rules,
       },
     },
   ];
   ```

## Usage
1. Add your JavaScript files and tests to the project.
2. Stage your changes using:
   ```bash
   git add .
   ```
3. Commit your changes. If there are ESLint errors, the commit will be blocked until they are resolved:
   ```bash
   git commit -m "Your commit message"
   ```

## Example
An example test file (`__test__/index.test.js`) might look like this:
```javascript
const { add } = require('../index');

describe('test index', () => {
    test('add', () => {
        expect(add(1, 2)).toBe(3);
    });
});
```

## Troubleshooting
- If you encounter ESLint errors related to Jest globals, ensure your `eslint.config.mjs` includes the Jest configuration as shown above.
- Run ESLint manually to debug issues:
  ```bash
  npx eslint <file_path>
  ```

