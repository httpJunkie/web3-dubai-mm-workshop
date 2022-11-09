Loosely follow truffle tutorial until Ganache section:

We want to ensure that ganache cli is installed:
```bash
 npm i ganache -g
 ```

We can run a local version of Ganache rather than the GUI right in our terminal using:

```bash
ganache
```

Running truffle migrate will target the development config:

```bash
truffle migrate
```

## Test Your Smart Contracts

In order to quickly test our smart contracts, we'll take advantage of `truffle exec` to run scripts to automate common tasks. Let's write a script that will execute all of our different functions. First, create a new file under a new `scripts` folder called `run.js`.

```js
var BoredPetsNFT = artifacts.require("BoredPetsNFT");
var Marketplace = artifacts.require("Marketplace");

async function logNftLists(marketplace) {
    let listedNfts = await marketplace.getListedNfts.call()
    const accountAddress = 'FIRST_ACCOUNT_ADDRESS'
    let myNfts = await marketplace.getMyNfts.call({from: accountAddress})
    let myListedNfts = await marketplace.getMyListedNfts.call({from: accountAddress})
    console.log(`listedNfts: ${listedNfts.length}`)
    console.log(`myNfts: ${myNfts.length}`)
    console.log(`myListedNfts ${myListedNfts.length}\n`)
}

const main = async (cb) => {
  try {
    const boredPets = await BoredPetsNFT.deployed()
    const marketplace = await Marketplace.deployed()

    console.log('MINT AND LIST 3 NFTs')
    let listingFee = await marketplace.getListingFee()
    listingFee = listingFee.toString()
    let txn1 = await boredPets.mint("URI1")
    let tokenId1 = txn1.logs[2].args[0].toNumber()
    await marketplace.listNft(boredPets.address, tokenId1, 1, {value: listingFee})
    console.log(`Minted and listed ${tokenId1}`)
    let txn2 = await boredPets.mint("URI1")
    let tokenId2 = txn2.logs[2].args[0].toNumber()
    await marketplace.listNft(boredPets.address, tokenId2, 1, {value: listingFee})
    console.log(`Minted and listed ${tokenId2}`)
    let txn3 = await boredPets.mint("URI1")
    let tokenId3 = txn3.logs[2].args[0].toNumber()
    await marketplace.listNft(boredPets.address, tokenId3, 1, {value: listingFee})
    console.log(`Minted and listed ${tokenId3}`)
    await logNftLists(marketplace)

    console.log('BUY 2 NFTs')
    await marketplace.buyNft(boredPets.address, tokenId1, {value: 1})
    await marketplace.buyNft(boredPets.address, tokenId2, {value: 1})
    await logNftLists(marketplace)

    console.log('RESELL 1 NFT')
    await marketplace.resellNft(boredPets.address, tokenId2, 1, {value: listingFee})
    await logNftLists(marketplace)

  } catch(err) {
    console.log('Doh! ', err);
  }
  cb();
}

module.exports = main;
```

From you ganache local instance we have running you can find the first test wallet/account that it set up for us and copy that address into the script above for `FIRST_ACCOUNT_ADDRESS`

```bash
Starting RPC server

Available Accounts
==================
(0) 0xa7C7Fd0dc2D327CaDA6e3Ef4a622750422F9D2fD (1000 ETH)
...
```

NFT_EVENT_API

getTimeUntilEventById(3)

{
  eventName: "ETH Atlantis",
  eventYear: "2022",
  soldOut: false,
  cancelled: false,
  postPoned: false,
  eventDate: "16.11.2022",
  eventTime: "03:00pm",
  eventMilTime: "15:00",
  untilEvent: {
    Days: 5
    hours: 5,
    minutes: 6, 
  }
}

## MetaMask SDK

The MetaMask SDK enables developers to connect their dapps with any MetaMask wallet (Extension or Mobile).

It is a library we can install with components to guide users to easily connect with a MetaMask wallet client. For dApps running on a desktop browser, the SDK will check if the Extension is installed and if not it will prompt the user to install it or to connect via QR code with their MetaMask Mobile wallet. Another example, for native mobile applications, the SDK will automatically deeplink into MetaMask Mobile wallet to make the connection.