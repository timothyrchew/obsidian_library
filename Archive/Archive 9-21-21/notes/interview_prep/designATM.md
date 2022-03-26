Reqs:

- User should be able to swipe a card
- User should be able to withdraw funds
- User should be able to deposit cash and check
- User should be able to transfer funds
- User should be able to check their balance


Questions:

- is the card connected to a specific bank?
- does the atm also belong to a single bank?
- are their any withdraw / deposit / transfer limits? if so, do we need to be concerned about a time window associated with the limit or do we just need to worry about a single transaction?

- are we interested mostly in the class design? how much code do we want to see? should we be able to run the code? what language?


Activity Flow:


Customer
- swipe card
- enter pin
- view transaction options
- select their transaction (withdraw cash)
- prompt for the amount with denomination types, or custom amount
- enter the dollar value
- dispense cash
- prompt for another transaction / logout


Classes:

Card
- cardNumber
- pin

Account
- accountNumber
- bankName
- balance

Bank
- address

ATM
- cashBalance

CardReader
- readCard()

Keypad
- getInput()

CashDispenser
- dispenseCash()

(Deposit)

CashDeposit
- depositCash()

CheckDeposit
- depositCheck()

Screen
- printMessage()

(Transaction)
- amount

WithdrawTransaction

DepositTransaction

GetBalanceTransaction

TransferTransaction


--

Requirements:
- Transactions: check balance, withdraw, deposit cash / check, transfer
- Transaction limits? withdraw / deposit max of 1000 per day
- Atm has $10,000


Classes:

Card
- accountNumber
- pin

Account
- balance
- type

Transactions
- timestamp
- wasSuccessful

WithdrawTransaction
- amount
- accountType

DepositCashTransaction
- amount
- accountType

DepositCheckTransaction 
- amount
- accountType

CheckBalanceTransaction
- balance
- accountType

ATM
- cash
- powerOn()
- powerOff()







