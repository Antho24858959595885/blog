---
layout: post
title: "Lab 6: Data Modeling"
---

## My Assumptions Making the Model: 
- Customers must register before ordering, so every Order is tied to exactly one Customer.
- Items belong to exactly one Manufacturer.
- Inventory is tracked at the Item level using quantity_available (only for customer).
- Finalized orders cannot be edited by the customer.
- The details “quantity desired,” “special instructions,” and “allow substitution” belong to the relationship between Order and Item, so they are stored in Order_Item.

## User Stories 

### US1 — Customer Registration (3 points)
As a customer, I want to register with my name, address, and phone number so that I can place orders.  
**Depends on:** none  
**Connects to:** US3 (must be registered to start an order)

### US2 — View Items for Sale (2 points)
As a customer, I want to browse items with name, price , description, quantity available, and manufacturer so that I can decide what to buy.  
**Depends on:** US5 (store owner must create items first)  
**Connects to:** US4

### US3 — Create / Manage a Draft Order (Pickup or Delivery) (5 points)
As a registered customer, I want to start an order for pickup or delivery so that I can build my cart before checkout.  
**Depends on:** US1  
**Connects to:** US4, US6

### US4 — Add Items to Order With Quantity + Instructions + Substitution Flag (5 points)
As a customer, I want to add items to my order with quantity desired, special instructions, and substitution permission so that my order is accurate even if stock changes.  
**Depends on:** US2 and US3  
**Connects to:** US6

### US5 — Store Owner Manages Catalog and Inventory (8 points)
As the store owner, I want to create and update items (name, price, description, quantity available, manufacturer) so that customers can shop online.  
**Depends on:** none  
**Connects to:** US2, US4

### US6 — Finalize Order With Scheduled Fulfillment Time (5 points)
As a customer, I want to choose a time and date for fulfillment and finalize my order so the store can prepare it and I can’t accidentally change it afterward.  
**Depends on:** US3 and US4  
**Connects to:** the “Finalized orders cannot be changed” rule

## LucidChart ERD (PNG)
![LucidChart ERD](/assets/images/lab6_erd.png)

### ERD Discussion
My LucidChart uses five entities: Customer, Manufacturer, Order, Item, and Order_Item. A customer can place many orders, and each order belongs to one customer (tracked by their id), just like how Manufacters can make many items but only one manufacturer. Because an order can have many items and an item can appear in many different orders, I used Order_Item as a bridge between the two. I thought it would also be useful to store order specific details like desired quantities and special customer instructions (with potential to subsistute items, which Walmart has been so kind to automatically do for me). For housekeeping concerns, I also included an order status and a scheduled fulfillment date/time on the order so nothing can be changed when the order is already complete.  

## Redgate Schema (PNG)
![Redgate Schema](/assets/images/lab6_schema.png)

### Schema Discussion
Almost everything stayed the same for my Schema desigm. although I roughly mentioned primary and foreign keys in my LucidChart design, they are much more formally defined here. Each table has a primary key (customer_id, order_id, and item_id) to identify records. They promote the same relationships shown in my first diagram: Order links to Customer, Item links to Manufacturer, and Order_Item links to both Order and Item. This structure keeps the catalog data seperate from the customer data. 


### Am I satisfied with my data representation?
I'd say I'm satisfied. Once things started to click about how these diagrams worked, I found it pretty seemless to implement. I'm happy with how I organized the diagram, keeping things seperate made it really easy to visualize in my head. 

### Complications I could encounter implementing this model
- Inventory changes while orders are being built (race conditions / overselling).
- Price changes over time (whether an order should lock the price at add-to-cart or at finalize).
- Enforcing “no edits after finalization” (typically handled in application logic, possibly with constraints/triggers).
- Delivery details (if customers need multiple addresses or special delivery instructions).

## My Personal Experience Building This
It was pretty fun to make the designs, I like scenarios that test my critical thinking skills, and ensuring that I could resolve any potential issue that could be encountered was no doubt challenging (I'm sure I missed some as well). One big glaring issue I did run into was, funnily enough, the captcha on Redgate. For the life of me I couldn't figure out why or how I kept getting the prompts wrong, but once I got through the rest was smooth sailing.