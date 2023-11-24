# TRC20
TRC20 base on tron blockchain writing the string into the memo field of the transaction to achieve this.

## Token Economic
 - Token: TRXS
 - Supply: 21000000000
 - limit: 1000

**The token serves no practical purpose.It's merely intended to demonstrate the theory.**

## Method
 - deploy: `data:,{"p":"trc-20","op":"deploy","tick":"trxs","max":"21000000000","lim":"1000"}`
 - mint: `data:,{"p":"trc-20","op":"mint","tick":"trxs","amt":"1000"}`
 - transfer: TBA

## Mint TRXS use nodejs
```
const TronWeb = require('tronweb');
const HttpProvider = TronWeb.providers.HttpProvider;
const fullNode = new HttpProvider("https://api.trongrid.io");
const solidityNode = new HttpProvider("https://api.trongrid.io");
const eventServer = new HttpProvider("https://api.trongrid.io");
const privateKey = "your privateKey"; //
const tronWeb = new TronWeb(fullNode, solidityNode, eventServer, privateKey);

const CONTRACT = "your tron address";  //

const memo = 'data:,{"p":"trc-20","op":"mint","tick":"trxs","amt":"1000"}';

async function main() {

    const unSignedTxn = await tronWeb.transactionBuilder.sendTrx(CONTRACT, 1000);
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


## Indexer
 - Recording the block number of deploy inscription.
 - Scanning blocks from the block number of deployed inscription onwards, iterating through each block's transactions and matching the data information returned by txid.
 - Use the FULL NODE HTTP API to query txid get the `from` and `to` addresses. For more details, please refer to the following link: https://developers.tron.network/reference/wallet-gettransactionbyid.
 - The obtained addresses need to undergo encoding conversion to Tron wallet addresses. For more details, please refer to the following link: https://www.btcschools.net/tron/tron_tool_base58check_hex.php.





