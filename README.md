# Python Credit Card Generator

`CCNumGen` is a Python 3.9 class that uses the [Luhn Algorithm](https://www.geeksforgeeks.org/luhn-algorithm/) to generate theoretically valid credit card numbers with CVV and expiration dates. 

* It can generate Amex, Discover, MC, Visa 13, and Visa 16 cards.
* It generates 3 or 4 digit CVV numbers, depending on the card type.
* It generates an expiration date that is 1 to 3 years from today.
* It prints each card to the console.
* It captures cards in a list of dictionaries. 

Adapted from: https://stackoverflow.com/questions/59232327/python-how-to-always-generate-valid-luhn-numbers

## Do the numbers work?

These are theoretical cards meant for testing the pre-checks that happen on a payment screen before payment is submitted. The number, CVV, and expiration are all theoretically valid, but their randomness makes it next to impossible that a given card is real. 

## Need different card numbers?

Sometimes for testing you need card numbers with specific prefixes. `CCNumGen` is easy to extend. 

* Name the new card type in the `CCNumGen` class -> `card_types` list.
* Define the new card in the `CC` class -> `CCDATA` dictionary.

## Usage

`CCNumGen` generates 5 types of cards. The second argument is the number of cards to generate. 

```
amex = CCNumGen('amex', 2)
discover = CCNumGen('discover', 2)
mc = CCNumGen('mc', 2)
visa13 = CCNumGen('visa13', 2)
visa16 = CCNumGen('visa16', 2)
```

Each card is printed to the console.

```
---------------------------
Type: amex
Number: 372672737435387
CVV: 8483
Exp: 09-2025
---------------------------
Type: discover
Number: 60019219645956610
CVV: 892
Exp: 05-2023
---------------------------
Type: mc
Number: 5169546719828815
CVV: 678
Exp: 03-2023
---------------------------
Type: visa13
Number: 4929291332548
CVV: 328
Exp: 08-2025
---------------------------
Type: visa16
Number: 4165317319538881
CVV: 106
Exp: 05-2024
---------------------------
```

Each card is stored in a list of dictionaries. 

```
[
  {
    'cc_type': 'mc', 
    'cc_num': '5538992865196249', 
    'cc_cvv': '617', 
    'cc_exp': '09-2024'
  }, 
  {
    'cc_type': 'mc', 
    'cc_num': '5191748311982485', 
    'cc_cvv': '434', 
    'cc_exp': '11-2023'
    }
]
```

To access the list directly:

```
print(amex.card_list)
print(discover.card_list)
print(mc.card_list)
print(visa13.card_list)
print(visa16.card_list)
```

To print the list to console:

```
amex.print_card_list()
discover.print_card_list()
mc.print_card_list()
visa13.print_card_list()
visa16.print_card_list()
```

## Handy Info

| Card       | Length    | Starts With | CVV Len | CVV Location                  |
|------------|-----------|-------------|---------|-------------------------------|
| Visa 13    | 13 digits | 4xx         | 3       | Back - left of sig            |
| Visa 16    | 16 digits | 4xx         | 3       | Back - left of sig            |
| MasterCard | 16 digits | 51 or 55    | 3       | Back - left of sig            |
| Amex       | 15 digits | 34 or 37    | 4       | Front - above CC num          |
| Discover   | 16 digits | 6011        | 3       | Back - last 3, after acct num |


## About CVV Numbers

**Other names:** 

* CSC - Card Security Code
* CID - Card Identification Number
* CVC2 - Card Verification Code
* CVV2 - Card Verification Value

**What is it?**

* An anti-fraud measure used by CC companies worldwide.
* A 3 or 4 digit number printed on the physical card - and not embossed. 
* Used for Card Not Present (CNP) transactions.
* Different from the CVV1 that's encoded on track 2 of the magnetic strip - this is used for Card Present Transactions (CPT).

**Generated with DES encryption using:**

* Pair of DES keys (CVKs)
* Primary Account Number (PAN)
* 4-digit Expiration Date.
* 3-digit Service Code. 
* Other data

**Amex:**

* Does have a 3 digit CID code on back of card.
* Uses the 4 digit CVV number on front of card for CNP transactions. 
