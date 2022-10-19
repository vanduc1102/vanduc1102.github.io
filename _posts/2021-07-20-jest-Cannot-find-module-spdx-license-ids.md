---
layout: post
title: Cannot find module spdx-license-ids from .../spdx-expression-parse/scan.js
date: 2021-07-20 16:43:00 +0700
categories: [NodeJs]
tags: [NodeJs, Jest, SwaggerUI]
---

## Error

```
Test server › should return OK on /health

    Cannot find module 'spdx-license-ids' from '../../node_modules/spdx-expression-parse/scan.js'

    Require stack:
      /Users/duc.nguyen/Workspace/sample-project/node_modules/spdx-expression-parse/scan.js
      /Users/duc.nguyen/Workspace/sample-project/node_modules/spdx-expression-parse/index.js
      /Users/duc.nguyen/Workspace/sample-project/node_modules/validate-npm-package-license/index.js
      /Users/duc.nguyen/Workspace/sample-project/node_modules/normalize-package-data/lib/fixer.js
      /Users/duc.nguyen/Workspace/sample-project/node_modules/normalize-package-data/lib/normalize.js
      /Users/duc.nguyen/Workspace/sample-project/node_modules/read-pkg-up/node_modules/read-pkg/index.js
      /Users/duc.nguyen/Workspace/sample-project/node_modules/read-pkg-up/index.js
      /Users/duc.nguyen/Workspace/sample-project/node_modules/koa2-swagger-ui/dist/index.js
      src/server.ts
      src/server.test.ts

      at Resolver.resolveModule (../../node_modules/jest-resolve/build/resolver.js:311:11)
      at Object.<anonymous> (../../node_modules/spdx-expression-parse/scan.js:4:11)
```

## How to fix

Current `jest.config.json`

```js
module.exports = {
	//...rest
	moduleFileExtensions: ["ts", "js", "html"],
};
```

We will need to add `json`

```js
module.exports = {
	//...rest
	moduleFileExtensions: ["ts", "js", "html", "json"],
};
```

## Reason

The package that failed this time is [spdx-license-ids](https://github.com/jslicense/spdx-license-ids), but the json file is used internally.
jest will only read files with the extension specified in [moduleFileExtensions in config](https://jestjs.io/docs/en/configuration#modulefileextensions-arraystring).
So It couldn't read the json file used by `spdx-license-ids` and got an error.

By the way, the initial value of moduleFileExtensions of jest is

`["js", "json", "jsx", "ts", "tsx", "node"]`

So this error wouldn't have happened if I hadn't made any extra settings 😇

Source: [https://qiita.com/nekoshita_yuki/items/b20b76cad37a6c9748b8](https://qiita.com/nekoshita_yuki/items/b20b76cad37a6c9748b8)
