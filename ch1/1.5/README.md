> The code challenges in this course build upon each other. It's highly recommended that you start from the beginning. If you haven't already, get started with our [Installation Instructions](https://www.burrrata.ch/ces-website/docs/en/sync/dev-env-setup).

<br />

## Why Decentralization?

We've been exploring blockchain networks and tools this entire chapter, and we're just scratching the surface. The truth is that building secure, usable, and useful payments networks is hard. You know what's not hard though? Building insecure ones! In this chapter we'll explore just how easy it is to introduce sneaky back doors, fees, and other nefarious ~~bugs~~ features into our payment processor.

<br />

## Fault Tolerance

Fault Tolerance is important. It is the degree to which your system will survive errors. While most of the networks we use today are centralized in their decision making, their databases are not centralized at all (e.g. a system with a single database and server)! Many of the systems that we might call "centralized" are architecturally decentralized (e.g. they have multiple databases and servers around the world) and are thus fault tolerant. There's [lots](https://blog.ethereum.org/2014/08/16/secret-sharing-erasure-coding-guide-aspiring-dropbox-decentralizer/) of [ways to do this](https://github.com/ethereum/research/wiki/A-note-on-data-availability-and-erasure-coding), but the main takeaway is that you should always backup your data because the data is literally money.

<br />

## Rent Extraction

Rent extraction is one of the most dangerous properties of centralized systems, especially when they are run by for-profit companies.

Essentially, once a company has a monopoly or something close to one, they can extract rent from all their users. This "rent" could be anything from increasing prices to showing more ads. There's an art to this, and every MBA and finance grad spends years practicing learning how to extract the maximal value from users/customers.

<p align='center'>
	<img src='https://miro.medium.com/max/700/0*7lrwGIDbAYk6q7zG.' />
</p>

To get your feet wet in the wild world of business consulting, you're going to add a fee to your centralized payments operator. From now on all users need to pay a 1 token fee to use the network. Sound familiar? Yup, every crypto exchange does this. When there's healthy competition, this is fine. When a handful (or one) company has a monopoly, then things get weird...

> TODO: Add a network fee to the state of the Paypal class and create a function that charges the sender of each transaction.

```
// the history of transactions
this.txHistory = [];
// the network fee
// TODO

// Charges a fee to use the network
chargeFee(tx) {
	// removes the network fee from the sender's balance
	// TODO
	// adds the network fee to payment operator's balance
	// TODO
}

// Checks if a transaction is valid, then processes it, then checks if there are any valid transactions in the pending transaction pool and processes those too
processTx(tx) {
	// charege a fee to use the network
	// TODO
	// check the transaction is valid
	if (this.checkTx(tx)) {
		// apply the transaction to Paypal's state
		this.applyTransaction(tx);
		// check if any pending transactions are now valid, and if so process them too
		this.processPendingTx()
	}
}
```

<br />

## Censorship

Censorship is one aspect that is inherent to centralized systems.

Most blockchain protocols are designed to be censorship resistant, but most exchanges are not. While Bitcoin, Ethereum, and other blockchains might not be able to censor transactions, but regulated exchanges can stop users from buying or selling cryptocurrencies with fiat money.

> TODO: Initialize an empty array `this.blacklist` in your constructor that stores blacklisted addresses. If we see a transaction that is sending money from a blacklisted address, we should throw an error.

```
// pending transaciton bool
this.pendingTx = [];
// the history of transactions
this.txHistory = [];
// the network fee
// TODO
// blacklist of accounts to be censored
// TODO

// Check if the user's address is on a blacklist. If not, check is the user's address is already in the state, and if not, add the user's address to the state
checkUserAddress(tx) {
	// check if the sender or receiver are on the blacklist
	// TODO
		// if the sender or receiver are banned, return false
		// TODO
```

<br />

## Fraud

When a central payment operator has control over everyone's assets, it can act maliciously.

To simulate fraudulent activity, let's write a method, `stealAllFunds()` in `Paypal.js` which will iterate through every address in `this.state`, and sum up everyone else's balances, also setting all of their balance to 0. It will then take the sum of all of that money and add it to Paypal's balance, effectively stealing everyone's funds.
- this happens _a lot_ in darknet markets, the most recent of which being [Nightmare](https://twitter.com/Patrick_Shortis/status/1156354524459802624) (btw that was name of the market, not the attack).

But what if the central payments operator wanted to steal funds, but continue to operate. Turns out they can have their cake and eat it too! All they have to do is mint more tokens ;) This is far easier for a central operator because they can look like a legit operation on the outside, but really just print money for themselves whenever they wanted. Then they would never be accused of stealing, but they could [steal as much as they wanted](https://medium.com/@bitfinexed) #tether #Bitfinex. Yay magic internet money!

> TODO: add functions to secretly mint tokens or steal funds

```
// Removes funds from user accounts and adds them to Paypal's balance
// - In reality, it would be far easier for Paypal to mint themselves extra cash so they could look like a legit operation on the outside, but really just print money for themselves whenever they wanted. Then they would never be accussed of stealing, but they could steal as much as they wanted. Yay magic internet money!
stealAllFunds() {
	// sums up all the value in the network
	// TODO
			// removes everyone's balance
			// TODO
	// adds that value to Paypal's wallet
	// TODO
}

// Mints funds without adding the transaction to the history
// - In reality, this would not work as corporations have to get audited for tax purposes and whatnot, so they'd have to call this something clever like a network upgrade to invest in R&D or a "restructuring". In either case, there would be a glossy marketing campaign put together to make you feel good about it :)
mintSecretFunds(amount) {
	// it's literally just 1 line of code...
	// TODO
}
```

<br />

## Testing

We recommend building each function, testing it out in `demo.js` to see how it works (`node demo.js`), then you've got that working run the mocha tests to make sure it passes (`mocha`). If you need help our wonderful community is just a [click](https://forum.cryptoeconomics.study) away :)

If all your tests, pass.... congratulations! :)

<br />
