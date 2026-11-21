# Casino Final Project Proposal

Christian Livingston, CSCI 121 — Java final project

## Purpose

Demonstrates object-oriented programming, abstract data types and data structures from CSCI 121 by extending my ATM project into a text-based casino for fun.

## Overview

A text based casino where players use chips to play Blackjack and Slots. It builds on my ATM code: each player is also a bank customer, so they buy chips from checking, cash out winnings, and can take out a loan. Player data is saved between runs.

## Game Classes

- Game (abstract): holds the Player and the bet, handles betting and payouts, and requires each game to implement play().
- Blackjack: plays a round against the dealer using a Deck and two Hands.
- Slots: spins the reels and pays out on matching symbols.
- Cards, Deck, and Hand: model the cards, the 52-card deck, and a hand's total.
- Might add roulette, but it will depend on how the other stuff goes

## Other Classes

- Player extends Customer, adding chips, a Loan, and buying and cashing out chips.
- Loan: tracks the amount owed and its interest.
- Casino: replaces Bank and handles login, the admin menu, and saving.
- Reused from the ATM: HasMenu, User, Admin, Customer, CheckingAccount, SavingsAccount.

## Anticipated Use Cases

- Players: log in, buy chips, play games, cash out, and take or repay a loan.
- Admin: view reports, add users, and apply interest.

## Use of Object-Oriented Programming Paradigms

14 classes, one interface, and two enums:

- Inheritance: Player extends Customer; Blackjack and Slots extend Game.
- Encapsulation: all data members are private.
- Polymorphism: Blackjack and Slots implement play() differently.
- Abstraction: HasMenu is an interface; Game and User are abstract.
- Aggregation: Casino holds a list of Customers.
- Composition: Blackjack owns its Deck and Hands.

## Use of Data Structures

- ArrayList of Customers holds every user.
- The Deck is an ArrayList of Cards, shuffled and dealt from the end.
- Arrays hold the slot reels.
- Serialization saves all players.

## Technology / Implementation

Written in Java with a command-line menu, using interfaces, abstract classes, enums and object serialization. No external dependencies.

## UML Diagram

```mermaid
classDiagram
    class HasMenu["HasMenu (interface)"] {
        +menu() String
        +start() void
    }

    class CheckingAccount["CheckingAccount (ser)"] {
        -double balance
        +CheckingAccount()
        +CheckingAccount(balance)
        +main() void
        +menu() String
        +start() void
        +getBalance() double
        +getBalanceString() String
        +setBalance(balance) void
        +checkBalance() void
        -getDouble() double
        +makeDeposit() void
        +makeWithdrawal() void
    }

    class SavingsAccount {
        -double interestRate
        +main() void
        +calcInterest() void
        +setInterestRate(rate) void
        +getInterestRate() double
    }

    class User["User (abstract)"] {
        -String userName
        -String PIN
        +login() boolean
        +login(userName, PIN) boolean
        +setUserName(userName) void
        +getUserName() String
        +setPIN(PIN) void
        +getPIN() String
        +getReport() String
    }

    class Admin {
        +Admin()
        +menu() String
        +start() void
        +getReport() String
    }

    class Customer["Customer (ser)"] {
        -CheckingAccount checking
        -SavingsAccount savings
        +Customer()
        +Customer(userName, PIN)
        +Customer(userName, PIN, checkingBal, savingsBal)
        +main() void
        +getChecking() CheckingAccount
        +getSavings() SavingsAccount
        +menu() String
        +start() void
        +changePIN() void
        +getReport() String
    }

    class Player["Player (ser)"] {
        -double chips
        -Loan loan
        +Player(userName, PIN)
        +Player(userName, PIN, checkingBal, savingsBal, chips)
        +main() void
        +menu() String
        +start() void
        +getChips() double
        +getChipsString() String
        +addChips(amount) void
        +removeChips(amount) boolean
        +buyChips() void
        +cashOut() void
        +takeLoan() void
        +repayLoan() void
        +getLoan() Loan
        +getReport() String
    }

    class Loan["Loan (ser)"] {
        -double amountOwed
        -double interestRate
        +Loan(amount, interestRate)
        +getAmountOwed() double
        +getAmountOwedString() String
        +addToLoan(amount) void
        +applyInterest() void
        +makePayment(amount) double
        +isPaidOff() boolean
    }

    class Game["Game (abstract)"] {
        +Player player
        +double currentBet
        +Game(player)
        +placeBet() boolean
        +payout(multiplier) void
        +menu() String
        +start() void
        +play() void
    }

    class Blackjack["Blackjack"] {
        -Deck deck
        -Hand playerHand
        -Hand dealerHand
        +play() void
        -dealInitialCards() void
        -playerTurn() void
        -dealerTurn() void
        -decideWinner() void
    }

    class Slots["Slots"] {
        -String[] symbols
        -int[] reels
        +play() void
        -spin() void
        -calcMultiplier() double
    }

    class Card["Card"] {
        -Suit suit
        -Rank rank
        +getValue() int
        +toString() String
    }

    class Suit["Suit (enum)"] {
        HEARTS
        DIAMONDS
        CLUBS
        SPADES
    }

    class Rank["Rank (enum)"] {
        TWO to TEN
        JACK
        QUEEN
        KING
        ACE
    }

    class Deck["Deck"] {
        -ArrayList cards
        +Deck()
        +shuffle() void
        +deal() Card
        +cardsLeft() int
    }

    class Hand["Hand"] {
        -ArrayList cards
        +addCard(card) void
        +getTotal() int
        +isBust() boolean
        +isBlackjack() boolean
        +toString() String
    }

    class Casino["Casino"] {
        -String FILE_NAME
        -Admin admin
        -ArrayList customers
        +Casino()
        +main() void
        +loadSampleCustomers() void
        +loadCustomers() void
        +saveCustomers() void
        +fullCustomerReport() void
        +addUser() void
        +applyInterest() void
        +applyLoanInterest() void
        +loginAsPlayer() void
        +menu() String
        +start() void
        +startAdmin() void
    }

    HasMenu <|.. CheckingAccount : Implements
    HasMenu <|.. User : Implements
    HasMenu <|.. Game : Implements
    HasMenu <|.. Casino : Implements
    CheckingAccount <|-- SavingsAccount : Extends
    User <|-- Admin : Extends
    User <|-- Customer : Extends
    Customer <|-- Player : Extends
    Game <|-- Blackjack : Extends
    Game <|-- Slots : Extends

    Customer "1" o-- "1" CheckingAccount
    Customer "1" o-- "1" SavingsAccount
    Player "1" o-- "1" Loan
    Game "n" o-- "1" Player
    Blackjack "1" o-- "1" Deck
    Blackjack "1" o-- "2" Hand
    Deck "1" o-- "n" Card
    Hand "1" o-- "n" Card
    Card "n" o-- "1" Suit
    Card "n" o-- "1" Rank
    Casino "1" o-- "1" Admin
    Casino "1" o-- "n" Customer
```
