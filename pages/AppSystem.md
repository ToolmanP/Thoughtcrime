## Overview
- General Principle: more maintainable, more extensible, faster and less prone to single-point failure
  logseq.order-list-type:: number
- ---
- ## Stateful System
- HTTP protocol is a stateless protocol to communicate.
  logseq.order-list-type:: number
- To make sure that the state can be stored across multiple HTTP requests, HTTP Session is introduced and passed by each HTTP response from the server in the presentation of COOKIES.
  logseq.order-list-type:: number
- If we want to design a service, stateless service is clearly better for
  logseq.order-list-type:: number
	- No extra overhead for storing the states.
	  logseq.order-list-type:: number
	- Better extensibility since we don't need to maintain the session across the machines.
	  logseq.order-list-type:: number
	- Less complexity.
	  logseq.order-list-type:: number
- Spring Session Type
  logseq.order-list-type:: number
	- Singleton  --- always one instance in one process
	  logseq.order-list-type:: number
	- Prototype --- one method call one instance
	  logseq.order-list-type:: number
	- Session --- one session one instance
	  logseq.order-list-type:: number
	- Request --- one request one instance
	  logseq.order-list-type:: number
- Database Connection Pool size
  logseq.order-list-type:: number
	- Can not be too large since it will over consume the system's resources and dispatching them requires a lot of context switches. Can not be too small since it may not be enough to feed the requests
	  logseq.order-list-type:: number
	- Empirical formula: connections = ((core_count * 2) + effective_spindle_count (effective concurrent IO count))
	  logseq.order-list-type:: number
	- Core_counts *2 is the total number of capable threads handling and effective_spindle_count can be deemed as a IO buffer and even a bottle neck. 
	  logseq.order-list-type:: number
	- We don't need a really large connection pool. (Instance pool)
	  logseq.order-list-type:: number
	- Instance Pool Management is a crucial part of application system design.
	  logseq.order-list-type:: number
- ---
- ## Message Queue
- ### Shortcomings of a synchronous client-server model
- Deeply coupled.
  logseq.order-list-type:: number
- No delivery guaranteed
  logseq.order-list-type:: number
- No request without a request buffer. (preventing the a large overflow of data to overwhelm the whole system and consorting to a chain of timeout)
  logseq.order-list-type:: number
- Too much emphasis on requests and responses
  logseq.order-list-type:: number
- Communication is not replayable.
  logseq.order-list-type:: number
-
- Message Queue acts a meditator mode in the programming modal in the system design.
- Message Queue introduces a message buffer between the consumers and producers, it however adds another layer of complexity over the system. However, at the end of day, its overhead still is beaten by its architectural benefit. We can now program based on a message oriented model.
- However, message queue does not provide a direct way to expose the exception to the application. E.g. when the bookstore is out of stock, the message can not be delivered immediately to their clients.
- Kafka uses logs to track the ordered sequence of events and is a stream based server.
- Kafka allows a partition of streams and can be deemed as a partitioned DB-like service to provide better concurrency
- ---
- ## WebSocket
- Sometime we do not need synchronous communication and if our requests are handled asynchronously, we naively require a method to receive asynchronous response.
- WebSocket is an application protocol that provides full-duplex communications between two peers over the TCP Protocol.
- It consists two parts: handshake and data transfer.
- Although full-duplex, it still cost more resource so maybe too many websocket connections are also intolerable. We need to control them.
- ### HandShake
- HTTP Based
  logseq.order-list-type:: number
- Use bidirectional keys to authenticate
  logseq.order-list-type:: number
- logseq.order-list-type:: number
  ---
- ## Transaction Management
- ### Container
- In application architecture, a container is the runtime for proxying all the resources needed for a controller and its service to responce.
- For example, if a controller needs a transaction support, the container will supercede it and handle all the necessary outbound requirements. e.g. instance selection, transaction management etc.
- ### Transaction Policy
- Required  --- joining the transaction created by the outer scope of the current control flow
  logseq.order-list-type:: number
- Require New --- always create a new transaction when doing a data operation
  logseq.order-list-type:: number
- Mandatory -- must have a transaction when invoking this method. If not, throw an exception at the given point.
  logseq.order-list-type:: number
- ### Isolation Policy
- If we do not set correct isolation level policy, there might be some problems and discrepancies.
- Isolation level describes how two or more transactions can interfere each other
- Dirty Read Problem --- Reading an scheduled-to-modify data in another transaction when the current ongoing transaction on modifying this data has not been commited yet. (Disable Read Uncommited)
  logseq.order-list-type:: number
- Non Repeatable Reads --- Modifying a scheduled to read data in another transaction when the current ongoing transaction wants to read the data twice yielding different results on different tries. (Enable REPEATABLE READ)
  logseq.order-list-type:: number
