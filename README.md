# Thundra Lambda Node.js Warmup Wrapper

Warm up your lambda and reduce cold starts.

## Installation
```bash
npm install @thundra/warmup --save
```

## Usage

There are three steps to warm up your lambdas.

If you are using [Thundra](https://github.com/thundra-io/thundra-lambda-agent-nodejs) to monitor your lambdas, you can skip step 2.

### 1. Setup our warmup lambda
You can setup `thundra-lambda-warmup` [manually](https://thundra.readme.io/docs/warmup-manual-setup) or [using Serverless Framework](https://thundra.readme.io/docs/warmup-serverless-framework).

Check out our [docs](https://thundra.readme.io/docs/how-to-warmup) for more information.

Manual setup might sound harder but it is actually more straightforward.

### 2. Wrap your lambda using this module

```js
const thundraWarmup = require("@thundra/warmup");

const thundraWarmupWrapper = thundraWarmup();

exports.handler = thundraWarmupWrapper((event, context, callback) => {
    callback(null, "No more cold starts!");
});
```

You can also pass an optional callback function which will be called on warmup requests.

```js
const thundraWarmup = require("@thundra/warmup");

const optionalCallback = () => console.log(Warming up...);

const thundraWarmupWrapper = thundraWarmup(optionalCallback);

exports.handler = thundraWarmupWrapper((event, context, callback) => {
    callback(null, "No more cold starts!");
});
```

`context.succeed`, `context.fail`, `context.done` are also supported.

```js
const thundraWarmup = require("@thundra/warmup");

const thundraWarmupWrapper = thundraWarmup();

exports.handler = thundraWarmupWrapper((event, context) => {
    context.succeed("No more cold starts!");
});
```
### 3. Enable warming up for your lambda
There are two ways to enable warming up for a specific lambda:
* set `thundra_lambda_warmup_warmupAware` environment variable `true` in your lambda or
* add your lambda's name to `thundra-lambda-warmup`'s `thundra_lambda_warmup_function` environment variable.

For more detailed information, check out [warmup configuration](https://thundra.readme.io/docs/warmup-configuration).