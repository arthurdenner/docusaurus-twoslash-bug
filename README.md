# docusaurus-twoslash-bug

This repo reproduces a bug in the `docusaurus-preset-shiki-twoslash` package when bundling to production.

## Steps

### Development

- `yarn install`
- `yarn start`
- Navigate to the tutorial page
- Verify that the Copy button works

### Production

- `yarn build`
- `yarn serve`
- Navigate to the tutorial page
- Verify that the Copy button doesn't work

## Solution

It looks like this is caused by the "cast" of `querySelectorAll` to an array using `[...]`.

After bundling the app, there is no cast. Therefore, the loop looks at the array `e`, can't find `.textContent` and nothing is copied.

![Screenshot of debugger showing the described behaviour](/.github/images/debugger.png)

Replacing with `Array.from`, it works as expected. And since `querySelectorAll` always returns a `NodeList`, we don't need for nullability.

![Screenshot of debugger showing the described solution](/.github/images/debugger-solution.png)

### Steps

- Open the twoslash repo in my branch ([link](https://github.com/arthurdenner/twoslash/tree/fix/copy-button))
- `yarn build` inside `twoslash/packages/docusaurus-preset-shiki-twoslash`
- `yarn link`
- Switch to this project
- `yarn remove docusaurus-preset-shiki-twoslash`
- `yarn link "docusaurus-preset-shiki-twoslash"`
- `yarn build`
- `yarn serve`
