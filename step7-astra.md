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
 <a href='command:katapod.loadPage?[{"step":"step6-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 7 of 9</span>
 <a href='command:katapod.loadPage?[{"step":"step8-astra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Bidding on auction items</div>

Our fourth example deals with transactions that place bids on auction items. This scenario demonstrates how 
two users, *dragonslayer* and *delossailor*, bid on the same item.

✅ Create the table and add the item:
```
CREATE TABLE IF NOT EXISTS auction_items (
  item_id TEXT,
  starting_bid DECIMAL,
  highest_bid DECIMAL,
  highest_bidder TEXT,
  PRIMARY KEY ((item_id))
);

INSERT INTO auction_items (item_id, starting_bid, highest_bid) 
VALUES ('ABC123', 10.00, 0.00);
SELECT * FROM auction_items WHERE item_id = 'ABC123';
```

✅ User *dragonslayer* bids $10.00: 
<details>
  <summary>Solution</summary>

```
UPDATE auction_items 
SET highest_bid = 10.00, highest_bidder = 'dragonslayer' 
WHERE item_id = 'ABC123'
IF starting_bid <= 10.00 AND highest_bid < 10.00;
SELECT * FROM auction_items WHERE item_id = 'ABC123';
```

</details>

<br/>

✅ User *delossailor* bids $10.00: 
<details>
  <summary>Solution</summary>

```
UPDATE auction_items 
SET highest_bid = 10.00, highest_bidder = 'delossailor' 
WHERE item_id = 'ABC123'
IF starting_bid <= 10.00 AND highest_bid < 10.00;
SELECT * FROM auction_items WHERE item_id = 'ABC123';
```

</details>

<br/>

✅ User *delossailor* bids $10.99: 
<details>
  <summary>Solution</summary>

```
UPDATE auction_items 
SET highest_bid = 10.99, highest_bidder = 'delossailor' 
WHERE item_id = 'ABC123'
IF starting_bid <= 10.99 AND highest_bid < 10.99;
SELECT * FROM auction_items WHERE item_id = 'ABC123';
```

</details>

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step6-astra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step8-astra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

