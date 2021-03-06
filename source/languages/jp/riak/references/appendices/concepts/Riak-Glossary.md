---
title: The Riak Glossary
project: riak
version: 0.10.0+
document: appendix
toc: true
audience: intermediate
keywords: [appendix, concepts]
---

Below is a list of terms and their applicability within the context of Riak:

## Bitcask

[[Bitcask]] is an Erlang application that provides an API for storing and
retrieving key/value data into a log-structured hash table with very fast
access. The design owes a lot to the principles found in log-structured file
systems and draws inspiration from a number of designs that involve log file
merging.

Bitcask ships with Riak as the default storage engine.

## Bucket

A Bucket is a container and keyspace for data stored in Riak, with a set of common properties for its contents (the number of replicas, or *n_val*, for instance).  Buckets are accessed at the top of the URL hierarchy under "riak", e.g. `/riak/bucket`.
* [[Take a more in-depth look at Bucket Operations|HTTP API#Bucket Operations]]

## CAP Theorem

[[CAP Theorem|http://en.wikipedia.org/wiki/CAP_theorem]], also known as
**Brewer's Theorem** states that in distributed systems, it is impossible to
simultaneously guarantee consistency, availability, and partition tolerance.

Riak's design is heavily influenced by the CAP Theorem.

## Cluster

A Riak cluster is a 160-bit integer space which is divided into equally-sized partitions. Each vnode in the Riak Ring is responsible for one of these partitions.

## Commit Hook

[[Commit Hooks]] provide a means for enhancing application interaction with
Riak either before or after persisting an object. Commit Hooks can be
defined in Riak bucket properties and execute once per client request.

## Consistent Hashing

Consistent hashing is a technique used to limit the reshuffling of keys when a hash-table data structure is rebalanced (when slots are added or removed). Riak uses consistent hashing to organize its data storage and replication. Specifically, the vnodes in the Riak Ring responsible for storing each object are determined using the consistent hashing technique.

* [[More on Consistent Hashing|http://en.wikipedia.org/wiki/Consistent_hashing]]

## Gossiping

Riak uses a "gossip protocol" to share and communicate ring state and bucket properties around the cluster.  Whenever a node changes its claim on the ring, it announces its change via this protocol.  Each node also periodically sends its current view of the ring state to a randomly-selected peer, in case any nodes missed previous updates.

## Hinted Handoff

Hinted handoff is a technique for dealing with node failure in the Riak cluster in which neighboring nodes temporarily take over storage operations for the failed node.  When the failed node returns to the cluster, the updates received by the neighboring nodes are handed off to it.

Hinted handoff allows Riak to ensure database availability.  When a node fails, Riak can continue to handle requests as if the node were still there.

## Key

Keys are unique object identifiers in Riak and are scoped within buckets.

## Lager

[[Lager|https://github.com/basho/lager]] is an Erlang/OTP framework that ships as Riak's default logger.

## LevelDB

[[LevelDB|http://code.google.com/p/leveldb/]] is an open source on-disk
key-value store written by Google Fellows Jeffrey Dean and Sanjay Ghemawat.

Riak provides [[eLevelDB|LevelDB]], an Erlang application that encapsulates
LevelDB as one of its supported storage engines.

## Links

Links are metadata attached to objects in Riak. These links make establishing relationships between objects in Riak as simple as adding a Link header to the request when storing the object.

* [[More about Riak's Links|Links]]

## MapReduce

Riak's MapReduce gives developers the capability to perform more powerful queries over the data stored in their key/value data.

* [[An In Depth Look at Riak's MapReduce|MapReduce]]

## Multi Data Center Replication

Multi Data Center Replication is available in [[Riak Enterprise|http://basho.com/products/riak-enterprise/]]
and provides a mechanism for replicating data from one Riak cluster to
another.

In multi data center replication, one cluster acts as a *primary cluster*.
The primary cluster replicates its data to one or more *secondary clusters*
with a full or real-time sync process. Data transfer is unidirectional by default, but bidirectional synchronization can be achieved by configuring a
pair of connections between clusters.


* [[More on multi data center replication|Multi-Data-Center-Replication-Architecture]]

## N Value

N Value or *n_val* defines the number of nodes in a Riak cluster to which
data are replicated. Riak sets a default N Value of **3** which means that
Riak will replicate all data to 3 nodes. N Value is configurable on a per
bucket basis and must be a value greater than zero and equal to or less than
the number of nodes in the cluster.

* [[Learn more about N Value|Tunable-CAP-Controls-in-Riak]]

## Node

A node is analogous to a physical server. Nodes run a certain number of vnodes, each of which claims a partition in the Riak Ring key space.

## Object

An object is another name for a value.

## Partition

Partitions are the spaces into which a Riak cluster is divided. Each vnode in Riak is responsible for a partition. Data are stored on a set number of partitions determined by the *n_val* setting, with the target partitions chosen statically by applying consistent hashing to an object's key.

## Protocol Buffers

[[Protocol Buffers|https://developers.google.com/protocol-buffers/docs/overview]]
are a technology developed by Google for the serialization of structured data.
Riak exposes a [[Protocol Buffers Client|protocol-buffers]] interface
which is available in the supported [[client libraries|Client-Libraries]].

## Quorum

Quorum in Riak has two meanings:

* The quantity of replicas that must respond to a read or write request before it is considered successful. This is defined as a bucket property or as one of the relevant parameters to a single request (R,W,DW,RW).
* A symbolic quantity for the above, `quorum`, which is equivalent to `n_val / 2 + 1`. With Riak's default settings, this is `2`.

## Read Repair

Read repair is an anti-entropy mechanism Riak uses to optimistically update stale replicas when they reply to a read request with stale data.

* [[More about Read Repair|Replication]]

## Replica

Replicas are copies of data stored in Riak. The number of replicas required for both successful reads and writes is configurable in Riak and should be set based on your application's consistency and availability requirements.

## Riak Core

[[Riak Core|https://github.com/basho/riak_core]] is the modular distributed systems framework that serves as the foundation for Riak's scalable architecture.

## Riak KV

[[Riak KV|https://github.com/basho/riak_kv]] is a component application of Riak and provides a key/value datastore and features MapReduce, lightweight data relations, and several different client APIs.

## Riak Pipe

[[Riak Pipe|https://github.com/basho/riak_pipe]] is the processing layer that powers Riak's MapReduce. It's best described as "UNIX pipes for Riak."

## Riak Search

Riak Search is a distributed, scalable, failure-tolerant, real-time, full-text search engine built around Riak Core and tightly integrated with Riak KV.

* [[In Depth on Riak Search|Riak Search]]

## Ring

The Riak Ring is a 160-bit integer space. This space is equally divided into partitions, each of which is claimed by a vnode, which themselves reside on actual physical server nodes.

## Secondary Indexing

Secondary Indexing in Riak gives developers the ability to tag an object stored in Riak with one or more values which can then be queried.

* [[More about Secondary Indexes|Secondary Indexes]]

## Sibling

A sibling is created when Riak is unable to resolve the canonical version of
an object being stored. Riak uses siblings because it is impossible to order
events with respect to time in a distributed system, this means they must be
ordered causally.

* [[More on siblings|Vector-Clocks#Siblings]]

## Value

Riak is most-easily described as a key/value store. In Riak, "values" are opaque BLOBS (binary large objects), identified with a unique key, that can be any type of data, though some programming advantages are gained by using JSON.

## Vector Clock

Riak utilizes vector clocks (or _vclock_) to handle version control. Since any node in a Riak cluster is able to handle a request, and not all nodes need to participate, data versioning is required to keep track of a current value. When a value is stored in Riak, it is tagged with a vector clock and establishes the initial version. When it is updated, the client provides the vector clock of the object being modified so that this vector clock can be extended to reflect the update.  Riak can then compare vector clocks on different versions of the object and determine certain attributes of the data.

* [[More about Vector Clocks|Vector clocks]]

## Vnode

Vnodes, or "virtual nodes" are responsible for claiming a partition in the Riak Ring, and they coordinate requests for these partitions. Vnodes reside on physical nodes in a Riak cluster, and the number of vnodes per physical node is determined by the total number of vnodes and the number of active physical nodes in the cluster. Riak balances the assignment of vnodes across the active physical nodes.
