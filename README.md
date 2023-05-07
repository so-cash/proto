# Published website for the cash on chain prototype

Available at https://so-cash.github.io/proto

This application simulate the back office system of a bank that manages client cash accounts on-chain for a single currency.    

Follow the examples below to understand how to use the application.

## Initialize your environment

You need to setup the configuration for your environment, that for this prototype is saved in the storage of your browser (so carefull as you can lose it)

We demonstrate the use of the setup with the Kerleano network, being the testnet of the Proof of Climate awaReness
* click to load the default config: https://so-cash.github.io/proto?config-url=https://so-cash.github.io/proto/kerleano.json
* save the config by clicking the "SAVE CONFIGURATION" btn
* you arrive in the Bank status page

Then you need to create a wallet to represent the bank back office.
* In the top left corner (where the black & white icon sits) click the "select a wallet" link
* You are redirected to the Wallet Custody application (hosted by CACIB) that allow creating Ethereum based wallets 
* Click the "CREATE" button in the "Create a ew Wallet" panel
* Follow the instructions and keep note of your wallet address and password (recovery is possible by CACIB support)
* The wallet appears in a list below. Open the folding section for the wallet you created (or an existing wallet) and click "⏎ SELECT" button.
* The selected wallet is displayed in the top left corner and the available amount of GWei is also shown (0 GWei by default).

