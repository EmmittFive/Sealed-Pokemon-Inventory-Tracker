# Software Requirements Specification

## Sealed Pokémon Inventory Tracker

## 1. Introduction

### 1.1 Purpose

This document lays out what I want the Sealed Pokémon Inventory Tracker to do before I start designing the database and building the application. I want one place where I can see what sealed Pokémon products I own, how many I have, what I paid, and what the collection is worth now.

The database design and final technology stack will be worked out separately.

### 1.2 Project Overview

I've been building my own sealed Pokémon collection, and keeping track of everything has started getting annoying as it grows. Purchase prices might be buried in old order confirmations, quantities might just be something I remember, and checking current value usually means going through a few different pricing sites.

I want the tracker to answer the questions I actually care about without making me piece everything together every time: What do I own? How many do I have? What did I pay? What is it worth now? How much am I up or down?

I am building around my own collection first because that gives me a real workflow to design around. The same basic idea could work for another collector or a smaller local card shop, but I do not want to design for every possible user before the core app even exists.

### 1.3 Scope

The app will focus on sealed Pokémon TCG products such as booster boxes, booster bundles, ETBs, UPCs, cases, and similar sealed products. The user will keep track of what they own, how many they own, when they bought it, and what they paid.

The app will handle current market value, total amount spent, and gain or loss. Current prices should come from an outside pricing source instead of being entered manually.

# 2. Overall Description

## 2.1 Product Perspective

The Sealed Pokémon Inventory Tracker will be a web app that can be used through a normal browser. Inventory data will be saved in a database so it stays there between sessions, and the user should never need to interact with the database directly.

Pricing will come from an outside source, but the inventory itself should still be usable if that source is temporarily unavailable.

## 2.2 User Profile

The main user is a sealed Pokémon collector. The app should be easy to use and should not require any knowledge of databases or programming. Adding a product, updating a quantity, entering a purchase price, or checking the collection value should all be straightforward.

A small local game store could probably use the same basic system too. Their quantities would be larger and inventory would change more often, but most of the information being tracked is still the same. I'm designing with collectors in mind first.

## 2.3 Assumptions and Constraints

For now, I am assuming one person is managing one inventory. The system also assumes that purchase information entered by the user is accurate, while current values will depend on the pricing source being used.

The first version will only support sealed Pokémon TCG products and is not meant to be a full point-of-sale or store management system. Automatic pricing will depend on what data sources are available and what their APIs or terms allow.

I want to keep the project realistic for one person to build. I would rather have a smaller application that works well and is useful to me than keep adding features that make it harder to finish.

# 3. Functional Requirements

Priority levels:

**1** - High priority
**2** - Medium priority
**3** - Low priority

| No.      | Requirement                       | Description                                                               | Priority |
| -------- | --------------------------------- | ------------------------------------------------------------------------- | -------: |
| **FR1**  | Add Inventory Entry               | Add a sealed product with quantity, purchase price, and purchase date.    |        1 |
| **FR2**  | Edit Inventory Entry              | Update saved inventory information.                                       |        1 |
| **FR3**  | Delete Inventory Entry            | Remove an inventory entry with a confirmation first.                      |        1 |
| **FR4**  | Track Product Information         | Store product name, Pokémon set, and product type.                        |        1 |
| **FR5**  | Track Quantity                    | Store how many units of a product are owned.                              |        1 |
| **FR6**  | Track Purchase Information        | Store purchase price and purchase date.                                   |        1 |
| **FR7**  | Current Market Price              | Retrieve the current market price from an outside pricing source.         |        1 |
| **FR8**  | View Inventory                    | Show the products in the inventory and their main information.            |        1 |
| **FR9**  | Search Inventory                  | Search for products by name.                                              |        1 |
| **FR10** | Filter Inventory                  | Filter by things like Pokémon set or product type.                        |        1 |
| **FR11** | Calculate Amount Spent            | Calculate cost for individual entries and the collection overall.         |        1 |
| **FR12** | Calculate Current Value           | Calculate current value for individual entries and the full collection.   |        1 |
| **FR13** | Calculate Gain or Loss            | Show how much a product or the collection is up or down.                  |        1 |
| **FR14** | Calculate Percentage Gain or Loss | Show gain or loss as a percentage.                                        |        1 |
| **FR15** | Inventory Summary                 | Show the main collection totals in one place.                             |        1 |
| **FR16** | Save Inventory                    | Keep inventory saved between sessions.                                    |        1 |
| **FR17** | Automatic Price Refreshing        | Refresh market prices automatically over time.                            |        2 |
| **FR18** | Price History                     | Save older market prices to track changes over time.                      |        2 |
| **FR19** | Purchase History                  | Keep separate purchases of the same product at different prices or dates. |        1 |
| **FR20** | Product Images                    | Add product images for easier identification.                             |        3 |
| **FR21** | CSV Import and Export             | Import or export inventory as CSV.                                        |        3 |
| **FR22** | User Accounts                     | Support multiple users with separate inventories.                         |        3 |

