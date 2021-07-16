# docusaurus-twoslash-bug

This repo reproduces a bug in the `docusaurus-preset-shiki-twoslash` package when bundling to production.

## Steps

### Development

- yarn install
- yarn start
- Navigate to the tutorial page
- Verify that the Copy button works

### Production

- yarn build
- yarn serve
- Navigate to the tutorial page
- Verify that the Copy button doesn't work

## Analysis

It looks like this is caused by the null operator on the return of `querySelectorAll`.

After bundling the app, both the array and the element in the loop are named `e`, so the loop looks at the array instead, can't find `.textContent` and therefore nothing is copied.

![Screenshot of debugger showing the described behaviour](/.github/images/debugger.png)

Replacing with `Array.from`, it works as expected in production. Since `querySelectorAll` always returns a `NodeList`, we don't need for nullability.