- Phantom Reads --- Adding a new row to the table when the current transaction wants to list the data in the table for multiple time (SERIALIZABLE)
  logseq.order-list-type:: number
- ### Snapshot
- Some database takes a snapshot of the current view of the data.
- It can prevent any dirty reads, nonrepeatable reads and phantom reads.
- But it requires a lot of spaces and resources for generating the snapshots.
- ---
- ## Concurrency Control
- ### Optimistic Concurrency Control
- We insert a version control number and a last-modified time in the row of the table and abort and try the changes when the version has changed when writing back.
- ### Pessimistic Concurrency Control
- Simply by locking the whole row/table and do not allow interleaved writing.
- ### Virtual Thread
- Java thread enables the program to bind a runnable object to a thread object and virtual thread can enable temporarily release the thread and executes other codes when waiting for I/O resource just like Golang.  In short, a virtual thread in its lifetime isn't necessary tied to a specific OS thread.
- ### Live Lock
- Two threads are busy and waiting for each other, however unlike dead locks they are not blocked.
- ### Immutable
- Immutable objects are great for multithreaded applications since their fields can not be changed and cannot
- ### Concurrent Collections
- ---
- ## Caching
- Everything needs caching to boost IO intensive application.
- We sometimes need distributed caching to serve backend, cluster or even frontend applications such that we can skip extra data processing procedure.
- Caching Policy
	- Ring Buffer + eviction + LRU
	  logseq.order-list-type:: number
	- persist and eviction.
	  logseq.order-list-type:: number
- Some strategy: offloading those frequently accessed data into redis. (Redis also needs disk space)
- We read from the cache first  and if we want to modify the data and evict the cache.
- Still we prefer to prepare caching for read-only data instead of frequently modified data. Since we don't want introduce a lot of cache miss.
- Caching also provide better locality and provide more efficient memory indexing and searching.
- Memcached introduces the concept of slabs. It defines a series of slabs of variant size however it causes internal fragmentation.
- Memcached distributed the data using the algorithm of consistent hashing to decide which node should store the data.
- Consistent hashing should survive a lot scenerios. e.g. server shutdown, leaving cluster, a server joining the cluster so on without the loss of availability. Consistent hashing only impacts only its neighbor and only import/export the data from/to its neighbor.
- Cache even though it needs IO persistence, still it's magnificently faster than regular DBMS
- ---
- ## Full-text Searching
- Suppose we have unstructured data and we want the context of the words we request to search.
- Inverted Indexing: document the location of the word inside a certain passage and in a certain order.
- #+BEGIN_NOTE
  Still if the key is not present in the passage, how can we find a related passage and feed it back to the user.
  #+END_NOTE
- ### Lucene
- Indexing based on reverse method, word -> passage + conext location
- A format supported rapid search
- We can use separated indexing fields for different aspect we may be interested in. e.g. one for title, another for the title of the passage
- Each focus of the index is defined as the field and a file can have many fields e.g. title, subjects,content
- #+BEGIN_IMPORTANT
  Another idea is that we only create index for those less-frequent keywords so that the index will be more efficient and accurate.
  #+END_IMPORTANT
- Some fields can be vectorized and be stored in the index so that it can be presented to the user immediately for user experience.  So we can choose which field is stored or indexed or simply discarded.
- We have a global table documenting the word-file location and subtables documenting word-context location in specific file.
- If required words are not present in the keywords list, we need to vectorize it and implement a similarity search using the algorithm of cosine similarity.
- An unstructured chunk of data consists of different terms coming from different fields.  Fields may be stored and indexed, fields contain terms and term may be tokenized to generate index or may be unified as a whole to provide index.
- An index may be segmented and each field goes into its own segment.
-
- ---
- ## Web Service
- Web services build service upon  web protocols providing better and reliable accessibility and interoperability.
- Service means that the application is behind the implementation and only exposes  a bunch of APIs or RPCs across the Internet. This MUST be heterogeneous and hardware/language/operating system independent.
- Web Protocol is text-oriented and circulated on the Internet.
- ### SOAP
- General: Method -> Text
- SOAP refers to Simple Object Access Protocol. Object is defined in a specific convention and  organized in XML.
- It describes the text's location, object type and the service URI.
- WSDL is language independent and the language can generate the implementation of a prototype interface based on the WSDL
- ### WSDL
- WSDL stands for web service description language.
- WSDL defines the method and its return type in XML. This is a good way to provide RPC or API definition.
- ###  Shortcomings of SOAP
- coupling with the message format
  logseq.order-list-type:: number
- coupling with the encoding of WS and attachment
  logseq.order-list-type:: number
- parse and assemble SOAP (extra overhead)
  logseq.order-list-type:: number
- need a wsdl to describe the details of WS
  logseq.order-list-type:: number
