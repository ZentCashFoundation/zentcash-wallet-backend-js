![Zent Cash](https://raw.githubusercontent.com/ZentCashFoundation/brand/master/logo/wordmark/zentcash_wordmark_color.png)

[![NPM](https://nodei.co/npm/zentcash-wallet-backend.png?compact=true)](https://npmjs.org/package/zentcash-wallet-backend)

![Prerequisite](https://img.shields.io/badge/node-%3E%3D8-blue.svg) [![Documentation](https://img.shields.io/badge/documentation-yes-brightgreen.svg)](https://zent.cash/zentcash-wallet-backend-js) [![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/ZentCashFoundation/zentcash-wallet-backend-js/graphs/commit-activity) [![License: GPL-3.0](https://img.shields.io/badge/License-GPL--3.0-yellow.svg)](https://github.com/ZentCashFoundation/zentcash-wallet-backend-js/blob/main/LICENSE)

# Zent Cash Wallet Backend

Provides an interface to the Zent Cash network, allowing wallet applications to be built.

* Downloads blocks from the network, either through a traditional daemon, or a blockchain cache for increased speed
* Processes blocks, decrypting transactions that belong to the user
* Sends and receives transactions

## Installation

NPM:

`npm install zentcash-wallet-backend --save`

Yarn:

`yarn add zentcash-wallet-backend`

## Documentation

[You can view the documentation here](https://zentcashfoundation.github.io/zentcash-wallet-backend-js/classes/_walletbackend_.walletbackend.html)

You can see a list of all the other classes on the right side of the screen.
Note that you will need to prefix them all with `WB.` to access them, if you are not using typescript style imports, assuming you imported with `const WB = require('zentcash-wallet-backend')`.

## Quick Start

You can find an [example project in the examples](https://github.com/ZentCashFoundation/zentcash-wallet-backend-js/tree/main/examples/example1) folder.

### Javascript

```javascript
const WB = require('zentcash-wallet-backend');

(async () => {
    const daemon = new WB.Daemon('127.0.0.1', 21698);
    /* OR
    const daemon = new WB.Daemon('seedpro2.zent.cash', 21698);
    */
    
    const wallet = WB.WalletBackend.createWallet(daemon);

    console.log('Created wallet');

    await wallet.start();

    console.log('Started wallet');

    wallet.saveWalletToFile('mywallet.wallet', 'hunter2');

    /* Make sure to call stop to let the node process exit */
    wallet.stop();
})().catch(err => {
    console.log('Caught promise rejection: ' + err);
});
```

### Typescript

```typescript
import { WalletBackend, Daemon, IDaemon } from 'zentcash-wallet-backend';

(async () => {
    const daemon: IDaemon = new Daemon('127.0.0.1', 21698);

    /* OR
    const daemon: IDaemon = new Daemon('seedpro2.zent.cash', 21698);
    */

    const wallet: WalletBackend = WalletBackend.createWallet(daemon);

    console.log('Created wallet');

    await wallet.start();

    console.log('Started wallet');

    wallet.saveWalletToFile('mywallet.wallet', 'hunter2');

    /* Make sure to call stop to let the node process exit */
    wallet.stop();
})().catch(err => {
    console.log('Caught promise rejection: ' + err);
});
```

## Configuration

There are a few features which you may wish to configure that are worth mentioning.

### Auto Optimize

Auto optimization is enabled by default. This makes the wallet automatically send fusion transactions when needed to keep the wallet permanently optimized.

To enable/disable this feature, use the following code:

```javascript
wallet.enableAutoOptimization(false); // disables auto optimization
```

### Coinbase Transaction Scanning

By default, coinbase transactions are not scanned.
This is due to the majority of people not having solo mined any blocks.

If you wish to enable coinbase transaction scanning, run this line of code:

```javascript
wallet.scanCoinbaseTransactions(true)
```

### Logging

By default, the logger is disabled. You can enable it like so:

```javascript
wallet.setLogLevel(WB.LogLevel.DEBUG);
```

and in typescript:

```typescript
wallet.setLogLevel(LogLevel.DEBUG);
```

The logger uses console.log, i.e. it outputs to stdout.

If you want to change this, or want more control over what messages are logged,
you can provide a callback for the logger to call.

```javascript
wallet.setLoggerCallback((prettyMessage, message, level, categories) => {
    if (categories.includes(WB.LogCategory.SYNC)) {
        console.log(prettyMessage);
    }
});
```

and in typescript:

```typescript
wallet.setLoggerCallback((prettyMessage, message, level, categories) => {
    if (categories.includes(LogCategory.SYNC)) {
        console.log(prettyMessage);
    }
});
```

In this example, we only print messages that fall into the SYNC category.

You can view available categories and log levels in the documentation.

## Contributing

### Building (For Developers)

`git clone https://github.com/ZentCashFoundation/zentcash-wallet-backend-js.git`

`cd zentcash-wallet-backend`

`npm install -g yarn` (Skip this if you already have yarn installed)

`yarn build`

Generated javascript files will be written to the dist/lib/ folder.

### Running tests

`yarn test` - This will run the basic tests

`yarn test-all` - This will run all tests, including performance tests.

### Before making a PR

* Ensure you are editing the TypeScript code, and not the JavaScript code (You should be in the `lib/` folder)
* Ensure you have built the JavaScript code from the TypeScript code: `yarn build`
* Ensure you have updated the documentation if necessary - Documentation is generated from inline comments, jsdoc style.
* Ensure you have rebuilt the documentation, if you have changed it: `yarn docs`
* Ensure the tests all still pass: `yarn test`, or `yarn test-all` if you have a local daemon running.
* Ensure your code adheres to the style requirements: `yarn style`

You can try running `yarn style --fix` to automatically fix issues.