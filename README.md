# TRC20
TRC20 base on TRON blockchain writing the string into the memo field of the transaction to achieve this.

## Token Economic
 - Token: TRXS
 - Supply: 21000000000
 - limit: 1000

**The token serves no practical purpose.It's merely intended to demonstrate the theory.**

## Method
 - deploy: `data:,{"p":"trc-20","op":"deploy","tick":"trxs","max":"21000000000","lim":"1000"}`
 - mint: `data:,{"p":"trc-20","op":"mint","tick":"trxs","amt":"1000"}`
 - transfer: TBA

## Deploy txid
https://tronscan.org/#/transaction/c9fa38e8af788c3d1694fae2f0d6440d8e3515e5ec1f31f0874045ea8c9c7c94


## Mint TRXS use nodejs
```
const TronWeb = require('tronweb');
const HttpProvider = TronWeb.providers.HttpProvider;
const fullNode = new HttpProvider("https://api.trongrid.io");
const solidityNode = new HttpProvider("https://api.trongrid.io");
const eventServer = new HttpProvider("https://api.trongrid.io");
const privateKey = "your privateKey"; //
const tronWeb = new TronWeb(fullNode, solidityNode, eventServer, privateKey);

const blackHole = "T9yD14Nj9j7xAB4dbGeiX9h8unkKHxuWwb";  //black hole address

const memo = 'data:,{"p":"trc-20","op":"mint","tick":"trxs","amt":"1"}';  //0.000001 TRX is the minimum transfer amount.

async function main() {

    const unSignedTxn = await tronWeb.transactionBuilder.sendTrx(blackHole, 1);
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
 - Scanning blocks from the block number of deployed inscription onwards, iterating through each block's transactions and matching the `data` information returned by txid.
 - Use the FULL NODE HTTP API to query txid get the `from` and `to` addresses. For more details, please refer to the following link: https://developers.tron.network/reference/wallet-gettransactionbyid.
 - The obtained addresses need to undergo encoding conversion to Tron wallet addresses. For more details, please refer to the following link: https://www.btcschools.net/tron/tron_tool_base58check_hex.php.


## FAQ
### Why you need transfer to `T9yD14Nj9j7xAB4dbGeiX9h8unkKHxuWwb` account?
Because TRON blockchain cannot transfer to yourself address.We decided to transfer the black hole address to the TRON blockchain. Refer to the black hole address given in the official [documentation](https://developers.tron.network/docs/faq#3-what-is-the-destruction-address-of-tron)https://developers.tron.network/docs/faq#3-what-is-the-destruction-address-of-tron of the TRON blockchain. 

### Why you need transfer to 0.000001 TRX to black hole address?
Because TRON blockchain cannot transfer zero amount, 0.000001 TRX is the minimum transfer amount.





