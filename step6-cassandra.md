<!-- TOP -->
<div class="top">
  <img src="https://datastax-academy.github.io/katapod-shared-assets/images/ds-academy-logo.svg" />
  <div class="scenario-title-section">
    <span class="scenario-title">Linearizable Consistency and Lightweight Transactions in Apache Cassandra®</span>
    <span class="scenario-subtitle">ℹ️ For technical support, please contact us via <a href="mailto:aleksandr.volochnev@datastax.com">email</a> or <a href="https://dtsx.io/aleks">LinkedIn</a>.</span>
  </div>
</div>

<!-- NAVIGATION -->
<div id="navigation-top" class="navigation-top">
 <a href='command:katapod.loadPage?[{"step":"step5-cassandra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 6 of 9</span>
 <a href='command:katapod.loadPage?[{"step":"step7-cassandra"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Shipping and cancelling orders</div>

In our third example, we use lightweight transactions to implement rules for changing an order status:
- status can change to *shipped* only from *awaiting shipment*
- status can change to *cancelled* only from *awaiting payment* or *awaiting shipment*

This scenario assumes that our *dragonslayer* user has two orders in the system with statuses 
*awaiting payment* and *awaiting shipment*. While a shipping department attempts to ship the orders, the user 
attempts to cancel them. The order statuses should change to *cancelled* and *shipped*, respectively.  

✅ Create the table and two orders:
```
CREATE TABLE IF NOT EXISTS orders_by_user (
  username TEXT,
  order_id UUID,
  status TEXT,
  PRIMARY KEY ((username), order_id)
);

INSERT INTO orders_by_user (username, order_id, status) 
VALUES ('dragonslayer', f1fa2590-2d78-4b77-9710-95bdb45b7fa1, 'awaiting payment');
INSERT INTO orders_by_user (username, order_id, status) 
VALUES ('dragonslayer', c420d3a3-cecc-4c25-a7f8-ef28eb532969, 'awaiting shipment');
SELECT * FROM orders_by_user WHERE username = 'dragonslayer';
```

✅ Ship the orders: 
<details>
  <summary>Solution</summary>

```
UPDATE orders_by_user SET status = 'shipped' 
WHERE username = 'dragonslayer' 
  AND order_id = f1fa2590-2d78-4b77-9710-95bdb45b7fa1
IF status = 'awaiting shipment';
UPDATE orders_by_user SET status = 'shipped' 
WHERE username = 'dragonslayer' 
  AND order_id = c420d3a3-cecc-4c25-a7f8-ef28eb532969
IF status = 'awaiting shipment';
```

</details>

<br/>

✅ Cancel the orders:
<details>
  <summary>Solution</summary>

```
UPDATE orders_by_user 
SET status = 'cancelled' 
WHERE username = 'dragonslayer' 
  AND order_id = f1fa2590-2d78-4b77-9710-95bdb45b7fa1
IF status IN ('awaiting payment','awaiting shipment');
UPDATE orders_by_user 
SET status = 'cancelled' 
WHERE username = 'dragonslayer' 
  AND order_id = c420d3a3-cecc-4c25-a7f8-ef28eb532969
IF status IN ('awaiting payment','awaiting shipment');
```

</details>

<br/>

✅ Retrieve the orders:
```
SELECT * FROM orders_by_user
WHERE username = 'dragonslayer';
```

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step5-cassandra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step7-cassandra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

