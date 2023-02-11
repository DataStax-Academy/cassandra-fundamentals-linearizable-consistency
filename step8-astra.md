<!-- TOP -->
<div class="top">
  <img class="scenario-academy-logo" src="https://datastax-academy.github.io/katapod-shared-assets/images/ds-academy-2023.svg" />
  <div class="scenario-title-section">
    <span class="scenario-title">Linearizable Consistency and Lightweight Transactions in Apache Cassandra®</span>
    <span class="scenario-subtitle">ℹ️ For technical support, please contact us via <a href="mailto:aleksandr.volochnev@datastax.com">email</a> or <a href="https://dtsx.io/aleks">LinkedIn</a>.</span>
  </div>
</div>

<!-- NAVIGATION -->
<div id="navigation-top" class="navigation-top">
 <a href='command:katapod.loadPage?[{"step":"step7-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 8 of 9</span>
 <a href='command:katapod.loadPage?[{"step":"step9-astra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Keeping the bidding history</div>

Our final and a bit more advanced example deals with transactions that place bids on auction items just like 
in the previous example. However, to make things even more realistic, we also keep the history of all placed bids, whether they were successful or not. 

✅ Create the table and add the item:
```
DROP TABLE IF EXISTS bids_by_item;
CREATE TABLE bids_by_item (
  item_id TEXT,
  bid_id TIMEUUID,
  bid DECIMAL,
  bidder TEXT,
  starting_bid DECIMAL STATIC,
  highest_bid DECIMAL STATIC,
  highest_bidder TEXT STATIC,
  PRIMARY KEY ((item_id), bid_id)
) WITH CLUSTERING ORDER BY (bid_id DESC);

INSERT INTO bids_by_item (item_id, bid_id, starting_bid, highest_bid) 
VALUES ('ABC123', NOW(), 10.00, 0.00);
SELECT * FROM bids_by_item WHERE item_id = 'ABC123';
```

✅ User *dragonslayer* bids $10.00: 
```
INSERT INTO bids_by_item (item_id, bid_id, bid, bidder) 
VALUES ('ABC123', NOW(), 10.00, 'dragonslayer');

UPDATE bids_by_item 
SET highest_bid = 10.00, highest_bidder = 'dragonslayer' 
WHERE item_id = 'ABC123'
IF starting_bid <= 10.00 AND highest_bid < 10.00;
SELECT * FROM bids_by_item WHERE item_id = 'ABC123';
```

✅ User *delossailor* bids $10.00: 
<details>
  <summary>Solution</summary>

```
INSERT INTO bids_by_item (item_id, bid_id, bid, bidder) 
VALUES ('ABC123', NOW(), 10.00, 'delossailor');

UPDATE bids_by_item 
SET highest_bid = 10.00, highest_bidder = 'delossailor' 
WHERE item_id = 'ABC123'
IF starting_bid <= 10.00 AND highest_bid < 10.00;
SELECT * FROM bids_by_item WHERE item_id = 'ABC123';
```

</details>

<br/>

✅ User *delossailor* bids $10.99: 
<details>
  <summary>Solution</summary>

```
INSERT INTO bids_by_item (item_id, bid_id, bid, bidder) 
VALUES ('ABC123', NOW(), 10.99, 'delossailor');

UPDATE bids_by_item 
SET highest_bid = 10.99, highest_bidder = 'delossailor' 
WHERE item_id = 'ABC123'
IF starting_bid <= 10.99 AND highest_bid < 10.99;
SELECT * FROM bids_by_item WHERE item_id = 'ABC123';
```

</details>

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step7-astra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step9-astra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

