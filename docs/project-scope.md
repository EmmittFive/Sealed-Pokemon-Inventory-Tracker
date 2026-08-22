# Project Scope

## Project Name

**Sealed Pokémon Inventory Tracker**

## Project Overview

The Sealed Pokémon Inventory Tracker is a web app for keeping track of sealed Pokémon products.

The main reason I wanted to make this is because I am building up my own sealed collection, and as it gets bigger it starts getting harder to keep track of everything. I want one place where I can see what I own, how much I paid for it, how many I have, and what the current market value is.

Instead of checking receipts, store orders, spreadsheets, and different websites every time, this app would keep all of that information together.

The same type of system could also be useful for small card shops or local game stores that need to keep track of sealed Pokémon inventory and compare their acquisition costs to current market prices.

## Problem

Once a sealed collection or store inventory starts getting bigger, it becomes harder to keep track of basic information.

For example:

* What products are currently in inventory?
* How many of each product are there?
* How much was paid for them?
* How much money has been put into the inventory total?
* What are the products worth now?
* Which products have gone up or down the most?

Sites like TCGplayer, PriceCharting, TCGCompare, PokeData, and Collectr are useful for checking prices, but I still want something focused more on inventory and purchase history.

## Goal

The goal is to build a web application that lets me keep track of my sealed Pokémon collection and compare what I paid to what the products are currently worth.

I want it to be useful enough that I would actually keep using it as my collection grows.

I also want the system to be general enough that it could be useful for another collector or a small local game store without having to completely redesign the application.

The project is also meant to be a portfolio project where I can show that I understand things like databases, frontend and backend development, APIs, and working with outside data.

## Target User

The main user is someone who collects sealed Pokémon products and wants to keep better track of their collection.

The application could also be useful for local game stores or small card shops that need a simple way to track sealed Pokémon inventory, acquisition costs, quantities, and current market prices.

At first, I am mainly building it around my own collecting workflow because that gives me a real use case to design around. The system should still be general enough that another collector or a small card shop could use it later.

## Main Features

The app should let the user:

* Add sealed products to their inventory
* Enter the Pokémon set
* Enter the type of product
* Enter how many they own
* Enter the purchase price
* Enter the purchase date
* Edit existing entries
* Remove products
* Search or filter the inventory
* See total money spent
* See current market value
* See gain or loss
* See percentage gain or loss

Product types could include things like:

* Booster boxes
* Booster bundles
* Elite Trainer Boxes
* Cases
* Ultra Premium Collections
* Other sealed products

## Minimum Viable Product

For the first working version, I want to keep it fairly simple.

The MVP should be able to:

1. Add a product
2. Edit a product
3. Delete a product
4. Store quantity
5. Store purchase price
6. Store purchase date
7. Store current market price
8. Show all inventory
9. Calculate total amount spent
10. Calculate current inventory value
11. Calculate gain or loss
12. Search or filter inventory

At first, market prices can be entered manually. Automatic pricing can be added after the main inventory system works.

## Pricing Data

One of the main features I want to add later is automatic price tracking.

Possible sources could include:

* TCGplayer
* PriceCharting
* TCGCompare
* PokeData
* Collectr
* Another Pokémon pricing source if I find a better option

I still need to research which sources provide the most useful and reliable data for sealed Pokémon products.

I also need to look into whether they provide an API, what their terms allow, and how easy it would be to match their product data with the products stored in my application.

Ideally, I would use an API or another officially supported way of getting the data. If that is not available, I can look into other options such as web scraping as long as the site's terms allow it.

For the first version of the project, prices can also just be entered manually. That way the rest of the inventory system does not depend on an outside pricing source before it can work.

## In Scope

For the first version, the project will focus on:

* Pokémon TCG sealed products
* Personal collection tracking
* Basic sealed inventory tracking
* Purchase or acquisition prices
* Quantities
* Current market prices
* Total inventory value
* Gain and loss calculations
* Basic search and filtering
* A web interface
* A database to save inventory

## Out of Scope

For now, I am not planning to include:

* Individual card tracking
* Card grading
* Buying or selling through the app
* Full point-of-sale features
* Employee management
* Supplier management
* Order fulfillment
* Tax calculations
* Price predictions
* Investment advice
* Mobile apps
* Social features
* Other card games

## Possible Future Features

Later on, I could add:

* Automatic price updates
* Price history
* Collection or inventory value charts
* Product images
* Average purchase price
* ROI percentages
* Where a product was purchased
* Supplier information
* Notes for condition
* Storage location
* Low inventory warnings
* CSV import and export
* User accounts
* Separate collector and store views
* Support for other card games

## Success Criteria

I would consider the project successful if I can actually use it instead of keeping track of my sealed collection somewhere else.

I should be able to open the app and quickly see:

* What I own
* How many I own
* What I paid
* How much I have spent total
* What everything is currently worth
* How much I am up or down

The system should also be structured in a way where the same basic inventory features could make sense for a small card shop.

The main goal is for it to be something useful to me first, while also being a strong project I can show on GitHub.
