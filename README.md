# Foundations of Data Systems

## 1. Reliable, Scalable, and Maintainable Applications

### Thinking About Data Systems

Many applications have common needs, including:

1. **Storing Data**: They keep data in databases so it or another app can use it later.
2. **Caching**: They remember previous complex operations to make future access faster.
3. **Search and Filter**: Users can look up data using keywords or apply filters, done through search indexes.
4. **Asynchronous Messaging**: They send messages to other processes for handling later, a part of stream processing.
5. **Batch Processing**: Periodically, they process a large amount of data all at once.

### Reliability

A fault is defined as one component of the system deviating from its spec, whereas a failure is when the system as a whole stops providing the required service to the user. Faults are usually caused by hardware or software faults.

Monitoring helps with detecting faults and failures.

### Scalability

Scalability: Describes a system's ability to cope with increased load.

Load parameters, which vary based on system architecture, describe the load on a system. Examples include requests per second for web servers, read/write ratios in databases, active users in chat rooms, or cache hit rates. What's important might be average usage or, in some cases, extreme usage scenarios.

Complex web of connections where one action (a tweet) spreads to many users is what "fan-out" describes.

Latency is the waiting time for a request, while response time includes the entire time from request to reply, covering processing, network, and queueing delays.

Percentile: A number where a certain percentage of the data is lower than that number. For example, the 95th percentile is the number where 95% of the data is lower than that number, e.g. 95% of requests are faster than 200ms.

**Head-of-Line Blocking:** A situation where a few slow requests delay the processing of subsequent requests.

A well-scaling architecture is based on assumptions about common and rare operations (load parameters). If these assumptions are wrong, scaling efforts can be wasteful or even harmful.

### Maintainability

Designing software with future maintenance in mind can prevent creating new legacy problems. Focus on three key design principles:

1. **Operability**: Ensure it's easy for operations teams to manage and maintain the system.
2. **Simplicity**: Aim for ease of understanding for new engineers, focusing on reducing system complexity rather than just simplifying the user interface.
3. **Evolvability (Extensibility/Modifiability)**: Design the system so it's easy to adapt and modify for future, unforeseen needs.