# 4. Non-Functional Requirements

Reliability and usability matter more to me than designing for scale I probably will not need.

| No.       | Requirement          | Description                                                                      | Priority |
| --------- | -------------------- | -------------------------------------------------------------------------------- | -------: |
| **NFR1**  | Usability            | Main features should be easy to understand without a guide.                      |        1 |
| **NFR2**  | Performance          | Common inventory actions should feel quick.                                      |        1 |
| **NFR3**  | Data Integrity       | Prevent bad or incomplete inventory data from being saved.                       |        1 |
| **NFR4**  | Reliability          | Saved inventory should remain accurate and available.                            |        1 |
| **NFR5**  | Input Validation     | Check quantities, prices, dates, and required fields before saving.              |        1 |
| **NFR6**  | Error Handling       | Show a useful message when something fails.                                      |        1 |
| **NFR7**  | Maintainability      | Keep the project organized and easy to work on later.                            |        2 |
| **NFR8**  | Security             | Keep passwords, API keys, and database credentials out of the public repository. |        1 |
| **NFR9**  | Responsive Interface | Work well on normal desktop and laptop screen sizes.                             |        2 |
| **NFR10** | Pricing Independence | Keep inventory usable if the pricing source is unavailable.                      |        2 |
| **NFR11** | External Data Usage  | Use outside pricing sources in a way that follows their terms.                   |        1 |

# 5. Data Requirements

This is one of the parts I want to get right before I start coding because a bad database design would make later features harder than they need to be. The system needs to distinguish between a Pokémon product and the copies of that product I actually own.

For example, if I buy six Perfect Order Booster Boxes at $141 each and then buy two more later at $165 each, I still own eight boxes of the same product. I do not want the second purchase to overwrite the first because both the price and date matter for my cost basis.

The database needs to support multiple purchases of the same product without duplicating all of the product information every time. I also want to leave room for price history later. I do not need to build it immediately, but I would rather account for it while the database is still easy to change.

The exact tables and relationships will be worked out in the ER diagram.

# 6. Interface Requirements

The inventory page will probably be the screen I use the most, so I want it to give me a useful overview right away. Quantity, purchase cost, current market price, current value, and gain or loss should be visible or easy to get to.

Adding and editing inventory should be simple, and search and filtering should be available from the main inventory view.

The app will also need to connect to an outside pricing source. TCGplayer, PriceCharting, TCGCompare, PokeData, Collectr, or another source could work, but I have not picked one yet.

# 7. Acceptance Criteria

For me, the first version is successful if I can actually use it instead of going back to a spreadsheet or digging through old orders whenever I want to keep up to date on my collection.

I should be able to add the products I own, record what I paid, keep the quantities updated, and see their current market prices without having to look them up myself. When I come back later, that information should still be there.

I should also be able to see how much money I have put into the collection, what it is currently worth, and how far up or down I am.
