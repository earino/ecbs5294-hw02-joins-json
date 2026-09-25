# The brief: six KPIs for the board deck

**From:** Finance. **For:** the board's marketplace review. **Data:** the files in `data/raw/` (a selected Olist
sample, not a representative one: `DATA.md` says how it was drawn).

This page is the definition of every number in the deck. Where your colleague's notebook and this page disagree, this
page is right.

## Words used below

- An **order** is a row of `orders.csv`. Its **date** is `order_purchase_timestamp`. A **paid order** has at least one
  row in `order_payments.csv`.
- The **customer state** of an order is `customer_state` of its customer (`customers.csv`). The **seller state** of an
  item is `seller_state` of its seller (`sellers.csv`).
- Money is in Brazilian reais (BRL) unless the KPI says euros (EUR).
- **The conversion rule, for every euro figure:** an amount is converted at the average of the ECB's daily
  EUR-per-BRL rates in the calendar month of its order's date. The rates are in `data/raw/api/frankfurter_eur.json`;
  the notebook's supplied `rates` table already reads them the right way round.

## The six KPIs

1. **Monthly revenue, in BRL and in EUR.** Revenue is what customers paid: the sum of `payment_value`. One row per
   month; every paid order in exactly one month.
2. **Average order value by customer state.** Revenue divided by the number of paid orders, both over the paid orders
   placed by customers in that state.
3. **Average review score by customer state.** One review, one vote. A review is one `review_id`: a review that
   covers two orders is still one review, and its state is the state of the customer who placed those orders. Report
   the mean `review_score` and the number of reviews.
4. **Item sales by product category, in English.** Item sales are the sum of item `price`, freight excluded. Every
   item is in exactly one row. An item whose category has no English name keeps its Portuguese name, labelled
   `untranslated: <name>`; an item with no category is labelled `no category`. Before the table, list the categories
   that have no English name, with their item counts.
5. **Delivery time by customer state** *(new)*. The delivery time of an order is the whole days from its purchase to
   its delivery to the customer: `date_diff('day', order_purchase_timestamp, order_delivered_customer_date)`. Only
   orders whose `order_status` is `delivered` and that have a delivery date. Report the mean and the number of orders.
6. **Revenue by seller state, in EUR** *(new)*. Payments cannot be split between the sellers of one order, so here
   revenue is what each seller sold: the sum of item `price`, freight excluded, by seller state, converted to euros by
   the rule above. Report items, BRL and EUR.

## What the deck needs with every number

- **The reconciliation:** each KPI is checked against a number computed independently, by different code, using the
  identity that KPI allows. Money must agree to the cent.
- **No order without a rate**, and **no order counted twice**. The deck will be read by people who will add the
  columns up.
