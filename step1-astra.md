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
 <a href='command:katapod.loadPage?[{"step":"intro"}]' 
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 1 of 9</span>
 <a href='command:katapod.loadPage?[{"step":"step2-astra"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Linearizable consistency</div>

While *eventual consistency* is completely adequate for most real-life use cases 
and *tunable consistency* helps to customize consistency guarantees for specific application needs,
there are situations when this is still not enough. Even *strong consistency*, which guarantees that 
all acknowledged writes are visible to subsequent reads, does not help with *race conditions* 
when there are multiple transactions trying to read and write the same piece of data concurrently. Examples 
of race conditions include two users trying to register new accounts using the same username, or multiple 
auction participants placing bids on the same item and potentially overwriting each other's bids. The latter 
scenario is demonstrated in the following illustration:

<pre class="non-executable-code">
User 1: read(10)   10<20   write(20)
--------------------------------------------> time
User 2:         read(10)   10<15   write(15)

</pre> 

Both users run their transactions concurrently. Both read the current highest bid of `10` and both check that 
their new desired bids of `20` and `15` are higher than `10`. While the first user writes her bid of `20`, the unsuspecting second 
user writes her bid of `15`. The resulting highest bid value is `15` instead of `20`, which is incorrect.

The solution to this and other problems that involve race conditions is *linearizable consistency*, which ensures that
concurrent transactions produce the same result as if they execute in a sequence, one after another. For the 
previous example, linearizable consistency would result in the correct highest bid of `20`, as shown in these two possible 
execution sequences:


<pre class="non-executable-code">
read(10) 10<20 write(20)
--------------------------------------------> time
                    read(20) 20>15  
</pre>
<pre class="non-executable-code">
                    read(15) 15<20 write(20)
--------------------------------------------> time
read(10) 10<15 write(15)

</pre>

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"intro"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step2-astra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

