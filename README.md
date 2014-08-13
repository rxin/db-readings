# Readings in Databases

A list of papers essential to understanding databases and building new data systems.

## <a name='TOC'>Table of Contents</a>

  1. [Basics and Algorithms](#basic-and-algo)
  2. [Essentials of Relational Databases](#indent)
  3. [Classic Systems Design](#system-design)
  4. [Columnar Databases](#column)
  5. [Data-Parallel Computation](#data-parallel)
  6. [Trends (Cloud Computing, Warehouse-scale Computing, New Hardware)](#trends)
  7. [Miscellaneous](#misc)
  8. [External Reading Lists](#external)


## <a name='basic-and-algo'> Basics and Algorithms
* [The Five-Minute Rule Ten Years Later, and Other Computer Storage Rules of Thumb](http://www.cs.berkeley.edu/~rxin/db-papers/5-min-rule.pdf): This paper (and the original one proposed 10 years earlier) illustrates a quantitative formula to calculate whether a data page should be cached in memory or not. It is a delight to read Jim Gray approach to an array of related problems, e.g. how big should a page size be.

* [Paxos Made Simple](http://www.cs.berkeley.edu/~rxin/db-papers/Paxos.pdf): Paxos is a fault-tolerant distributed consensus protocol. It forms the basis of a wide variety of distributed systems. The idea is simple, but notoriously difficult to understand (perhaps due to the way the original Paxos paper was written).

* [AlphaSort: A Cache-Sensitive Parallel External Sort](http://www.cs.berkeley.edu/~rxin/db-papers/alphasort.pdf): cache-friendly sorting


## <a name='basic-and-algo'> Essentials of Relational Databases

* [Anatomy of a Database System](http://mitpress.mit.edu/books/chapters/0262693143chapm2.pdf): Joe Hellerstein's great overview of relational database systems. This essay walks readers through all components essential to relational database systems.

* [A Relational Model of Data for Large Shared Data Banks](http://www.cs.berkeley.edu/~rxin/db-papers/Relational-Model-Codd.pdf): Codd's argument for data independence (from 1970), a fundamental concept in relational databases. Despite the current NoSQL trend, I believe ideas from this paper are becoming increasingly important in massively parallel data systems.

* [ARIES: A Transaction Recovery Method Supporting Fine-Granularity Locking and Partial Rollbacks Using Write-Ahead Logging](http://www.cs.berkeley.edu/~rxin/db-papers/ARIES.pdf): The first algorithm that actually works: it supports concurrent execution of transactions without losing data even in the presence of failures. This paper is very hard to read because it mixes a lot of low level details in the explanation of the high level algorithm. Perhaps try understand ARIES (log recovery) by reading a database textbook before attempting to read this paper.

* [Efficient Locking for Concurrent Operations on B-Trees](http://www.cs.berkeley.edu/~rxin/db-papers/B-tree.pdf) and The [R*-tree: An Efficient and Robust Access Method for Points and Rectangles](http://www.cs.berkeley.edu/~rxin/db-papers/R-tree.pdf): B-Tree is a core data structure in databases (not just relational). It is optimized and has a low read amplification factor for random lookups of on-disk data. R-tree is an extension of B-tree to support lookups of multi-dimensional data, e.g. geodata.

* [Improved Query Performance with Variant Indexes](http://www.cs.duke.edu/courses/spring03/cps216/papers/oneil-quass-1997.pdf): Analytical databases and OLTP databases require different trade-offs. These are reflected in the choices of indexing data structures. This paper talks about a number of index data structures more suitable for analytical databases.

* [On Optimistic Methods for Concurrency Control](http://www.cs.berkeley.edu/~rxin/db-papers/OCC-Optimistic-Concurrency-Control.pdf): There are two ways to support concurrency. The first is the pessimistic way, i.e. to lock shared data preemptively. This paper explains an alternatively to locking called Optimistic Concurrency Control. Optimistic approaches assume conflicts are rare and executes transactions without acquiring locks. Before committing the transactions, the database system checks for conflicts and aborts/restarts transactions if conflicts arise.

* [Access Path Selection in a Relational Database Management System](http://www.cs.berkeley.edu/~rxin/db-papers/SystemR-Optimizer.pdf): The basics of query optimization. SQL is declarative, i.e. you specify using a query language what data you want, not how you want it. There are usually multiple ways (query plans) of executing a query. The database system examines multiple plans and decides on an optimal one (best-effort). This process is called query optimization. The traditional way of doing query optimization is to have a cost-model for different access methods and query plans. This paper explains the cost-model and a dynamic programming algorithm to pick the best plan.

* [Eddies: Continuously Adaptive Query Processing](http://www.cs.berkeley.edu/~rxin/db-papers/Eddies.pdf): Traditional query optimization (and the cost model used) is static. There are two problems with the traditional model. First, it is hard to build the cost model absent of data statistics. Second, query execution environment might change in long running queries and a static approach cannot capture the change. Analogous to fluid dynamics, this paper proposes a set of techniques that optimize query execution dynamically. I don't think ideas in Eddies have made their way into commercial systems yet, but the paper is very refreshing to read and might become more important now.


## <a name='system-design'> Classic Systems Design


## <a name='column'> Columnar Databases


## <a name='data-parallel'> Data-Parallel Computation


## <a name='trends'> Trends (Cloud Computing, Warehouse-scale Computing, New Hardware)


## <a name='misc'> Miscellaneous


## <a name='externel'> External Reading Lists

A number of schools have their own reading lists for graduate students in databases.

* Berkeley: http://www.eecs.berkeley.edu/GradAffairs/CS/Prelims/db.html
* Brown: http://www.cs.brown.edu/courses/cs227/papers.html
* Stanford: http://infolab.stanford.edu/db_pages/infoqual.html
* Wisconsin: http://www.cs.wisc.edu/sites/default/files/db.reading.pdf

