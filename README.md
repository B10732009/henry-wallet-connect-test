# Wallet Connect Test #
 
Trying to integrate an react.js app with wallet-connect module into a express.js app.

## Folders ##

- express/ : a express.js app generated by [NPM Express Generator](https://www.npmjs.com/package/express-generator). 
- react/ : a react.js demo app for [CERTHIS-WALLET](https://github.com/certhis/CERTHIS-WALLET) from a [Demo App](https://codesandbox.io/s/certhis-wallet-react-lhuddn)). 

Remember to `cd` to each folder and download modules first.

```sh
npm install
```

## Notes ##

### React Demo App Bugs ###

The source code of [CERTHIS-WALLET Demo app](https://codesandbox.io/s/certhis-wallet-react-lhuddn) seems to have some bugs.

In the `package.json` , change the version of `react` and `react-dom` to __*18.2.0*__.

```json
"react": "18.2.0",
"react-dom": "18.2.0",
```

In the `src/App.js` , fix the syntax error.

- put all `import` to the top of file.
  
  ```js
  import CoinbaseWalletSDK from "@coinbase/wallet-sdk";
  import WalletConnectProvider from "@walletconnect/web3-provider";
  import { ethers } from "ethers";
  ```
- put a `var` or `let` in front of `CerthisWallet`.

  ```js
  let CerthisWallet = CerthisWalletLib.init(
      Web3,
      CoinbaseWalletSDK,
      WalletConnectProvider
  );
  ```
 
 - in the function `handleLogout()` , the function `reload()` seems to not work, so I just comment out it.
 
   ```js
   const handleLogout = async () => {
      await CerthisWallet.disconnect();
      window.provider = null;
      setIsConnected(false);
      setAddress("");
      setBalance("");
      setNetwork("");
      // location.reload();
    };
   ```
### Integration ###

- `react/build.sh` will package the react app, convert `index.html` to `index.jade` , and copy the package to `express/`.

  At the directory `root/react/` , run
  ```sh
  ./build.sh
  ```
  
- in the `express/app.js` , remember to set the package path.

  ```js
  app.use(express.static('react_build')); // set the path of react app package 
  ```

- html2jade converter (this has been done in `react/build.sh`)

  If the module is not installed globally, it seems that we need to find the program by ourself.
  
  At directory `root/react/` , run
  ```sh
  ./node_modules/html2jade/cli.js filename.html
  ```

## Links ##

- [LdapDapp(main system)](https://github.com/jenhao-thesis/LdapDapp)
- [WalletConnect](https://walletconnect.com/)
- [WalletConnect Example Dapp](https://github.com/WalletConnect/walletconnect-example-dapp) 
- [CERTHIS-WALLET](https://github.com/certhis/CERTHIS-WALLET)
- [CERTHIS-WALLET Demo app](https://codesandbox.io/s/certhis-wallet-react-lhuddn)
- [HTML to JADE Converter Online](https://codebeautify.org/html-to-jade-converter)
- [html2jade NPM Module](https://www.npmjs.com/package/html2jade)
- [Update Node.js Version](https://www.freecodecamp.org/news/how-to-install-node-js-on-ubuntu-and-update-npm-to-the-latest-version/)