Then you need to get some GWei (or $10^{-9}$ CRC).
* Go to [Kerleano issues](https://github.com/ethereum-pocr/kerleano/issues) and create a Faucet issue to request CRCs
  * Specify the address of your wallet and the reason for requesting CRC
  * The community with access to CRC will activate the faucet for your address
* You will receive a confirmation via the issue and when refreshing your page you see the balance of your wallet credited.
* You can also see the balance directly in the [Kerleano explorer](https://ethereum-pocr.github.io/explorer/kerleano) 

Through out the following steps you can monitor the transactions in the Kerleano explorer by finding the address of the smart contract and clicking the "Token" name (ie /token/<address> url).

## Creating the bank back office module in the blockchain
Here you will deploy a smart contract that represent the back office module where you, as bank, will manage client cash accounts:

(If you already did that step, you can initialize the bank BO module by setting the address of the smart contract)

* click the "CREATE THE BANK" button
* you are requested to enter the password of your wallet to sign the transaction (you can click "Remember the password for the session" to avoid capturing the password at every next transaction)
* after a few seconds the screen displays the created smart contract address ad several information explained below
  * Bank instance: the address of the smart contract deployed representing the back office module of your bank
  * Bank instance owner: the address of your wallet, being displayed as the owner of the BO module. 
  * Total commercial money supply in circulation: Initialized to zero, the total cash (liabilies to your clients or client deposits)
  * Total debt against other banks: The total net balance against other banks you are in debt of (negative balance) or in credit of (positive balance)

* Go the the [prototype issues](https://github.com/so-cash/proto/issues) to record your bank address for other to find your bank BO module and create correspondent banking relationships

## Creating customer cash accounts
Here you will open an account to a fictive client and manage its balance

* Go the the Client account menu
* Set the name of the client (will be public on chain, so be caution)
* Click "CREATE NEW ACCOUNT" to deploy a new smart contract that represents the client account for this client, linked to your BO module
* After a few seconds the pnel displays the account created with 
  * a zero balance in the currency of the BO module
  * address: is the smart contract address of that account
  * IBAN: is the automatically created IBAN from the address under the [ICAP standard](https://ethereum.org/en/glossary/#icap)
  * owner: the smart contract address of your BO module, indicating that your BO is in charge of this account
* You are expected to communicate the address / IBAN to your client

You can create as many accounts as you wish

## Your client deposit 100 000 EUR
Here we will simulate a client transfering money to its newly created account with the following steps:
* Your bank receives a swift message (MT202 or MT103) from another bank to credit the account of your client with 100 000 EUR
* Your nostro is credited by the other bank
* You credit the account of your client with 100 000 EUR
  * Go to the Client account menu
  * Select the account of your client
  * Set the amount to 100 000 EUR
  * Click "CREDIT" 
  * few seconds later the balance of the account is updated to 100 000 EUR
* Accounting wise your internal general ledger should reflect the following entries:
  * Debit the nostro account of your bank with 100 000 EUR
  * Credit the account of your client with 100 000 EUR


## Your client request a transfer of 10 000 EUR to an off-chain account
Here we will simulate a client transfering money from its account to an off-chain account with the following steps:
* Your client request a transfer of 10 000 EUR to an off-chain account
  * Go to the Client account menu
  * Select the account of your client
  * Set the amount to 10 000 EUR
  * Click "DEBIT" 
  * few seconds later the balance of the account is updated to 90 000 EUR
* Accounting wise your internal general ledger should reflect the following entries:
  * Debit the account of your client with 10 000 EUR
  * Credit the nostro account of your bank with 10 000 EUR
* Your bank sends a swift message (MT202 or MT103) to the other bank to debit your nostro account with 10 000 EUR

## Your client request a transfer of 25 000 EUR to another on-chain client account within your bank 
Here we will simulate a client transfering money from its account to another on-chain account with the following steps:
* Your client request a transfer of 25 000 EUR to another on-chain account within your bank
  * Go to the Client account menu
  * Select the account of your client
  * Click "TRANSFER..."
  * Set the amount to 25 000 EUR
  * Set the target account to the other account you created
  * Click "OK" 
  * few seconds later the balance of the account is updated to 65 000 EUR and the balance of the other account is updated to 25 000 EUR
* Accounting wise your internal general ledger should reflect the following entries:
  * Debit the account of your client with 25 000 EUR
  * Credit the account of the other client with 25 000 EUR

## Creating a correspondent banking relationship
Like in the real world, you need to create a correspondent banking relationship with other banks you want to transfer money to. The following steps can be seen as equivalent to echanging RMA (Relationship Management Application) in the SWIFT world.

* Get the smart contract address of the bank you want to create a relationship with
* Go to the Bank status menu
* In the list of registered banks, Resiter a new bank panel, set the address of the bank smart contract
* Click "OK"
* after a few seconds the panel displays the bank you registered with
  * the address of the bank smart contract
  * the owner of that bank smart contract
  * the registration status
  * We owe them: The balance you are in debt toward the other bank (recorded in the IOU smart contract)
  * They owe us: The balance the other bank is in debt toward you (recorded in the IOU smart contract)


* Communicate your bank address to the other bank so that it can register your bank as well

Once both banks have registered each other, they can start transferring client money between them.

## Your client request a transfer of 15 000 EUR to an on-chain account of another bank
Here we will simulate a client transfering money from its account to an on-chain account of another bank with the following steps:
* Your client request a transfer of 15 000 EUR to an on-chain account of another bank
  * Go to the Client account menu
  * Select the account of your client
  * Click "TRANSFER..."
  * Set the amount to 15 000 EUR
  * Set the target account to the address account at the other bank
  * Click "OK" 
  * few seconds later the balance of the account is updated to 50 000 EUR and the balance of the other account is updated to 15 000 EUR
  * the balances in the IOU smart contract is updated to reflect that you bank owe 15 000 EUR to the other bank and the other bank has a debt of 15 000 EUR toward your bank
* Accounting wise your internal general ledger should reflect the following entries:
  * Debit the account of your client with 15 000 EUR
  * Credit the nostro account of your bank with 15 000 EUR
  * If interbank settlement happens during the same day, no more accounting entry are required, else you need to record a credit of a liability with the other bank against your nostro account.

## Interbank settlement
Here we will simulate the settlement of the interbank debt with the following steps:
You have a debt of 15 000 EUR toward the other bank that you can pay by a Swift transfer (MT202)
* make the Swift transfer to the other bank
* The other bank receives the credit of 15 000 EUR on its nostro account and
  * Goes to the Bank status menu
  * Select your bank in the list of registered banks and sees that the payment received match the received payment
  * Click the "CONFIRM YOU RECEIVED THE PAYMENT" button to acknowledge the payment of the debt in the IOU smart contract
  * a few seconds later the balances in the IOU smart contract is updated to reflect that the debt is paid

## Allowing your client to operate its account
In the previous steps, you have been operating the account of your client. Now you want to allow your client to operate its account by itself.   
Since the account is a smart contract, you can add allow external wallets to operate the account.

We will simulate this using metamask as the external wallet. And the account of the client will be represented as an ERC20 token in metamask.
* In metamask [connect to the Kerleano network](https://chainlist.org/?search=kerleano&testnets=true) 
* Identify (or create) a wallet you want to use
* Ensure you have CRC available (if not transfer an amount of CRC from you bank wallet to your metamask wallet via the Wallet menu)
* In the prototype, go the the Client account menu and select the account of your client
* In the "grant access to" field enter the address of the metamask wallet and click "OK"
* Back in metamask, import the client account as a token (Assets > Import tokens)
  * Token contract address: the address of the client account
  * Token symbol: automatically populated with the currency symbol of the BO module
  * Token decimals: automatically populated with the currency decimals of the BO module (2 by default)
  * Validate and confirm import
* You can now see the balance of the client account in metamask
* You can now transfer money from the client account to another on-chain account using metamask, the beneficiary address should be the smart contract address of the beneficiary account within the same bank or another bank in correspondent banking relationship.