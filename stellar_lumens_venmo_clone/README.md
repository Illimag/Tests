# Stellar Lumens and Vemno Clone

This is a walkthrough in building a Venmo Clone with Stellar Lumens network for moving transaction on the network.

Stellar Lumens is a distributed blockchain ledger technology. We will be using a feature called

	AnchorX

To maintain customer accounts, we need to create a organization. It is a Stellar company, but it is called an

	anchor

We will do this in 2 ways.

We create a Stellar account on behalf of the customer

We use the memo field of the transaction to operate on behalf of your customer/users.

We will create an anchor maintaining a Stellar account for each customer. 

This tutorial is from this blog

https://blog.abuiles.com/building-your-own-venmo-with-stellar/

This is written in the programming language JavaScript and the mobile wallet is written using React Native.

## Stellar Consensus Protocol

Stellar is built with the Stellar Consensus Protocol (SCP), which is a way to reach consensus without relying on close systems. This maintains the Stellar ledger.

## Stellar Development Foundation

The Stellar Development Foundation is a non-profit corporation for the Stellar Network and the Stellar Protocol.

## Stellar Core and Horizon API

Stellar Core software is in charge of communicating and maintaining the Stellar network. Using the Stellar consensus protocol does validation and agrees on the status of transactions in the network.

Horizon is a RESTful API that allows interaction with Stellar Core and makes it easy to build apps using the network. We can submit transactions, read accounts or subscribe to events in the network.

You can interact with it using any HTTP client like cURL, Postman or Paw. There are also SDKs in different programming languages such as Go, Java, JavaScript, Ruby, etc.

We will be using a test network in JavaScript.

## Account

Stellar uses accounts. We will need to create one before interacting with the network. To create an account we need to deposit Lumens into it.

When you create an account, you get a public and private key. The public key is the equivalent of your bank account number and then the private key is the password. 

## Creating accounts in the test network.

