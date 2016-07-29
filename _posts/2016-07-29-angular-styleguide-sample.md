---
layout: post
title: Angular 1.x styleguide Sample App
---

I've shared a very minimalistic application that follows Architecture, file structure, components, one-way dataflow and best practices suggested by [@toddmotto](//twitter.com/toddmotto) on this repo [angular-styleguide](//github.com/toddmotto/angular-styleguide).

[Angular 1.x Style Guide Sample Application](https://github.com/chihab/angular-styleguide-sample)

The app is really really simple, actually it is just the code on the styleguide with the needed dependencies to build it.

It is a good companion to reading the Style Guide.

## Install

```

git clone https://github.com/chihab/angular-styleguide-sample
cd angular-styleguide-sample
npm install

```


## Build


```
npm run build
```

When you do a `npm run X`, npm will search for a `X` key inside `scripts` property on `package.json` and simply run the command defined as value of property `X`

```
{
  "name": "angular-styleguide-sample",
  "version": "0.1.0",
  "description": "Angular 1.x styleguide (ES2015) Sample App",
  "main": "index.js",

  "scripts": {
    "start": "node_modules/webpack-dev-server/bin/webpack-dev-server.js",
    "build": "node_modules/webpack/bin/webpack.js",
    "test": " [ -f dist/bundle.js ] "
}
```

`npm start` and `npm test` are predefined shortcuts for `npm run start` and `npm run test` respectively.

`npm build` would not work.

`npm run build` launches `node node_modules/webpack/bin/webpack.js`


### Webpack


webpack.js is an executable file (with a node hashbag) that does the module bundling stuff.

When installed globally using `npm -g`, a symbolic link (webpack) that points to node_modules/webpack/bin/webpack.js is installed on your PATH, so you would just run `webpack`.

Webpack (with the loader configuration we specified) basically converts all the es6 import statements to something our super es5 browsers could understand. He does the `concat` and the `minify` for us and resolves dependencies in an optimal way.

The resulting `bundle.js` file is stored in dist folder.

This is the only file you should specify on your index.html.


## Run


```
npm start
```

`npm start` runs webpack-dev-server (a separate NPM package), which is a little node.js Express server with automatic refresh.
The webpack server, serves your files, watches them and automatically launches the build when a file has been modified.

Il serves by default on `http://localhost:8080/``


## Application Bootstrap


The entry point of the sample app is `index.js`

```
import App from './app';
angular.bootstrap(document, [App]);
```

`document` is our body and `App` is our App component name. Notice the ending `.name` on component definition (this is what is actually exported).

You can see it as:

* The first instruction result as the string "app"
* The second as ```angular.bootstrap(document, ["app"]);```

This is how all component are linked via import and export statements.

Hope you'll find this post useful.
