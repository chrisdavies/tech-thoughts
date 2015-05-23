# Travis CI is awesome

I just integrated [a project](https://github.com/chrisdavies/rlite) with Travis CI. The experience was trivially easy.

- [Sign up for Travis CI](https://travis-ci.org)
- Sync travis-ci with GitHub and select the repo you want to enable
- Push your project up to GitHub
- Add a `.travis.yml` file to your project (see the examples below)
- Update your `package.json` to have a test in your `scripts` section (see the examples below)
- Once your build is passing, add a nifty badge to your readme
  - Just click the badge in your repo's travis-ci status page
  - In the popup, select markdown
  - Paste the markdown into your readme, and you're good to go!

## Sample .travis.yml file

```yml
language: node_js
node_js:
    - "0.12"
before_script:
    - "npm i -g jasmine-node"
```

## Sample package.json scripts section

```JSON
"scripts": {
  "test": "jasmine-node test/*.spec.js"
},
```
