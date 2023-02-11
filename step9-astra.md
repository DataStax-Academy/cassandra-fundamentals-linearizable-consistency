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
 <a href='command:katapod.loadPage?[{"step":"step8-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 9 of 9</span>
 <a href='command:katapod.loadPage?[{"step":"finish-astra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Lightweight transaction limitations</div>

In terms of performance, lightweight transactions can be *several times slower* than regular inserts, updates and deletes, 
which is the consequence of using the *Paxos* consensus protocol for the internal implementation. Therefore, 
you should only use lightweight transactions when it is absolutely necessary. 

You should be able to recognize potential *race conditions* and estimate *data contention*. 
In case of low data contention, when only a few transactions compete for access to the same data, 
using lightweight transactions to enforce linearizable consistency is a good choice. However, in case of high data contention, 
when many concurrent transactions compete for access to the same data, you may be better off 
changing your data model to reduce data contention or organize transactions into a time-ordered queue to ensure serial execution.

With respect to our previous examples, registering new users,
resetting user passwords, and shipping and cancelling orders clearly fall into the low data contention category.
The example of bidding on auction items can potentially have high data contention.

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step8-astra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"finish-astra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