This is how to generate a keypair with the Stellar JS SDK and then ask a service run by the Stellar Development Foundation called a friendbot.

	const StellarSdk = require('stellar-sdk')
	const fetch = require('node-fetch')

	// Generates a keypair and funds account with friendbot
	async function createAccount(){
		const pair = StellarSdk.Keypair.random()
		console.log('Requesting Lumens')

		await fetch(`https://horizon-testnet.stellar.org/friendbot?addr=${pair.publicKey()}`)

		return pair
	}

	async function run(){
		const pair = await createAccount()

		console.log('
			Congrats, you have a Stellar account in the test network!
			seed: ${pair.secret()}
			id: ${pair.publicKey()}
		')

		const url = 'https://horizon-testnet.stellar.org/accounts/${pair.publicKey()}'

		console.log('
			Loading account from test network:
			${url}
		')

		const response = await fetch(url)
		const payload = await response.json()
	}

	run()

## Assets 

Assets are resources with economic value. Cryptocurrencies allow you to represent any kind of asset. We can trade assets on the Stellar network.

Stellar uses a native asset which is called the Lumens represented with the symbol XLM.

Here is a example of an account in JSON format.

	{
	  "id": "GBCFAMVYPJTXHVWRFP7VO6F4QE7B4UHAVJOEG5VR6VEB5M67GHQGEEAB",
	  "account_id": "GBCFAMVYPJTXHVWRFP7VO6F4QE7B4UHAVJOEG5VR6VEB5M67GHQGEEAB",
	  "balances": [
	    {
	      "balance": "0.0000000",
	      "limit": "922337203685.4775807",
	      "asset_type": "credit_alphanum4",
	      "asset_code": "EURT",
	      "asset_issuer": "GAP5LETOV6YIE62YAM56STDANPRDO7ZFDBGSNHJQIYGGKSMOZAHOOS2S"
	    },
	    {
	      "balance": "0.0000000",
	      "limit": "922337203685.4775807",
	      "asset_type": "credit_alphanum4",
	      "asset_code": "ETH",
	      "asset_issuer": "GBDEVU63Y6NTHJQQZIKVTC23NWLQVP3WJ2RI2OTSJTNYOIGICST6DUXR"
	    },
	    {
	      "balance": "0.0000000",
	      "limit": "922337203685.4775807",
	      "asset_type": "credit_alphanum4",
	      "asset_code": "USD",
	      "asset_issuer": "GBSTRH4QOTWNSVA6E4HFERETX4ZLSR3CIUBLK7AXYII277PFJC4BBYOG"
	    },
	    {
	      "balance": "57.9899400",
	      "asset_type": "native"
	    }
	  ]
	}


## Anchor

Anchors issue assets on top of the Stellar network and then credit those asset to accounts on the Stellar network. 

If the Anchor represents fiat, then it is likely an authorized entity to deal with money like banks, savings and credit institutions or a remittance company. Users deposit fiat to the anchor's account and they credit the user Stellar account with the balance.

Banks and Venmo works like this except with a private ledger. Unlike Stellar which uses a public ledger. 

Anchors can represent also other cryptocurrencies.

## Mapping Vemno to Stellar

1. Download the app and create an user.
2. Go through phone number, email and bank account verification.
3. Once you are authorized to use Venmo, transfer money from your bank account and send it to other Venmo users.
4. Transfer to your bank whatever balance you have left.

### Download the app and create an user

Users should be able to download an app and then create an account. To keep things the application will use an username no password for sign-up/sign-in.

### Account verification

In Venmo there is a verification process. By default every user will be marked as verified. In real application, you probably want to collect user data like SSn, driver's license, passport, proof of residence, etc.

### Credit from bank account

The app will have a section which will simulate transferring from your bank account.

### P2P payments

Once the account has been provisioned and have some Dollars, users should be able to send money to other users.

### Transfer balance from AnchorX to Bank account

The app will have a section for depositing their dollars to their bank account.

## Building the backend

In this section, you'll be implenmenting a GraphQL AP which will support user sign-up and sign-in, deposits, withdrawls and payments. The mobile application will be interacting with this API.

The server will be written in TypeScript and use Prisma to generate the user management API.

## Setting up the server

In this section you can find a GraphQL server created with GraphQL CLI and using the TypeScript template. To make things easy there is boilerplate project which you can use to get started.

Download the boilerplate running the following commands:

	git clone https://github.com/abuiles/anchorx-api-boilerplate anchorx-api

	cd anchorx-api

## User Model

The user model is defined in 

	database/datamodel.graphql

Users in AnchorX signup using their username. After they signup, the service assigns automatically a Stellar account. The code on the right is the prisma representation for the user model.

After adding the user model definition to 

	database/datamodel.graphql

you have to run 

	yarn prisma deploy

to create a new table in the database and get the CRUD end-points for users.

	type User {
	id: ID! @unique
	username: String! @unique
	stellarAccount: String!
	stellarSeed: String!
	}

The pull request #2, shows the changes added in this step. The important changes, the rest is code generate by prisma.

Next you need to add a GraphQL mutation to support user signup. After a new user is created, you need to provision a Stellar account. Provisioning a new account requires multiple steps:

1. Create a public and private key pair.
2. After the public key has been created, fund that acount with some Lumens.
3. After the account has been created in the Stellar ledger, create a trustline with the anchor asset.

## User signup mutation

In this section you will start defining the GraphQL schema and define the resolvers.

Start by defining a mutation called signup and a query called user. The first one takes a username and returns an User instance and the second one allows you to retrieve an user given their username.

Next you need to add the code in the resolver to support signup and user find, open the file

	src/index.ts

and make sure that it looks like the code on the right. There are links to diffs included so you can see exactly what changed. For this tutorial Prisma is taking care of the databases and ORM setup.

In the mutation signup, you notice that 

	stellarAccount and stellarSeed 
	
have a fixed value of 1234. That's just a placeholder. In the next section will add the

	JS Stellar SDK

and use it to generate the account and the seed, also we'll add a fake service to encrypt the seed before saving it to the database.

The following is an edit of the

	src/schema.graphql

that defines the schema

	# import User from './generated/prisma.graphql'

	type Query {
		user(username: String!): User
	}

	type Mutation {
		signup(username: String!): User!
	}

This defines the resolvers and queries in the 

	src/index.ts

	const resolvers = {
		Query: {
			user(_, { username }, context: Context, info){
				return context.db.query.user(
					{
						where: {
							username
						}
					},
					info
				)
			}
		},
		Mutation: {
			signup(_, { username }, Context, info){
				const data = {
					username,
					stellarAccount: '1234',
					stellarSeed: '1234'
				}

				return context.db.mutation.createUser(
					{ data },
					info
				)
			},
		},
	}

## Assigning a Stellar account to new users

When a user signups on the service it will generate a Stellar account.

First install the

	JS Stellar SDK

and the types definition for TypeScript.

After installing the SDK, you need to change the signup mutation so it generates a real public key and seed.

To do so, the Stellar SDK has a class called Keypair.

You'll need to import Keypair and then call

	Keypair.random()

to generate a new pair.

On the right you can see the changes to

	src/index.ts

You should never store seed keys as plain text. In a production app you should probably be using something like Google or AWS KMS and have a clear set of restrictions of who can encrypt or decrypt. 

In this tutorial you'll be using a encryption module for Node called

	crypto-js

In the next section you will add the module and encrypt the seed before saving it.

	Install the stellar-sdk and then TypeScript types for the stellar-sdk

	yarn add stellar-sdk
	yarn add @types/stellar-sdk

	import { importSchema } from 'graphql-import'
	import { Prisma } from './generated/prisma'
	import { Context } from './utils'
	import { Keypair } from 'stellar-sdk'

	const resolvers = {
		Query: {
			...
		},
		Mutation: {
			signup(_, { username }, context: Context, info) {
				const keypair = Keypair.random()

				const data = {
					username,
					stellarAccount: keypair.publicKey(),
					stellarSeed: keypair.secret()
				}
			}
		}
	}

## Encrypting the seed

Install

	crypto-js

and after that you can use it in the signup mutation to store the encrypted seed.

	Add the crypto-js module with its types.

	yarn add crypto-js
	yarn add @types/crypto-js

	Use crypto-js before storing the seed key

	import { Prisma } from './generated/prisma'
	import { Context } from './utils'
	import { Keypair } from 'stellar-sdk'
	import { AES } from 'crypto-js'

	const resolvers = {
		Query: {
			signup(_, { username }, context: Context, info){
				const keypair = Keypair.random()

				const configCryptoScret = 'StellarIsAwesome-But-Do-Not-Put-This-Value-In-Code'

				const secret = AWS.encrypt(
					keypair.secret(),
					configCryptoScret
				).toString()

				const data = {
					username,
					stellarAccount: keypair.publicKey(),
					stellarSeed: keypair.secret()
					stellarSeed: secret
				}
				)
			}
		}
	}

## Testing

You can test the changes made up to this point by running the server.
