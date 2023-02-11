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
 <a href='command:katapod.loadPage?[{"step":"step4-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 5 of 9</span>
 <a href='command:katapod.loadPage?[{"step":"step6-astra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Resetting user passwords</div>

In our second example, we use lightweight transactions to reset a user password. This scenario 
assumes that the user with username *dragonslayer* requests to reset his password. Our application generates 
a random reset token that expires in 1 hour or 3600 seconds and can be used only once. A subsequent attempt to use 
the same token should fail.  

✅ Generate a password reset token: 
```
UPDATE users USING TTL 3600
SET reset_token = 6ef95fd0-9ae0-11ea-a9d2-d777ab7dec9e 
WHERE username = 'dragonslayer';

SELECT * FROM users
WHERE username = 'dragonslayer';
```

✅ Update the password: 
```
UPDATE users 
SET reset_token = null, password = 'encrypted password'
WHERE username = 'dragonslayer'
IF reset_token = 6ef95fd0-9ae0-11ea-a9d2-d777ab7dec9e;

UPDATE users 
SET reset_token = null, password = 'malicious password'
WHERE username = 'dragonslayer'
IF reset_token = 6ef95fd0-9ae0-11ea-a9d2-d777ab7dec9e;
```

✅ Retrieve the user information:
```
SELECT * FROM users
WHERE username = 'dragonslayer';
```

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step4-astra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step6-astra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

