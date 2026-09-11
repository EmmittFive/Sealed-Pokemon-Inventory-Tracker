# Database Design

## Sealed Pokémon Inventory Tracker

## Design Overview

The ER diagram gave me the general structure of the database. From there, I turned the entities into tables and used their relationships to decide where the foreign keys should go. Most of the relationships are one-to-many, so the foreign key is placed in the table on the many side.

At first, the product and inventory information could look like it belongs in one table, but separating them makes more sense. The `product` table describes the sealed product, while `inventory_entry` keeps the information from each purchase. This lets multiple inventory entries point back to the same product without repeating all of its information.

Pokémon sets and product types have their own tables for a similar reason. A set or product type can be used by many products, so it only needs to be entered once. It also avoids small differences in spelling and makes filtering more consistent.

Pricing has a few separate parts. `price_source` identifies where the data came from, `external_listing` connects a product to its listing on that source, and `market_price` stores each price that is retrieved. Keeping these separate allows the database to hold older prices without mixing them into the inventory tables.

## Tables

### `pokemon_set`

This keeps the Pokémon set information in one place. A set is optional because some sealed products may not belong to one specific set.

| Column | Data Type | Rules | Description |
| --- | --- | --- | --- |
| `set_id` | INTEGER | Primary key | Identifies the set |
| `set_name` | VARCHAR(100) | Required, unique | Name of the set |
| `release_date` | DATE | Optional | Release date of the set |
| `notes` | TEXT | Optional | Additional set information |

### `product_type`

This keeps product types such as Booster Box, Elite Trainer Box, Booster Bundle, Case, or Ultra Premium Collection from being entered repeatedly.

| Column | Data Type | Rules | Description |
| --- | --- | --- | --- |
| `product_type_id` | INTEGER | Primary key | Identifies the product type |
| `type_name` | VARCHAR(50) | Required, unique | Name of the product type |
| `description` | TEXT | Optional | Description of the product type |

### `product`

This is the basic information about the sealed product itself.

| Column | Data Type | Rules | Description |
| --- | --- | --- | --- |
| `product_id` | INTEGER | Primary key | Identifies the product |
| `set_id` | INTEGER | Optional, foreign key | References `pokemon_set.set_id` |
| `product_type_id` | INTEGER | Required, foreign key | References `product_type.product_type_id` |
| `product_name` | VARCHAR(255) | Required | Name of the product |
| `image_url` | VARCHAR(500) | Optional | Location of a product image |
| `notes` | TEXT | Optional | Additional product information |

### `inventory_entry`

Each row represents one purchase of a product and keeps its quantity, unit price, and purchase date.

| Column | Data Type | Rules | Description |
| --- | --- | --- | --- |
| `entry_id` | INTEGER | Primary key | Identifies the inventory entry |
| `product_id` | INTEGER | Required, foreign key | References `product.product_id` |
| `quantity` | INTEGER | Required, greater than 0 | Number of units owned from this entry |
| `unit_purchase_price` | NUMERIC(10,2) | Required, 0 or greater | Price paid for one unit |
| `purchase_date` | DATE | Required | Date of the purchase |
| `notes` | TEXT | Optional | Additional purchase information |
| `created_at` | TIMESTAMP | Required | When the entry was created |

### `price_source`

This keeps track of where the market price is coming from.

| Column | Data Type | Rules | Description |
| --- | --- | --- | --- |
| `source_id` | INTEGER | Primary key | Identifies the pricing source |
| `source_name` | VARCHAR(100) | Required, unique | Name of the source |
| `base_url` | VARCHAR(500) | Optional | Website or API address |
| `notes` | TEXT | Optional | Additional source information |

### `external_listing`

Different pricing sites may use their own ID or listing for the same product. This table connects that outside listing to the product in the tracker.

| Column | Data Type | Rules | Description |
| --- | --- | --- | --- |
| `listing_id` | INTEGER | Primary key | Identifies the external listing |
| `product_id` | INTEGER | Required, foreign key | References `product.product_id` |
| `source_id` | INTEGER | Required, foreign key | References `price_source.source_id` |
| `external_product_id` | VARCHAR(255) | Required | Product identifier used by the source |
| `listing_url` | VARCHAR(500) | Optional | Link to the outside listing |
| `notes` | TEXT | Optional | Additional listing information |

### `market_price`

Each time the app retrieves a price, it can add a new row here. The newest row can be used as the current price while the older rows remain available for price history.

| Column | Data Type | Rules | Description |
| --- | --- | --- | --- |
| `price_id` | INTEGER | Primary key | Identifies the price record |
| `listing_id` | INTEGER | Required, foreign key | References `external_listing.listing_id` |
| `price` | NUMERIC(10,2) | Required, 0 or greater | Market price for one unit |
| `currency` | CHAR(3) | Required | Currency code, such as `USD` |
| `retrieved_at` | TIMESTAMP | Required | When the price was retrieved |
| `notes` | TEXT | Optional | Additional price information |

## Relationships

| Relationship | Description |
| --- | --- |
| `pokemon_set` to `product` | One set can have many products. A product can belong to zero or one set. |
| `product_type` to `product` | One product type can have many products. Each product has one product type. |
| `product` to `inventory_entry` | One product can have many inventory entries. Each entry belongs to one product. |
| `product` to `external_listing` | One product can have many external listings. Each listing belongs to one product. |
| `price_source` to `external_listing` | One source can provide many listings. Each listing uses one source. |
| `external_listing` to `market_price` | One listing can have many price records. Each price belongs to one listing. |

## Calculated Values

| Value | Calculation |
| --- | --- |
| Product quantity | Sum of the product's inventory entry quantities |
| Entry cost | `quantity * unit_purchase_price` |
| Total amount spent | Sum of all entry costs |
| Current product value | Product quantity multiplied by its latest market price |
| Total inventory value | Sum of all current product values |
