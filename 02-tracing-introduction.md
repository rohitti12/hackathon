# What is distributed tracing?

**Distributed tracing** allows you to see how a request progresses through different services and systems, the timing of each operation, any logs and errors as they occur.
- *Distributed* refers to distributed systems, which consist of independent components that communicate through requests to form an application. 
- *Tracing* refers to traces, which track the end-to-end path of a request as each travels from service to service. 
 
## Why is distributed tracing important?

Distributed tracing is crucial for understanding the behavior of complex, distributed systems. It provides insights into how requests flow through different components, helping to diagnose and troubleshoot issues related to latency, errors, and dependencies.


## How does distributed tracing work?

Distributed tracing works by assigning a unique identifier to a request and propagating this identifier across different services involved in processing the request. Each service records information about the request, creating a trace that can be visualized and analyzed and this process is called as *Context Propagation*

### Context: 
Context is an object that contains the information for the sending and receiving service, or execution unit, to correlate one signal with another.


### Propagation:
Propagation is the mechanism that moves context between services and processes. It serializes or deserializes the context object and provides the relevant information to be propagated from one service to another. Propagation is usually handled by instrumentation libraries and is transparent to the user.

OpenTelemetry maintains several official propagators. The default propagator is using the headers specified by the W3C TraceContext specification.



