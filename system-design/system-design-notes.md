# System Design Notes

## Data Flow Simulation  

![](../.gitbook/assets/sys_design_001_all.png)

## System Design Interview - Step by Step Guide

<table>
  <thead>
    <tr>
      <th style="text-align:left">Recall</th>
      <th style="text-align:left">Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>What is the most important</p>
        <p>component for a successful
          <br />system design interview?
          <br />
          <br />What should we focus on the most
          <br />while preparing for one?</p>
      </td>
      <td style="text-align:left">
        <p>===Chapter 1===</p>
        <ul>
          <li>When a interviewer gives you a Q:</li>
          <li>Then Ask Interviewers about 4 categories to
            <br />get requirement clarifications.
            <br />(1) Users/Customers
            <br />(2) Scale (Read/Write)
            <br />(3) Performance
            <br />(4) Cost
            <br />
          </li>
          <li>Why to things above?</li>
          <li>Get functional requirements &amp; non-functional requirements
            <br />(1) <b>Functional Requirements (what system will do)</b>:
            <br />system behavior, specific APIs,
            <br />set of operations that system will support
            <br />(2) <b>Non-Functional Requirements (what system is <br />supposed to be)</b>:
            <br
            />system qualities, such as fast, fault-tolerant, secure
            <br />
          </li>
          <li>
            <p>Functional Requirements - API
              <br />Below are the steps to generalize APIs
              <br />
              <br />e.g. The system has to <code>count</code>  <em>video</em> view <b>events. </b>
            </p>
            <p>--&gt; def <code>count</code>View<b>Event</b>(videoId):
              <br /># get eventType such as &apos;view&apos;, &apos;like&apos;, &apos;share&apos;</p>
            <p>--&gt; def <code>count</code><b>Event</b>(videoId, eventType):
              <br /># sum all different functions such as &apos;count&apos;, &apos;sum&apos;,
              &apos;average&apos;
              <br />--&gt; def process<b>Event</b>(videoId, eventType, function):
              <br /># generalize events, APIs
              <br />--&gt; def process<b>Event</b>s(listOfEvents)
              <br /># each event is an object which contains info about video,
              <br />type of the event, time of the event...
              <br />
              <br />e.g.2 Data Retrieval API
              <br />The system has to return <em>video</em> views count for a time period.</p>
            <p>--&gt; def getViewsCount(videoId, startTime, endTime)
              <br /># video views -&gt; &apos;likes&apos;, &apos;dislikes&apos;, &apos;view&apos;
              <br
              />--&gt; def getCount(videoId, eventType, startTime, endTime)
              <br /># get stats and functions to the method
              <br />--&gt; def getStats(videoId, eventType, function, startTime, endTime)
              <br
              />
            </p>
          </li>
          <li>
            <p>non-Functional Requirements</p>
            <p>Interviewer Reply: Let&apos;s design xxx at <b>scale, <br /></b>let&apos;s
              try to make it as <b>fast</b> as possible.
              <br />
              <br />--&gt; then we use CAP theorem
              <br />CAP theorem tells me we can&apos;t have <b>C</b>onsistency, <b>A</b>vailability,
              <br
              /><b>P</b>erformance/Partition Tolerance all at once. Then I&apos;ll
              <br
              />choose availability over consistency.
              <br />* <b>Scalable</b> (tens of thousands of video views per sec)
              <br />* <b>High Performant</b> (few tens of milliseconds to return
              <br />total views count for a video)
              <br />* <b>Highly Available</b> (survives hardware/network failures,
              <br />no single point of failure)
              <br />* Consistency
              <br />* Cost (hardware, development, maintenance)
              <br />
            </p>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>===Chapter 2 Drive the Conversation: What to Store===</p>
        <p></p>
        <ul>
          <li>starts with...</li>
          <li><b>Data</b>
            <br />-&gt; We need to think what data we want to store and how
            <br />-&gt; We need to define data model</li>
          <li>
            <p>What do we <b>store</b>?</p>
            <table>
              <thead>
                <tr>
                  <th style="text-align:left">Individual Events</th>
                  <th style="text-align:left">vs</th>
                  <th style="text-align:left">Aggregate Data (Batch Events)</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td style="text-align:left">Store <b>Raw Events</b>
                  </td>
                  <td style="text-align:left">Func</td>
                  <td style="text-align:left"><b>Aggregate</b> data in real-time</td>
                </tr>
                <tr>
                  <td style="text-align:left"><b>Stream</b> Data Processing</td>
                  <td style="text-align:left"></td>
                  <td style="text-align:left"><b>Batch</b> Data Processing</td>
                </tr>
                <tr>
                  <td style="text-align:left">
                    <p>*fast <b>writes</b>
                    </p>
                    <p>*can slice however we need</p>
                    <p>*can recalculate numbers if needed</p>
                  </td>
                  <td style="text-align:left">Pros</td>
                  <td style="text-align:left">
                    <p>*fast <b>reads</b>
                    </p>
                    <p>*data is ready for decision making</p>
                  </td>
                </tr>
                <tr>
                  <td style="text-align:left">
                    <p>*slow reads</p>
                    <p>*<b>Costly </b>for large scale</p>
                    <p>(many events to be stored)</p>
                  </td>
                  <td style="text-align:left">Cons</td>
                  <td style="text-align:left">
                    <p>*can <b>only query</b> the way it was <b>aggregated</b>
                    </p>
                    <p>*<b>requires</b>  <b>data aggregation</b> pipeline (Hard!)</p>
                    <p>*hard or even impossible to
                      <br />fix errors</p>
                  </td>
                </tr>
              </tbody>
            </table>
          </li>
          <li>Then ask interviewer <b>expected delay. <br />Time between when event happened &lt;--&gt; when event was processed<br /><br /></b>(1)<b> </b>if
            expected delay is<b> less than several mins<br /> </b>--&gt; aggregate
            data on the fly<b><br /></b>(2) if expected delay is<b> okay for several hours <br /> </b>--&gt;
            store raw events and process them in the background
            <br />(3) <b>combine both</b> approach, store raw events and batch process data
            <br
            />--&gt; Flexibility up
            <br />--&gt; <b>Raw Data:</b> store only several days/weeks raw data + purge old
            data
            <br /> <b>Batch Data</b>: store aggregated data (view counts) in real-time
            <br
            />--&gt; Costly, expensive</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>===Chapter 3 === Database</p>
        <p></p>
        <ul>
          <li>Can you give me a specific DB name? Explain your choice.</li>
          <li>Both SQL and NoSQL DB can scale and perform well, let me evaluate both
            types.</li>
          <li>Ask questions below</li>
          <li>
            <p></p>
            <table>
              <thead>
                <tr>
                  <th style="text-align:left">
                    <ul>
                      <li>How to scale writes?</li>
                      <li>How to scale reads?</li>
                      <li>How to make both writes and reads fast?</li>
                      <li>How not losing data in case of hardware faults and network partitions?</li>
                      <li>How to recover data in case of an outage?</li>
                      <li>How to ensure data security?</li>
                      <li>How to make it extensible for data model changes in the future?</li>
                    </ul>
                  </th>
                </tr>
              </thead>
              <tbody></tbody>
            </table>
            <table>
              <thead>
                <tr>
                  <th style="text-align:left"></th>
                </tr>
              </thead>
              <tbody></tbody>
            </table>
          </li>
          <li>============================================
            <br />How SQL handle these requirements--- Step by Step</li>
        </ul>
        <p><b><code>@@@@ Scalability and Performance @@@@</code></b>
        </p>
        <ul>
          <li>How to reduce the load on a single SQL machine?</li>
          <li><b>(1)</b>  <b>Add</b>  <b>Sharding DB</b> (several SQL DBs work together)</li>
        </ul>
        <p></p>
        <ul>
          <li>How to route the traffic better?</li>
          <li><b>(2) Add Cluster Proxy</b> (a proxy machine and route traffic to correct
            shard)</li>
          <li>Cluster proxy is the only one who knew about all shards info.
            <br />
            <br />
            <img src="../.gitbook/assets/sys_design_cluster_proxy (1).png" alt/>
            <br />
          </li>
          <li>How do we make cluster proxy be aware of newly added DB?</li>
          <li><b>(3)</b>  <b>Add Configuration Service </b>(to maintain health check)
            <br
            />-&gt; config service maintains a health check connection to all shards)
            <br
            />-&gt; config service always know which DB is available
            <br />
            <br />
            <img src="../.gitbook/assets/sys_design_db2_config_service.png" alt/>
            <br />
          </li>
          <li>Instead of calling shard DB instance directly, we add shard proxy
            <br />(4) <b>Add Shard Proxy</b> (a DB helper to add more useful functions)
            <br
            />--&gt; 1. shard proxy can cache query results
            <br />--&gt; 2. monitor DB instance health
            <br />--&gt; 3. publish metrics
            <br />--&gt; 4. terminate query that takes too long</li>
        </ul>
        <p>
          <img src="../.gitbook/assets/sys_design_db3_shard_proxy.png" alt/>
          <br />
          <br />
          <br /><b><code>@@@@ Availability @@@@</code></b>
        </p>
        <ul>
          <li>What if DB shard died?</li>
          <li>How to ensure data is not lost?
            <br />
          </li>
          <li>(5) Add Replicas between different data center
            <br />--&gt; [<b>Write]</b> when the cluster proxy sends data to a shard(DB),
            <br
            />--&gt; data is sync or async replicated to corresponding read replica.
            <br
            />--&gt; <b>[Read]</b> when the cluster proxy retrieve data from a shard,
            <br
            />--&gt; data will be retrieved either from master or read replica.
            <br />
            <img src="../.gitbook/assets/sys_design_db4_replica.png" alt/>
            <br />
          </li>
          <li>e.g. Youtube built a DB solution to scale and manage large clusters
            <br
            />of MySQL instance, called Vitess.
            <br />
            <br />
          </li>
          <li>=============================================</li>
          <li>How NoSQL handle these requirements --- Step by Step</li>
          <li>NoSQL DB (Cassandra)</li>
        </ul>
        <p>
          <br /><b><code>@@@@ Scalability and Performance @@@@</code></b>
        </p>
        <ul>
          <li>Split data into chunks, we call it <b>nodes (= shards in SQL)</b>
          </li>
          <li>We don&apos;t need config service to manage each node, instead, we
            <br
            />let nodes talk to each other and exchange information about
            <br />their state.</li>
          <li>Every sec node exchanges information with a few other nodes
            <br />(less than 3). State information about every node propagates
            <br />throughout the cluster -- <b>gossip protocol</b>.
            <br />This way, we also don&apos;t need cluster proxy anymore.</li>
        </ul>
        <p><b> </b>
          <img src="../.gitbook/assets/sys_design_db5_nosql_routing.png"
          alt/>
        </p>
        <ul>
          <li>NoSQL Routing Process</li>
          <li>e.g. Processing service is asking us to store views count B, then
            <br />Node4 has been selected. Node4 served as the coordinator node.
            <br />which node do we store this view count B?
            <br />(1) Round Robin Algorithm
            <br />(2) Consistent Hashing Algorithm: choose a node that
            <br />is closest to the client
            <br />
          </li>
          <li><b>###Consistent Hashing###</b>
          </li>
          <li><b>Data Replication Using Quorum(&#x6CD5;&#x5B9A;&#x4EBA;&#x6578;)</b>
          </li>
          <li>Why? Because synchronous data replication is slow. We usually
            <br />replicate data asynchronously. <b>  <br /> </b>
          </li>
          <li><b>Quorum Writes: </b>sends a &apos;successful&apos; message while 2 out
            of 3
            <br />(not all) of replicas are successfully stored.<b><br /><br /></b>A coordinator
            node (node4) calls multiple nodes
            <br />to replicate data, to store multiple copies(3 copies) of data.
            <br />However, waiting for all (3 responses) from replicas maybe
            <br />too slow, we can send a &apos;successful&apos; message once some
            <br />(2 requests) succeeded.</li>
        </ul>
        <p><b> </b>
          <img src="../.gitbook/assets/sys_deign_db6_nosql_readquorum.png"
          alt/>
        </p>
        <ul>
          <li><b>Quorum Read</b>: read quorum defines a minimum number of nodes
            <br />that have to agree on the response.</li>
          <li>Cassandra uses version number technique to determine the staleness of
            data.<b><br /><br /></b>
          </li>
          <li>For non-functional requirements, in this case, we choose
            <br /><b>Availability over Consistency<br /></b>--&gt; We prefer to show stale
            data over no data at all.</li>
          <li>For the case of <b>Leader-Follower replication</b>, some read replicas
            <br
            />may be behind their master.
            <br />--&gt; Different user would see <em>different view count</em> for a video.
            <br
            />--&gt; But this is temporary, this situation will be resolved over time.
            <br
            />--&gt; we call it <b>Eventual Consistency.<br /></b>--&gt; Cassandra -
            tunable consistency
            <br />
            <br />
          </li>
          <li>======How to Store Data=======</li>
          <li><b>relational database, SQL</b>
            <br />How do we store data in relational DB?
            <br />First, we need to design Data Model, with the following steps</li>
          <li>
            <p>
              <img src="../.gitbook/assets/sys_deign_db7_nosql_store.png" alt/>
            </p>
            <table>
              <thead>
                <tr>
                  <th style="text-align:left">
                    <ul>
                      <li>
                        <p>relational DB start with Nouns</p>
                        <p>1) <b>Define Nouns</b>: we start with nouns in the system<b> </b>
                          <br />2) <b>Convert Nouns to Tables</b>:
                          <br />3) <b>Use Foreign Keys to reference related data</b>:
                          <br />4) Normalization: to m<b>inimize data duplication across different tables <br /></b>e.g.
                          only store video name in video info table.
                          <br />To minimize changes across different tables -&gt; cuz
                          <br />inconsistent data is possible.
                          <br />5)</p>
                      </li>
                    </ul>
                  </th>
                </tr>
              </thead>
              <tbody></tbody>
            </table>
          </li>
          <li><b>NoSQL</b>
            <br />How do we store data in NoSQL DB?</li>
          <li>NoSQL starts with <b>Queries</b>
          </li>
        </ul>
        <p>
          <img src="../.gitbook/assets/sys_deign_db8_nosql_query.png" alt/>
        </p>
        <ul>
          <li>There are 4 types of NoSQL DB:
            <br />(1) Column
            <br />(2) Document
            <br />(3) Key-Value
            <br />(4) Graph
            <br />
          </li>
          <li><b>About Cassandra DB</b>
          </li>
          <li>Cassandra DB&apos;s benefit:
            <br />* fault-tolerant
            <br />* scalable (both read and write throughput increases linearly as new
            <br
            />* machine is added)
            <br />* supports multi-datacenter replication
            <br />* works well with time-series data</li>
          <li>Cassandra is <b>Column DB</b> that supports asynchronous
            <br />masterless replication.
            <br />
          </li>
          <li><b>About HBase</b>
          </li>
          <li>HBase is Column DB, similar to Cassandra, but it has a master-based
            <br
            />architecture
            <br />
          </li>
          <li><b>About MongoDB</b>
          </li>
          <li>MongoDB is <b>Documented-oriented DB</b>, uses leader-based replication
            <br
            />
          </li>
        </ul>
        <p>ssss</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>==== Chapter 4 ==== Data Processing</p>
        <p></p>
        <ul>
          <li>starts with... requirements</li>
          <li>Requirements
            <br />1) we want to scale the processing service to scale together when
            <br />the video views increase. &lt;-- scalable
            <br />2) we want to process events quickly. &lt;-- performance/fast
            <br />3) we don&apos;t want to lose data &lt;-- availability/reliable
            <br />
          </li>
          <li>The above questions turn into statements below:</li>
          <li>How to <b>scale</b>?</li>
          <li>How to achieve <b>high throughput</b>?</li>
          <li>How to <b>not losing data</b> when processing node crashes?</li>
          <li>What to do when DB is <b>unavailable</b> or slow?
            <br />--&gt; How to make data processing <b>scalable, fast, and reliable?</b>
          </li>
        </ul>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <ul>
          <li><b>Scalable == partitioning</b>
          </li>
          <li><b>Fast == in-memory &amp; minimize disk reads</b>
          </li>
          <li><b>Reliable == Replication and checkpointing</b>
          </li>
        </ul>
        <p><b><br />%%%% Data Aggregation Basics %%%%</b>
        </p>
        <p><code>@@@ </code><b><code>Should we aggregate data first?</code></b><code> @@@</code>
        </p>
        <ul>
          <li><b> </b>
            <img src="../.gitbook/assets/sys_deign_process1_data_aggregation.png"
            alt/>
          </li>
          <li>1st option, increment counter by 1 when an event comes (+1 +1 +1)</li>
          <li>2nd option, accumulate data in the processing service memory for
            <br />several seconds. And add accumulated value to the DB counter. (+3)</li>
        </ul>
        <p><code>@@@ </code><b><code>Push or Pull?</code></b><code> @@@</code>
        </p>
        <ul>
          <li><b>Push &amp; Pull &#x600E;&#x9EBC;&#x770B;&#xFF1F;<br />&#x90FD;&#x662F;&#x4EE5;Processing Service&#x70BA;&#x57FA;&#x6E96;&#xFF0C;either push&#x5230;PS&#xFF0C;<br />&#x6216;&#x662F;pull&#x56DE;PS<br /><br /> </b>
            <img
            src="../.gitbook/assets/sys_deign_process2_push_pull.png" alt/>
          </li>
          <li><b>Push</b>: some other service sends events synchronously to the
            <br />processing service
            <br />
            <br />[serviceA] -- event --&gt; [processing service]
            <br />
          </li>
          <li><b>[v] Pull</b>: the processing service pulls events from some temp storage.
            <br
            />
            <br />[]
            <br />
          </li>
          <li><b>Pull option has more advantages</b>, as it provides a better fault
            <br
            />tolerance support and easier to scale.
            <br />
            <br />Why? Because if processing service crashes, we still have events in
            <br
            />the storage and can re-process them.
            <br />
            <br />
          </li>
          <li>How data aggregation happens during event processing?</li>
          <li>Checkpointing &amp; Partitioning</li>
        </ul>
        <p>
          <img src="../.gitbook/assets/sys_deign_ps3_data_aggregation.png" alt/>
        </p>
        <ul>
          <li><b>++ Checkpointing ++</b>
          </li>
          <li>Checkpointing is a technique to <b>add fault-tolerance into the system</b>.
            <br
            />It consists of saving a snapshot of the application&apos;s state, so it
            can
            <br />restart at the point where it failed. It happens in the queue.</li>
          <li>After we processed several events and successfully stored them in
            <br />the DB, we write checkpoint to some persistent storage.
            <br />If processing service machine fails, it will be replaced with another
            <br
            />one and this new machine will resume processing where the failed
            <br />machine left off. (key in stream data processing)
            <br />
          </li>
          <li><b>++ partitioning ++</b>
          </li>
          <li>Partitioning also happens in the queue.</li>
          <li>Each queue is independent from the others. Every queue physically
            <br />lives on its own machine and stores a subset of all events
            <br />(AA / BBB / C)
            <br />We compute a hash based on video identifier and use this hash number to
            pick a queue.</li>
          <li>Partitioning allows us to <b>parallelize events processing</b>. More events
            <br
            />we get, more partitions we create.</li>
        </ul>
        <p></p>
        <p>
          <img src="../.gitbook/assets/sys_deign_ps4_ps_design.png" alt/>
        </p>
        <p>@@@ Processing Service (Detailed Design) @@@</p>
        <ul>
          <li><b>Partition Consumer + De-duplication Cache</b>:
            <br />* <b>Partition Consumer</b>: we need a component to read events.
            <br />from byte array and turn it to actual object.
            <br />Here we <b>prefer single thread</b> to read data. Otherwise, if we use
            <br
            />multi-threaded, checkpointing will be much harder to ensure order.
            <br
            />* <b>De-duplication Cache</b>: to <b>avoid double counting</b>. Messages
            that
            <br />get send to partition/shard could have duplicates. Meaning same
            <br />message could be sent multiple times. Then we need de-duplication
            <br />cache.
            <br />This distributed cache <b>stores unique event identifiers</b>(uniq event
            id)
            <br />for last 10 mins. Then if several identical messages arrived within
            <br
            />10 mins interval, only one of them(first one) would be processed.
            <br />
            <br />
          </li>
          <li><b>Aggregator + In-memory Store</b>:
            <br />* <b>Aggregator (passport check officer, consumption)</b>:
            <br />It does in-memory counting.
            <br />[hash table1] --&gt; [hash table2] --&gt; [hash table3] ...
            <br />1 1 1 1 1(stop) -&gt; 1 1 1 +1...
            <br />While adding hash table2,
            <br />The hash table1(complete) was sent to internal queue.
            <br />
            <br />A <b>hash table</b> that accumulates data for some period of time.
            <br />This hash table is stored in the in-memory store.
            <br />Periodically, we stop writing to the current hash table and create
            <br
            />a new one. Then a new hash table keeps accumulating incoming
            <br />data. While old hash table is no longer counting any data and each
            <br
            />counter from old hash table is sent to the internal queue for furhter
            <br
            />processing.
            <br />
            <br />Why aggregator is connected with Internal Queue?
            <br />Ans: To speed up processing, with multiple threads.
            <br />We can also do the other way around, where queue -&gt; aggregator.
            <br
            />Especially if processing data takes time. By sending data to the
            <br />internal queue, <b>we decouple consumption and processing</b>.
            <br />
          </li>
          <li><b>Internal Queue (multi-lines luggage check, processing)</b>:
            <br />Internal queue speeds up processing with mulitple threads.
            <br />Processing data takes longer time.
            <br />
          </li>
          <li><b>Database Writer + Embedded DB + Dead-Letter Queue</b>:
            <br />* <b>DB writer</b> can be either single thread or multi-threaded.
            <br />Single thread: <b>checkpointing is easy</b> / but slow
            <br />Multi threaded: checkpointing is hard / but <b>increases throughput</b>.
            <br
            />
            <br />[Dead-Letter Queue is useful]
            <br />How do we handle undelivered data?
            <br />* <b>(1)</b>  <b>Dead-Letter Queue</b>: To temporary store data when data
            cannot
            <br />be routed to their destination.
            <br />DLQ Pros: To protect DB performance or availability issues.
            <br />Very useful when we need to preserve data incase of downstream
            <br />services degradation. (availiability up)
            <br />* <b>(2) Local Disk Storage / Data Enrichment</b>:
            <br />To temporary store undelivered messages. Minimal information, like
            <br
            />video identifier and timestamp. We don&apos;t need to store video titile
            or
            <br />channel name or video creation date.
            <br />* <b>Embedded DB</b>:
            <br />To store additional attributes, like video title or channel name.
            <br />Embedded DB is on local machine, which eliminates a need for
            <br />remote calls.
            <br />
          </li>
          <li><b>State Store (Recover from prev un-lost state)</b>:
            <br />State management. To periodically save the entire in-memory data
            <br />to a durable storage. It helps to recover when PS is down, and then
            <br
            />embedded DB/in-memory state is lost.
            <br />If breakdown happends, just re-create the point where it failed.
            <br />A new machine just have to re-load this state into its memory
            <br />when started.</li>
        </ul>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p></p>
      </td>
    </tr>
  </tbody>
