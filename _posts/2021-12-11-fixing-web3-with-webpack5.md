---
layout: post
title: Fixing issues of Web3 with Webpack@5
date: 2021-12-11 00:14:27 +0700
categories: [react]
tags: [reactjs, typescript, webpack, web3]
---

I built the ConKetKemon game as a demonstration of developing a game on the blockchain.

The project uses the following Web3 libraries:

`yarn add web3 @web3-react/core @web3-react/injected-connector`

Starting with Webpack 5, built-in Node.js polyfills were removed.

Here is how to resolve the resulting polyfill issues:

`webpack.config.js`

```js
const nrwlConfig = require("@nrwl/react/plugins/webpack.js");
const NodePolyfillPlugin = require("node-polyfill-webpack-plugin");

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
				http: require.resolve("stream-http"),
			},
		},
		plugins: [...config.plugins, new NodePolyfillPlugin()],
	};
};
```

Note that I am using [NX@13](https://nx.dev/) to manage the workspace.

We also need to install `stream-http`:

`yarn add stream-http -D`

In order to use this custom `webpack.config.js`, update the NX configuration in `workspace.json`:

```
-	"webpackConfig": "@nrwl/react/plugins/webpack"
+	"webpackConfig": "apps/frontend/webpack.config.js"
```

Repository on GitHub: <https://github.com/coinconket/conketkemon>