- need a proxy generated from WSDL
  logseq.order-list-type:: number
- ### REST
- General: Data + Convention -> Restful
- Data Driven, Decoupling the communication model between frontend and backend.
- REST stands for representational state transfer which represents the data object and its layout in a common form.
- Each resource can have different representations.
- Each resource can have its own unique identity.
- State is not maintained by the server.
- The representation of resource is a state of client.
- Client's representation will be transferred when client access different resource.  The state of the client changes according to different representation of resources.
- #### Definition
- REST is a typical Client-Server architecture with a stateless server.
- All states are hold in the messages delivered between clients and server.
- Server only process the requirements of data but its displacement depends on clients.
- REST is idiomepent and the client should have the ability to display the resource. The resource is self described
- We only transfer the data not the method definition!
- #### Design rules
- ![image.png](../assets/image_1703853699290_0.png)
- ![image.png](../assets/image_1703853839914_0.png)
- HTTP designs the type of action and url defines the resource the client is interested in.
- CRUD -> GET/POST/PUT/DELETE
- Resource -> URI
- ![image.png](../assets/image_1703854017511_0.png)
- ### When
- Support communication across firewall
- Support application integration
- Support B2B integration
- Encourage the reusability of the code
- Not stand-alone applications
- Not Homogeneous applications in LAN.
- ---
- ## Microservice
- ### Concept
- Building small, self-contained applications and services which can bring great flexibility and added resilience to the code.
- Domain Driven Design helps us divide our applications into different tangent spaces and we can then implement different services based on them.
- Microservices are delivered in small, managable pieces, independent of others, preventing the case of single-point failure.
- ### Advantages
- Better maintenance: since the code base is small
- Improved Productivity: same reason
- Greater fault tolerance: the spotted fault is constrained inside a container and is  segregated from other containers so that the fault can not be spread to other parts of the system.
- Better modularity: simple
- ### Components
- Gateway: redirect the request to different microservices
  logseq.order-list-type:: number
- Service Registry + discovery : register the microservice and assign a KV location on it (load balancing)
  logseq.order-list-type:: number
- Config Server: a RAFT based KV service
  logseq.order-list-type:: number
- Breaker Dashboard: Runtime Health check
  logseq.order-list-type:: number
- ### Design Pattern
- Independent database + message queue for asynchronous server request handling.
- Independent database + gateway to redirect the requests to domain-responsible microservices.
- ### Fault Tolerance
- ![image.png](../assets/image_1703854940036_0.png)
- Survilliance
  logseq.order-list-type:: number
	- Heartbeat system or periodical request
	  logseq.order-list-type:: number
	- Throwing exception
	  logseq.order-list-type:: number
	- Data gathering and service probing
	  logseq.order-list-type:: number
	- ![image.png](../assets/image_1703855270986_0.png)
- Fault Analysis
  logseq.order-list-type:: number
	- Backtraces of requests and responses
	  logseq.order-list-type:: number
	- Logging analysis
	  logseq.order-list-type:: number
- Service restoration (primary+backup model  for fault tolerance)
  logseq.order-list-type:: number
- Service degradation (Temporarily disable some services)
  logseq.order-list-type:: number
- ---
- ## Serverless
- Only focus on the logic of the service instead of the server functionality (scheduler, transactin, security)
- In a serverless environment, the platform provides the infrastucture to build the applications with extremely lightweight functions.
- Platform handles all the starting, stopping and scaling chores.
- Event-driven workloads and flows into a service platform and chaining the functions to give different results.
- Stateless services very extensive
- ---
- ## MySQL
- ### Optimizing Overview
- Better Structured tables.
  logseq.order-list-type:: number
- Correct data types on columns
  logseq.order-list-type:: number
- Right indexes on certain columns to make queries efficient
  logseq.order-list-type:: number
- Storage Engine
  logseq.order-list-type:: number
- Row Format
  logseq.order-list-type:: number
- Locking Strategy
  logseq.order-list-type:: number
- Caching sized correctly
  logseq.order-list-type:: number
- ### Hardware Based
- Disk Seeks
  logseq.order-list-type:: number
- Disk Reading and writing
  logseq.order-list-type:: number
- CPU Cycles
  logseq.order-list-type:: number
- Memory Bandwidth
  logseq.order-list-type:: number
- ### Portability
- Wrap MySQL specific keywords in a statement with in a comment so that other database can use it.
- ### Indexing
- A B+ tree organized index
- Unnecessary indexes waste space and waste time and its variability depends.
- We need the right balance to achieve fast queries using the optimal balance.
- MYSQL uses indexes for operations such as
- Where Clauses
  logseq.order-list-type:: number
- Eliminate rows for consideration
  logseq.order-list-type:: number
- Optimize the lookup based on the leftmost prefix matching
  logseq.order-list-type:: number
