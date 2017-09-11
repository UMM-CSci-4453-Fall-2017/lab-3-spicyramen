# Lab 3 - Spicy Ramen Speculations
## How will inventory be tracked?  
We will have a table named `inventory`. It will be structured as follows:
### Inventory Table  

|id (Primary)| vendor | item| price | quantity | unit |note|
|------------|:------:|:---:|:-----:|:--------:|:---:|:---:|
| int        | text   |text |decimal (7,2)| int | text | text|

`id` will be unique, and will be our primarty key. We can see how many of each item is in stock based on the quantity key. Employees will interact with this table.
## Should the system keep track of vendors?
We think it would be a good idea to include vendor information in our `inventory` table so a manager can lookup items in the inventory by vendor. This will be useful for managers who often order in bulk for a single vendor. With this feature, managers will be able to at a glance, see how many, and with what quantity, the products they have from a single vendor. To implement this, we have a field in our `inventory` table (see above), `vendor`, that will represent this value.
## What about the people working the tills (cash registers)?
  * **Should the system track when they log into a till?**  
  * **Should the system track how long an interaction takes place (efficiency?).**   
  _Answers the two privious bullet points._ Yes, it is a good idea to track what employees and managers interacted with the table and for how long. The database system should be able to store user sessions in log files.
  *  **Should the system try to help with up-selling? (suggesting other merchandise... would you like a drink with that?)**   
  In the current dataset, we do not have enough data to build a accurate recommender system yet. In addition, the user interface is not visible to the the customer, just the employee. Recomender systems usually perform best, when the system can recieve user input directly.

## How should it deal with
  To accomidate sales, promotional items, coupons, we will create a table called
  `Deals` Table.   
  ### Deals Table  
  | deal-id | item-id | price-multiplier | name | deal-type |
  |---------|:-------:|:----------------:|:----:|:----:|
  |int (primary and unique)| int | int | text | text |
  * **sale items**
  For every item purchased, the item-id will be searched in the Deals Table. If any record matches, and has deal-type **SALE**, the multiplier will be applied to the overal price of the item.   
  * **promotional items**  
  For every item purchased, the item-id will be searched in the Deals Table. If any record matches, and has deal-type **PROMOTIONAL**, the multiplier will be applied to the overal price of the item.
  * **coupons**
  Each coupon will have an associated deal-id in the `Deals` table. Each record is associated with an id from the `inventory` table, which we label item-id. The price will be the price from the inventory table times the quantity times the price-multiplier of the coupon record.
  * **items sold by unit**  
  We will simply multiply the quantity field by the price field to get the cost total cost of the item(s).
  * **items sold by volume**  
  For a single item that can be sold at different volumes (for instance, pop). We will insert multiple records into the `inventory` table, and will vary the unit feild.  
  * **refunds**  
  The refund price will be the item price times the quantity. We wil make the condition for our company, that any promotional or sale items are not refundable.
  * **mistakes**  
  1) Automated punishment by electricution (employees will be wearing shock collars)  
  2) Cashe the last 10 transactions, and allow them to be undone.

### items part of a combination deal  
#### combination types  

  | combo-id | combo-price |
  |----------|:-----------:|
  | int      | decimal(7,2)|

#### combination items  
  | combo-id | item-id |
  |----------|:-------:|
  | int      | int     |

When a customer purchases a combo, the cleark will enter the combo-id.
The system will:  
  1) Display the price of the combo-id.  
  2) Decrease the quantity of every item in the combo via the item-id.

## How independent should the individual tills be?  
  * **Can the tills operate if the local network is down?**
  Final transactions will occur once the server is back up. Transacations while the local network is down, will not be gaurteed correct.  
  * **What is stored on the server?**
  All of the tables, and databases.  
  * **What is stored on the till?**
  Temporary JSON/Javascript objects that will be sent to the server via HTTPS protocol.

## Generating Reports  
We will create a `status` table that will behave as follows:  

|item-id | sold-quantity|
|--------|:------------:|
| int    | int          |

Every time an item is sold, its sold-quantity will be incremented. 
