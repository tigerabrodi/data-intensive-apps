# CAP Theorem

CAP theorem is a concept that a distributed system can only have two of the following three properties:

- **Consistency**: All nodes see the same data at the same time.
- **Availability**: Every request gets a response on success/failure.
- **Partition Tolerance**: The system continues to operate despite network partitions.

It's important to mention that the CAP theorem doesn't apply to single-node databases. It only applies to distributed systems, by which I mean systems that run on multiple nodes.

The reason it doesn't apply to single-node databases is that they don't have to deal with network partitions. If you have a single-node database, you can have consistency and availability at the same time.

# What we can pick?

You may think we can pick any two of the three properties, but that's not the case.

Partition tolerance is a must-have in a distributed system because network partitions are inevitable. By network partition, I mean that some nodes can't communicate with other nodes. You can imagine a system where we have a leader node and some follower nodes. If the leader node can't communicate with the followers, we have a network partition. So you can't pick Consistency and Availability (CA).

This means you can either pick Consistency and Partition Tolerance (CP), or Availability and Partition Tolerance (AP).

# Consistency

Consistency means that every single read on any node returns the most recent write. This is the strongest consistency model. If you have a system that is consistent, you can be sure that all nodes see the same data at the same time.

The upside of consistency is that your users will always see the most recent data. The downside is that if a node can't communicate with the other nodes, it can't serve requests. This means that if you have a consistent system, you can't have availability when there is a network partition (a node can't communicate with the other nodes).

# Availability

Availability means that every request gets a response on success/failure. This is the strongest availability model. If you have a system that is available, you can be sure that every request gets a response.

The upside of availability is that your system can serve requests even when there is a network partition. The downside is that your users may see stale data. This is because if a node can't communicate with the other nodes, it can't be sure that it has the most recent data.

# Consistency or Availability?

If the system must always return the most recent data, such as in banking systems, you should pick Consistency over Availability. If the system must always be available, such as in social networks, you should pick Availability over Consistency.

It's different for every system, and you must decide what is more important for your system.
