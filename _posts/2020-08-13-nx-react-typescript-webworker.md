---
layout: post
title:  Using ReactJS with WebWorker to compile smart contract
date:   2021-08-13 12:00:00 +0700
categories: [nx, reactjs, webworker, solc, solidity, typescript]
---

![Demo Compiling Solidity Smart Contract in React with WebWorker](/assets/assets/img/2021-08-13-react-webworker-compile-solidity.png)

In this post I gonna compile a [Solidity](https://docs.soliditylang.org/) program with [Solidity Compiler JS](https://github.com/ethereum/solc-js) in [browsers supported WebWorker](https://caniuse.com/webworkers).

Tools and plugins:
```
nx@12.7.1
worker-plugin@5.0.1
solc@0.8.7-fixed
webpack@4 (included in NX)
```

Compiling a smart contract with Solc is a heavy task for browsers, and for such heavy task we need to use [WebWorker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers).

The tool for setting up the project is [NX](https://nx.dev/). it is great tool to create monos-repository.

## Configuring WebWorker for NX

NX is using Webpack underhood, In this post we use [worker-plugin](https://www.npmjs.com/package/worker-plugin) to make WebWorker work with Webpack. In order to config `worker-plugin` we need to extend the default configuration of Webpack inside NX.

1. Create [webpack.json](https://github.com/vanduc1102/coinconket/blob/master/apps/frontend/webpack.config.js) in root folder of `frontend` module.

```js
const WorkerPlugin = require('worker-plugin');
const nrwlConfig = require('@nrwl/react/plugins/webpack.js');

module.exports = (config, context) => {
  nrwlConfig(config);
  return {
    ...config,
    node: {
      Buffer: true,
      module: 'empty',
    },
    plugins: [new WorkerPlugin(), ...config.plugins],
  };
};
```
As you can see we have some polyfill configurations for [NodeJS in Webpack](https://v4.webpack.js.org/configuration/node/)

1. Add `webpack.json` to [workspace.json](https://github.com/vanduc1102/reactjs-practices/blob/main/projects/nx-webworker-sample/workspace.json#L24) file.

```js
"webpackConfig": "apps/frontend/webpack.config.js"
```
Here is the detail of [webpackConfig](https://nx.dev/latest/react/web/build#webpackconfig) option.

## Creating Solidity compiler worker

1. `Solc@0.8.7-fixed` doesn't have typescript declaration yet, we have to add a type declaration manually. Simply add `declare module 'solc/wrapper';` to `*.d.ts` file in your project.

1. Creating a worker [SolcJs.worker.ts](https://github.com/vanduc1102/reactjs-practices/blob/main/projects/nx-webworker-sample/apps/frontend/src/SolcJs.worker.ts) file.
```ts
/* eslint-disable no-restricted-globals */
import * as wrapper from 'solc/wrapper';
const ctx: Worker = self as any;

importScripts(
  'https://solc-bin.ethereum.org/bin/soljson-v0.8.6+commit.11564f7e.js'
);

ctx.addEventListener('message', ({ data }) => {
  const solc = wrapper((ctx as any).Module);
  const compileResult = solc.compile(
    createCompileInput(data.contractFileName, data.content)
  );
  ctx.postMessage(compileResult);
});

function createCompileInput(
  fileName = 'storage.sol',
  fileContent: string
): string {
  const CompileInput = {
    language: 'Solidity',
    sources: {
      [fileName]: {
        content: fileContent,
      },
    },
    settings: {
      outputSelection: {
        '*': {
          '*': ['*'],
        },
      },
    },
  };
  return JSON.stringify(CompileInput);
}
```

2. Create a Promisify function to call `SolcJs.worker.ts` and wait until the compilation finished.
```ts
const compileWithWorker = async (data: any) => {
    return new Promise((resolve, reject) => {
      const worker = new Worker('../../SolcJs.worker.ts', {
        type: 'module',
      });
      worker.postMessage(data);
      worker.onmessage = function (event: any) {
        resolve(event.data);
      };
      worker.onerror = reject;
    });
};
```
 Thank to the great [answer](https://stackoverflow.com/questions/41423905/wait-for-several-web-workers-to-finish) from T.J.Crower.

3. Now we are ready to use WebWorker to compile a Simple Solidity Smart Contract.
```ts
const handleCompile = async () => {
    setCompiling(true);
    const result = await compileWithWorker({
      content: SimpleStorageContact,
    });
    setCompileResult(result as string);
    setCompiling(false);
};
```

SourceCode in Github - [nx-webworker-sample](https://github.com/vanduc1102/reactjs-practices/tree/main/projects/nx-webworker-sample)
