# Readings in Databases

A list of papers essential to understanding databases and building new data systems.

## <a name='TOC'>Table of Contents</a>

  1. [Basics and Algorithms](#basic-and-algo)
  2. [Essentials of Relational Databases](#essentials)
  3. [Classic System Design](#system-design)
  4. [Columnar Databases](#column)
  5. [Data-Parallel Computation](#data-parallel)
  6. [Trends (Cloud Computing, Warehouse-scale Computing, New Hardware)](#trends)
  7. [Miscellaneous](#misc)
  8. [External Reading Lists](#external)


## <a name='basic-and-algo'> Basics and Algorithms
* [The Five-Minute Rule Ten Years Later, and Other Computer Storage Rules of Thumb](http://www.cs.berkeley.edu/~rxin/db-papers/5-min-rule.pdf) (1997): This paper (and the original one proposed 10 years earlier) illustrates a quantitative formula to calculate whether a data page should be cached in memory or not. It is a delight to read Jim Gray approach to an array of related problems, e.g. how big should a page size be.

* [Paxos Made Simple](http://www.cs.berkeley.edu/~rxin/db-papers/Paxos.pdf) (2001): Paxos is a fault-tolerant distributed consensus protocol. It forms the basis of a wide variety of distributed systems. The idea is simple, but notoriously difficult to understand (perhaps due to the way the original Paxos paper was written).

* [AlphaSort: A Cache-Sensitive Parallel External Sort](http://www.cs.berkeley.edu/~rxin/db-papers/alphasort.pdf) (1995): cache-friendly sorting

* [Patience is a Virtue: Revisiting Merge and Sort on Modern Processors](http://research.microsoft.com/pubs/209622/patsort-sigmod14.pdf) (2014): Sorting revisited. Actually also a good survey on sorting algorithms used in practice and their trade-offs.
* [The Raft Consensus Algorithm](https://ramcloud.stanford.edu/wiki/download/attachments/11370504/raft.pdf) (20141) : Raft is a consensus algorithm designed as an alternative to Paxos. It was meant to be more understandable than Paxos by means of separation of logic, but it is also formally proven safe and offers some new features.[1] Raft offers a generic way to distribute a state machine across a cluster of computing systems, ensuring that each node in the cluster agrees upon the same series of state transitions. 


## <a name='essentials'> Essentials of Relational Databases

* [Anatomy of a Database System](https://mitpress.mit.edu/sites/default/files/titles/content/9780262693141_sch_0002.pdf) (200x): Joe Hellerstein's great overview of relational database systems. This essay walks readers through all components essential to relational database systems.

* [A Relational Model of Data for Large Shared Data Banks](http://www.cs.berkeley.edu/~rxin/db-papers/Relational-Model-Codd.pdf) (1970): Codd's argument for data independence (from 1970), a fundamental concept in relational databases. Despite the current NoSQL trend, I believe ideas from this paper are becoming increasingly important in massively parallel data systems.

* [ARIES: A Transaction Recovery Method Supporting Fine-Granularity Locking and Partial Rollbacks Using Write-Ahead Logging](http://www.cs.berkeley.edu/~rxin/db-papers/ARIES.pdf) (1992): The first algorithm that actually works: it supports concurrent execution of transactions without losing data even in the presence of failures. This paper is very hard to read because it mixes a lot of low level details in the explanation of the high level algorithm. Perhaps try understand ARIES (log recovery) by reading a database textbook before attempting to read this paper.

* [Efficient Locking for Concurrent Operations on B-Trees](http://www.cs.berkeley.edu/~rxin/db-papers/B-tree.pdf) (1981) and The [R*-tree: An Efficient and Robust Access Method for Points and Rectangles](http://www.cs.berkeley.edu/~rxin/db-papers/R-tree.pdf) (1990): B-Tree is a core data structure in databases (not just relational). It is optimized and has a low read amplification factor for random lookups of on-disk data. R-tree is an extension of B-tree to support lookups of multi-dimensional data, e.g. geodata.

* [Improved Query Performance with Variant Indexes](http://www.cs.duke.edu/courses/spring03/cps216/papers/oneil-quass-1997.pdf) (1997): Analytical databases and OLTP databases require different trade-offs. These are reflected in the choices of indexing data structures. This paper talks about a number of index data structures more suitable for analytical databases.

* [On Optimistic Methods for Concurrency Control](http://www.cs.berkeley.edu/~rxin/db-papers/OCC-Optimistic-Concurrency-Control.pdf) (1981): There are two ways to support concurrency. The first is the pessimistic way, i.e. to lock shared data preemptively. This paper explains an alternatively to locking called Optimistic Concurrency Control. Optimistic approaches assume conflicts are rare and executes transactions without acquiring locks. Before committing the transactions, the database system checks for conflicts and aborts/restarts transactions if conflicts arise.

* [Access Path Selection in a Relational Database Management System](http://www.cs.berkeley.edu/~rxin/db-papers/SystemR-Optimizer.pdf) (1979): The basics of query optimization. SQL is declarative, i.e. you specify using a query language what data you want, not how you want it. There are usually multiple ways (query plans) of executing a query. The database system examines multiple plans and decides on an optimal one (best-effort). This process is called query optimization. The traditional way of doing query optimization is to have a cost-model for different access methods and query plans. This paper explains the cost-model and a dynamic programming algorithm to pick the best plan.

* [Eddies: Continuously Adaptive Query Processing](http://www.cs.berkeley.edu/~rxin/db-papers/Eddies.pdf) (2000): Traditional query optimization (and the cost model used) is static. There are two problems with the traditional model. First, it is hard to build the cost model absent of data statistics. Second, query execution environment might change in long running queries and a static approach cannot capture the change. Analogous to fluid dynamics, this paper proposes a set of techniques that optimize query execution dynamically. I don't think ideas in Eddies have made their way into commercial systems yet, but the paper is very refreshing to read and might become more important now.


## <a name='system-design'> Classic System Design

* [A History and Evaluation of System R](http://www.cs.ubc.ca/~rap/teaching/504/2010/readings/history-of-system-r.pdf) (1981): There were System R from IBM and Ingres from Berkeley, two systems that showed relational database was feasible. This paper describes System R. It is impressive and scary to note that the internals of relational database systems in 2012 look a lot like System R in 1981.

* [The Google File System](http://research.google.com/archive/gfs.html) (2003) and [Bigtable: A Distributed Storage System for Structured Data](http://research.google.com/archive/bigtable.html) (2006): Two core components of Google's data infrastructure. GFS is an append-only distributed file system for large sequential reads (data-intensive applications). BigTable is high-performance distributed data store that builds on GFS. One way to think about it is that GFS is optimized for high throughput, and BigTable explains how to build a low-latency data store on top of GFS. Some of these might have been replaced by newer proprietary technologies internal to Google, but the ideas stand.

* [Chord: A Scalable Peer-to-peer Lookup Service for Internet Applications](http://www.cs.berkeley.edu/~rxin/db-papers/Chord-DHT.pdf) (2001) and [Dynamo: Amazon’s Highly Available Key-value Store](http://www.cs.berkeley.edu/~rxin/db-papers/Dynamo.pdf) (2007): Chord was born in the days when distributed hash tables was a hot research. It does one thing, and does it really well: how to look up the location of a key in a completely distributed setting (peer-to-peer) using consistent hashing. The Dynamo paper explains how to build a distributed key-value store using Chord. Note some design decisions change from Chord to Dynamo, e.g. finger table O(logN) vs O(N), because in Dynamo's case, Amazon has more control over nodes in a data center, while Chord assumes peer-to-peer nodes in wide area networks.



## <a name='column'> Columnar Databases

Columnar storage and column-oriented query engine are critical to analytical workloads, e.g. OLAP. It's been almost 15 years since it first came out (the MonetDB paper in 1999), and almost every commercial warehouse database has a columnar engine by now.

* [C-Store: A Column-oriented DBMS](http://www.cs.berkeley.edu/~rxin/db-papers/C-Store.pdf) (2005) and [The Vertica Analytic Database: C-Store 7 Years Later](http://vldb.org/pvldb/vol5/p1790_andrewlamb_vldb2012.pdf) (2012): C-Store is an influential, academic system done by the folks in New England. Vertica is the commercial incarnation of C-Store.

* [Column-Stores vs. Row-Stores: How Different Are They Really?](http://db.csail.mit.edu/projects/cstore/abadi-sigmod08.pdf) (2012): Discusses the importance of both the columnar storage and the query engine.

* [Dremel: Interactive Analysis of Web-Scale Datasets](http://research.google.com/pubs/pub36632.html) (2010): A jaw-dropping paper when Google published it. Dremel is a massively parallel analytical database used at Google for ad-hoc queries. The system runs on thousands of nodes to process terabytes of data in seconds. It applies columnar storage to complex, nested data structures. The paper talks a lot about the nested data structure support, and is a bit light on the details of the query execution. Note that a number of open source projects are claiming they are building "Dremel". The Dremel system achieves low-latency through massive parallelism and columnar storage, so the model doesn't necessarily make sense outside Google since very few companies in the world can afford thousands of nodes for ad-hoc queries.


## <a name='data-parallel'> Data-Parallel Computation

* [MapReduce: Simplified Data Processing on Large Clusters](http://research.google.com/archive/mapreduce.html) (2004): MapReduce is both a programming model (borrowed from an old concept in functional programming) and a system at Google for distributed data-intensive computation. The programming model is so simple yet expressive enough to capture a wide range of programming needs. The system, coupled with the model, is fault-tolerant and scalable. It is probably fair to say that half of the academia are now working on problems heavily influenced by MapReduce.

* [Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing](http://www.cs.berkeley.edu/~rxin/db-papers/Spark.pdf) (2012): This is the research paper behind the Spark cluster computing project at Berkeley. Spark exposes a distributed memory abstraction called RDD, which is an immutable collection of records distributed across a cluster's memory. RDDs can be transformed using MapReduce style computations. The RDD abstraction can be orders of magnitude more efficient for workloads that exhibit strong temporal locality, e.g. query processing and iterative machine learning. Spark is an example of why it is important to separate the MapReduce programming model from its execution engine.

* [Shark: SQL and Rich Analytics at Scale](https://amplab.cs.berkeley.edu/publication/shark-sql-and-rich-analytics-at-scale/) (2013): Describes the Shark system, which is the SQL engine built on top of Spark. More importantly, the paper discusses why previous SQL on Hadoop/MapReduce query engines were slow.

* [Spanner](http://static.googleusercontent.com/media/research.google.com/en//archive/spanner-osdi2012.pdf) (2012): Spanner is "a scalable, multi-version, globally distributed, and synchronously replicated database". The linchpin that allows all this functionality is the TrueTime API which lets Spanner order events between nodes without having them communicate. [There is some speculation that the TrueTime API is very similar to a vector clock but each node has to store less data](http://www.cse.buffalo.edu/~demirbas/publications/augmentedTime.pdf). Sadly, a paper on TrueTime is promised, but hasn't yet been released.

* [Dryad: Distributed Data-Parallel Programs from Sequential Building Blocks](http://cs.brown.edu/~debrabant/cis570-website/papers/dryad.pdf) (2007): Dryad is a programming model developed at Microsoft that enables large scale dataflow programming. "The fundamental difference between the \[MapReduce and Dryad\] is that a Dryad application may specify an arbitrary communication DAG rather than requiring a sequence of map/distribute/sort/reduce operations". 

## <a name='trends'> Trends (Cloud Computing, Warehouse-scale Computing, New Hardware)

* [A View of Cloud Computing](http://www.cs.berkeley.edu/~rxin/db-papers/cloudcomputing.pdf) (2010): This is THE paper on Cloud Computing. This paper discusses the economics and obstacles of cloud computing (referring to the elasticity of resources, not the consumer-facing "cloud") from a technical perspective. The obstacles presented in this paper will impact design decisions for systems running in the cloud.

* [The Datacenter as a Computer: An Introduction to the Design of Warehouse-Scale Machines](http://www.cs.berkeley.edu/~rxin/db-papers/WarehouseScaleComputing.pdf): Google's Luiz André Barroso and Urs Hölzle explains the basics of data center hardware and software for warehouse-scale computing. There is an [accompanying video](http://dl.acm.org/citation.cfm?id=2019527&bnc=1). The video talks about the importance of cutting long-tail latency in massively parallel systems. The other key idea is the disaggregation of resources. Technologies such as GFS/HDFS already disaggregate disks because of high network bandwidth, but yet to see the same trend applying to DRAMs because that'd require low-latency networking.

* [CAP Twelve Years Later: How the "Rules" Have Changed](http://www.cs.berkeley.edu/~rxin/db-papers/CAP.pdf) (2012): The CAP theorem, proposed by Eric Brewer, asserts that any net­worked shared-data system can have only two of three desirable properties: Consistency, Availability, and Partition-Tolerance. A number of NoSQL stores reference CAP to justify their decision to sacrifice consistency. This is Eric Brewer's writeup on CAP in retrospective, explaining "'2 of 3' formulation was always misleading because it tended to oversimplify the tensions among properties."


## <a name='misc'> Miscellaneous

* [Reflections on Trusting Trust](http://www.cs.berkeley.edu/~rxin/db-papers/TrustingTrust-Thompson.pdf) (1984): Ken Thompson's Turing Award acceptance speech in 1984, describing black box backdoor issues and pointing out trust is not absolute.



## <a name='externel'> External Reading Lists

A number of schools have their own reading lists for graduate students in databases.

* Berkeley: http://www.eecs.berkeley.edu/GradAffairs/CS/Prelims/db.html
* Brown: http://www.cs.brown.edu/courses/cs227/papers.html
* Stanford: http://infolab.stanford.edu/db_pages/infoqual.html
* Wisconsin: http://www.cs.wisc.edu/sites/default/files/db.reading.pdf
* [The reading list](http://www.cs286.net/home/reading-list) for Joseph Hellerstein's graduate course in database systems at Berkeley. It is more comprehensive than this list, but there is substantial overlap.

