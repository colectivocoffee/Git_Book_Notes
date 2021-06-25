# System Design Notes

|  | sssss |
| :--- | :--- |


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
          <li>When interviewer gives you a Q:</li>
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
            shard)
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
        </ul>
        <p>
          <br /><b><code>@@@@ Scalability and Performance @@@@</code></b>
        </p>
        <ul>
          <li>Split data into chunks, we call it <b>nodes (= shards in SQL)</b>
          </li>
          <li><b><br /></b>
            <br />
            <br />
          </li>
        </ul>
        <p><b>&lt;code&gt;&lt;/code&gt;</b>
        </p>
        <p>ssss</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

![](../.gitbook/assets/sys_design_cluster_proxy.png)

* * aaa