</table>

## === Ch5 === Data Ingestion

#### Data Ingestion Path



![](../.gitbook/assets/sys_design_di1_data_ingestion_path.png)

## Ingestion Path Components

| Partitioner Service Client/API Gateway | Load Balancer | Partitioner Service and Partitions |
| :--- | :--- | :--- |
| Blocking vs non-blocking I/O | Software vs hardware balancing | Partition Strategy |
| Buffering and batching | Networking Protocols | Service Discovery |
| Timeouts | Load balancing algorithms | Replication |
| Retrics | DNS | Message Format |
| Exponential backoff and jitter | Health Checking |  |
| Circuit breaker | High Availiability |  |

### API Gateway/Partitioner Service Client

* **Blocking vs non-blocking I/O**:

  **Blocking I/O  `(one socket per connection style)`**

  \(1\) One thread per connection. New client, new thread, new connection.  
  \(2\) Initiated the connection by using sockets.   
  \(3\) \[Single execution thread\] When a client makes a request, the socket that handles the connection on the server is blocked. We can then track progress of the request. \(Easy to debug\)  
  \(4\) We can use thread local variables in blocking I/O. When issue happens, exception pops up the stack and it is easy to catch them.   
  \(5\) Pros: Not complex, **easy to debug**.  
       Cons: **Not efficient**    
  \(6\) When server starts to experience a slow down, the whole cluster of machines may die \(一起掛）  
  \(7\) How to solve this? We need **Rate Limiter**. 

  * **Rate Limiter**: Help to keep system stable during traffic peeks.   

  **Non-Blocking I/O `(Queue Style)`**  
  \(1\) Single thread to handle multiple concurrent connections.   
  \(2\) Server just **queues** the request and the actual I/O is then processed at some later point.  
  \(3\) Piling up requests in queue are far less expensive than piling up threads.   
  \(4\) Pros: **More efficient** and has **higher throughput**.    
       ****Cons: **Adding more complexity** of operations.  
  

* **Buffering and Batching `(send group events in one shot)` Batching** \(send group events together in a single request\) **Buffer** \(a place to put grouped events\) \(1\) Instead of sending each event individually, we first put events into a buffer.  \(2\) We then wait up to several seconds before buffer's content or until batch fills up.  \(3\) Pros: **Increases throughput**, **save on cost**, **effective request compression**       Cons: **More complex** both on client and server side. \(4\) e.g. In a batch request, some events fail, while other succeed.       Should we resend whole batch or only failed events? 
* **Timeouts `(wait until xx time and leave)`**  
  \(1\) Timeouts define how much time a client is willing to wait for a response from a server.

  * **Connection Timeout:  \*** connection timeout defines how much time a client is willing to wait for a connection to establish. \* usually connection timeout is small, 10/milliseconds. Because there's no heavy request happening just yet.
  * **Request Timeout: \*** request timeout happens when request processing takes too much time, and a client is not willing to wait any longer.  \* How to choose request timeout value? We need to **analyze latency percentiles**.  e.g. latency **1% of the slowest requests** in the system and set it as req timeout. 

* **Retries** **`(what to do with failed requests? Use retries, try again.)`** \(1\) Timeout is probably due to a bad machine was hitted. The second attempt may hit a different server machine. \(2\) Retry smart. We **shouldn't do all retries at the same time**. We may create a so-called  **retry storm event** and overload server with too many requests.  
* **Exponential Backoff and Jitter Algorithms \(how not to do all retries at the same time?\)**  
  \(1\) To prevent all retries start at the same time, we need exponential backoff and jitter algorithms. 

  * **Exponential Backoff:**  Exponential backoff increases the waiting time between retries up to a maximum backoff time.  e.g. 1s -&gt; 2s -&gt; 5s -&gt; 10s -&gt; 30s -&gt; 1min -&gt; 5min -&gt; 10min
  * **Jitter:** Jitter **adds randomness** **to retry intervals to spread out the load**. If we do not add jitter, exponential backoff algorithm will retry requests at the same time. Jitter helps to separate the retries. 

  \(2\)  Be aware of too many retries.   
         e.g.  when partitioner service is down or degraded, then retries would never succeed.    

* **Circuit Breaker `(stops unlimited retries with a threshold)`**   
  \(1\) Circuit breaker pattern stops a client from repeatedly trying to execute an operation that is likely to fail.  
  \(2\) We simply calculate how many requests have failed recently. If error threshold is exceeded, we stop calling a downstream service.   
  \(3\) **\[Retry with limited requests\]** Somtime later, limited number of requests from the client are allowed to pass through and invoke the operation. If requests are successful, it is assumed that the fault has been fixed.  
  \(4\) **\[Success, then allow all\]** Then we allow all requests at this point and start counting failed requests from scratch \(start over again.\) 

  \(5\) Pros: Have a threshold to stop requests.   
       Cons: **Hard to test the system,** Error **threshold** and timers are **hard to set** 

### Load Balancer

### Partitioner Service



* * aaa
* 
