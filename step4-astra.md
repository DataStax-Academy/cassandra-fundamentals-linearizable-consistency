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
 <a href='command:katapod.loadPage?[{"step":"step3-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 4 of 9</span>
 <a href='command:katapod.loadPage?[{"step":"step5-astra"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Registering new users</div>

In our first example, we use lightweight transactions to register new users. This scenario 
demonstrates how two people, *Joe* and *Jen*, try to register new accounts using the same 
username *dragonslayer*. Only one registration should succeed.

✅ Create the table:
```
DROP TABLE IF EXISTS users;
CREATE TABLE users (
  username TEXT,
  email TEXT,
  name TEXT,
  password TEXT,
  reset_token UUID,
  PRIMARY KEY ((username))
);
```

✅ Register the users: 
```
INSERT INTO users (username, email, name) 
VALUES ('dragonslayer', 'joe@datastax.com', 'Joe')
IF NOT EXISTS;
INSERT INTO users (username, email, name) 
VALUES ('dragonslayer', 'jen@datastax.com', 'Jen')
IF NOT EXISTS;
```

✅ Retrieve the new account:
```
SELECT * FROM users
WHERE username = 'dragonslayer';
```

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step3-astra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step5-astra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