- MIN MAX operation
  logseq.order-list-type:: number
- Sort or group a table if the sorting is done on a leftmost prefix of a usable prefix
  logseq.order-list-type:: number
- #### Cluster Index
- Primary key always has a cluster index based on a B+ tree index and B+ tree index location is related to the actual location of the data layout on the disk
- Query Performance benefits from the NOT NULL optimization because it cannot include any NULL values.
- NOT NULL optimization based.
- If primary key can not be determined and create an auto increment column to be used as a index.
- These unique IDs can serve as pointers and server as another table to be a foreign key.
- #### Spatial Index
- NOT NULL geometry valued columns
- SPATIAL index must be SRID-restricted to estimate the distance between multidimensional data
- #### Foreign Key Optimization
- Duplicated columns or less frequently accessed data might be separated to different tables.
- However, we need to perform joining when accessing the full record of the data. The latency of one single record will increase but the amortized cost is lower.
- #### Index Prefix
- We can use the prefix character s of the column to create index
- #### Multiple Column Indexes
- An index may consist of multiple columns and maintain  a partial order relationship. The order counts. If we preempt the searching using wrong order, the index will not take effect. The first must be used before using the second field for indexing
- **B -Tree is optimized for comparison or BETWEEN operators ALSO LIKE will benefit too (Only happens when the leftmost prefix matches )**
-
- ### Configuration Settings
-
- ### Data Layout
-
- ### Partitioning
	-
	-
	-
- ---
- ## NOSQL
- ### Volumes
- ## DataLake
- ## InfluxDB
- ## NGINX
- remove
  logseq.order-list-type:: number
- ## HADOOP
- ## HDFS
	- ### Requirements:
	- Large Data Access
	  logseq.order-list-type:: number
	- Streaming Data Access
	  logseq.order-list-type:: number
	- Commodity hardwares
	  logseq.order-list-type:: number
	- ### Concepts
	- NameNode: serving as a metadata server and stores the metadata and mapping. Gets where the data lives and check whether the file lives and replies to the client where the file lives and instruct the clients to read the data from the DataNode.
	  logseq.order-list-type:: number
	- DataNode: the place where the real data lives in. Does not know where is the data and how the data is organized. It only sends to heartbeat to the namenode.
	  logseq.order-list-type:: number
	- #+BEGIN_WARNING
	  We need to ensure the replication of the machines in the DataNode and replicates the whole system if possible to provide replication of namenodes system as well as the datanodes.
	  #+END_WARNING
	- #### DataNode
	- Access ops follows the Adjacent - first,  Balance Second rule.
	  logseq.order-list-type:: number
	- HDFS usually have one node per cluster.
	  logseq.order-list-type:: number
	- One file is split into different files and restored distributely across different machines.
	  logseq.order-list-type:: number
	- Heartbeat: **Block Report**
	  logseq.order-list-type:: number
		- Block Report holds the list of all blocks on a datanode.
		  logseq.order-list-type:: number
		- Namenode then can collect all the block reports and check the integrity of the file.
		  logseq.order-list-type:: number
		- If replication integrity checks fails, another replication process should be instigated by the NameNode.
		  logseq.order-list-type:: number
	- #### NameNode
	- Has logging and takes the responsibility of managinng fault tolerane.
	  logseq.order-list-type:: number
	- Replication factor is the times about the chunks are replicated. 
	  logseq.order-list-type:: number
	- Last Block is not the same size as the previous blocks.
	  logseq.order-list-type:: number
	- Files in HDFS are write-one and have strictly one writer. 
	  logseq.order-list-type:: number
	- #+BEGIN_TIP
	  Recall that the orders in the book store can be stored here. Since it's immutable.
	  #+END_TIP
-
	- ### Limitations
	- High latency and low-throughput. HDFS uses the concept of partitioning and stores the files in different batches.
	  logseq.order-list-type:: number
	- Large volumes of small files accesses.
	  logseq.order-list-type:: number
	- Large volumes are restricted and should be everything.
	  logseq.order-list-type:: number
	- #### Fault Tolerance and Persistence
	- There's no free dinner. Fault tolerance vs Performance. So the distance partitioning and replication factor counts in HDFS
	  logseq.order-list-type:: number
	- Replication factor should be set to three and one of them is assigned as the primary.
	  logseq.order-list-type:: number
	- #+BEGIN_IMPORTANT
	  SafeMode
	  If replication checks fails across the DataNode on Name, any chunk of files that does not satisfy the replication factor constraints should be replicated in the DataNode until the replication constraint is satisfied again
	  #+END_IMPORTANT
	- 3. NameNode has a edit log just like in CHFS which documents all the metadata changes. It is redo-undo log.
	- 4. DataNode has a CRC operation when guaranteeing integrity of the chunk file.