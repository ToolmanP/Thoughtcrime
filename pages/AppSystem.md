## Overview
- General Principle: more maintainable, more extensible, faster and less prone to single-point failure
  logseq.order-list-type:: number
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
- ## WebSocket
- Sometime we do not need synchronous communication and if our requests are handled asynchronously, we naively require a method to receive asynchronous response.
- WebSocket is an application protocol that provides full-duplex communications between two peers over the TCP Protocol.
- It consists two parts: handshake and data transfer
- ### HandShake
- HTTP Based
  logseq.order-list-type:: number
- Use bidirectional keys to authenticate 
  logseq.order-list-type:: number
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
- ## Concurrency Control
- ### Optimistic Concurrency Control
- We insert a version control number and a last-modified time in the row of the table and abort and try the changes when the version has changed when writing back.
- ### Pessimistic Concurrency Control
- Simply by locking the whole row/table and do not allow interleaved writing.
- ## Log Structured Merge Tree
- ---
- ## MySQL
	- ### Partitioning
	- ### Indexing
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