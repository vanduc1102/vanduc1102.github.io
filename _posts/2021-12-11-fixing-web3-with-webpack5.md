---
layout: post
title:  Fixing issues of Web3 with Webpack@5
date:   2021-12-11 00:14:27 +0700
categories: [react]
tags: [reactjs, typescript,webpack, web3]
---

I am building ConKetKemon game, it is a demo of building a game on blockchain.

My project uses web3 lib:

`yarn add web3 @web3-react/core @web3-react/injected-connector`

Since Webpack@5 released, they removed NodeJS Polyfill

Here is how to fix the issue:

`webpack.config.js`

```js
const nrwlConfig = require('@nrwl/react/plugins/webpack.js');
const NodePolyfillPlugin = require('node-polyfill-webpack-plugin');

module.exports = (config, context) => {
  nrwlConfig(config);
  return {
    ...config,
    node: {
      global: true,
    },
    resolve: {
      ...config.resolve,
      fallback: {
        ...config.resolve.fallback,
        http: require.resolve('stream-http'),
      },
    },
    plugins: [...config.plugins, new NodePolyfillPlugin()],
  };
};
```
Noticed I am using [NX@13](https://nx.dev/) for creating the workspace

And we need to install `stream-http`

`yarn add stream-http -D`

In order to use the `webpack.config.js`, we have to change the default of NX setup to

```
-	"webpackConfig": "@nrwl/react/plugins/webpack"
+	"webpackConfig": "apps/frontend/webpack.config.js"
```

Repository on github: <https://github.com/coinconket/conketkemon>
