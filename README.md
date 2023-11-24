# TRC20
TRC20 base on TRON blockchain writing the string into the memo field of the transaction to achieve this.

## Token Economic
 - Token: TRXI
 - Supply: 2100000000
 - limit: 1000

## Method
 - deploy: `data:,{"p":"trc-20","op":"deploy","tick":"trxi","max":"2100000000","lim":"1000"}`
 - mint: `data:,{"p":"trc-20","op":"mint","tick":"trxi","amt":"1000"}`
 - transfer: TBA

## Deploy txid
https://tronscan.org/#/transaction/c9fa38e8af788c3d1694fae2f0d6440d8e3515e5ec1f31f0874045ea8c9c7c94

## Mint TRXS with nodejs
1. Install Node.js
2. Create a directory,such as `TRC20Mint`
3. Open `TRC20Mint`, execute command:`npm init`
4. Execute command:`npm install tronweb `
5. Create an index.js file,copy the code below
6. Run index.js:`node index.js` 

```
const TronWeb = require('tronweb');
const HttpProvider = TronWeb.providers.HttpProvider;
const fullNode = new HttpProvider("https://api.trongrid.io");
const solidityNode = new HttpProvider("https://api.trongrid.io");
const eventServer = new HttpProvider("https://api.trongrid.io");
const privateKey = "your privateKey"; //
const tronWeb = new TronWeb(fullNode, solidityNode, eventServer, privateKey);

const blackHole = "T9yD14Nj9j7xAB4dbGeiX9h8unkKHxuWwb";  //black hole address

const memo = 'data:,{"p":"trc-20","op":"mint","tick":"trxs","amt":"1000"}';  

async function main() {

    const unSignedTxn = await tronWeb.transactionBuilder.sendTrx(blackHole, 1); //0.000001 TRX is the minimum transfer amount.
    const unSignedTxnWithNote = await tronWeb.transactionBuilder.addUpdateData(unSignedTxn, memo, 'utf8');
    const signedTxn = await tronWeb.trx.sign(unSignedTxnWithNote);
    console.log("signed =>", signedTxn);
    const ret = await tronWeb.trx.sendRawTransaction(signedTxn);
    console.log("broadcast =>", ret);
}

main().then(() => {

    })
    .catch((err) => {
        console.log("error:", err);
    });
```

## Mint TRXS with TokenPocket Wallet
![dbc85fd41563c2e8328dde5dae67e7b](https://github.com/TRC20org/TRC20/assets/151912398/130e91fc-24cc-45e6-9d78-12fde1659e13)



## Idea for developing indexer
1. Recording the block number of deploy inscription.
2. Get all transactions of black hole address.
3. Use the FULL NODE HTTP API to get the `from`,`to`,`data` field of each txid, match all mint inscriptions. For more details, please refer to the following link: https://developers.tron.network/reference/wallet-gettransactionbyid.
4. The obtained addresses need to undergo encoding conversion to Tron wallet addresses. For more details, please refer to the following link: https://www.btcschools.net/tron/tron_tool_base58check_hex.php.

## Indexer(TBA)


## FAQ
### Why you need transfer to `T9yD14Nj9j7xAB4dbGeiX9h8unkKHxuWwb` account?
Because TRON blockchain cannot transfer to yourself address.We decided transfer to the black hole address of the TRON blockchain. Refer to the black hole address given in the official [documentation](https://developers.tron.network/docs/faq#3-what-is-the-destruction-address-of-tron) of the TRON blockchain. 

### Why you need transfer to 0.000001 TRX to black hole address?
Because TRON blockchain cannot transfer zero amount, 0.000001 TRX is the minimum transfer amount.





