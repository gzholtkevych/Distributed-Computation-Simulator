# *Logical Clock in **D**istributed **L**edger **T**echnology* (**DLT**)

**D**distributed **L**edger **T**echnology (**DLT**) is a new data storage paradigm that provides information distribution across many computers (***nodes***) in the network.

In other words, any **D**istributed **L**edger (**DL**) is a distributed information system that provides accumulating and storing information and guarantees

- immutability,
- availability, and
- consistency

of the stored information.

This understanding of DL focuses our research on the following issues.

1. The network topology of DL.
2. Specificity to communicate network nodes.
3. Local behavior of each network node.

## Network Topology

> A ***network topology*** is defined by
>
>- a finite set $P$ of nodes which contains at least two members,
>- a function $\mathrm{addressees}: P\rightarrow 2^P$ that associates the set of addressee nodes with each node.
>
> It is assumed that the following constraints
>
> - for any $n\in P$, $n\notin\mathrm{addressees}(n)$;
> - ***connectivity:*** for any $n'\neq n''\in P$, there exist $n_k\in P,\ k=1,\ldots,m$ where $m \geq 2$ such that
>   - $n_1=n'$ and $n_m=n''$;
>   - $n_{k+1}\in\mathrm{addressees}(n_k)$ for all $k=1,\ldots,m-1$.

---

> If a network topology satisfies the constraint
>
> - for any $n'\neq n''\in P$, $n'\in\mathrm{addressees}(n'')$ implies $n''\in\mathrm{addressees}(n')$
>
> then the network is called ***unoriented***.

## Examples of Networks

### An Oriented Ring

### An Unoriented Ring

### An Unoriented Tree

A network topology is ***unoriented tree-like*** if for any $n',n''\in P$, there exists exactly one sequence $n_k\in P$, $k=1,\ldots,m$ where $m\geq 2$ such that

- $n_1=n'$ and $n_m=n''$;
- for any $1\leq k<l\leq m$, $n_k\neq n_l$;
- for any $k=1,\ldots,m-1$, $n_{k+1}\in\mathrm{addressees}(n_k)$ .

## Communication of Network Nodes

It is assumed that network nodes communicate only by message passing.

We assume that node $n\in P$ can directly send a message to node $n'\neq n\in P$ if $n'\in\mathrm{addresses}(n)$.
In other words, $n'\in\mathrm{addresses}(n)$ indicates the existence of a one-way channel from $n$ to $n'$.

## Appendix

### DL Node Behavior for Oriented Ring with FIFO channels

```python
class Node:
    def __init__(self, network, id: int):
        self.__network = network 
        self.__id = id
        self.__ledger = []
        self.__queue: List[Any] = []
        self.__lts: int = 0
        self.__ts: int  = 0

    def behavior(self):
        def handle_write_request(data: Any):
            offer = ("offer", self.__id, self.__ts)
            self.__queue.append(data)
            self.__ts += 1
            self.__network.send(offer)
        # end of handle_write_request()
        def handle_offer(offer: Tuple[int, int]):
            if offer[0] == self.__id:
                data, self.__queue = self.__queue[0], self.__queue[1 :]
                grant = ("grant", self.__id, self.__lts, offer[1], data)
                self.__lts += 1
                self.__ledger.append(grant[1 :])
                self.__network.send(grant)
            else:
                ts = max(offer[1], self.__ts)
                new_offer = ("offer", offer[0], ts)
                self.__ts = ts + 1
                self.__network.send(new_offer)
        # end of handle_offer()
        def handle_grant(grant: Tuple[int, int, int, Any]):
            if grant[0] != self.__id:
                self.__ledger.append(grant[1 :])
                self.__network.send(grant)
            else:
                pass
        # end of handle_grant()
        while True:  # main loop
            while True:  # wait for a message
                msg: Optionanal[Tuple[str, Any]] = network.receive()
                if msg is None:
                    continue
            # end of waiting
            if msg[0] == "wreq":
                handle_write_request(msg[1])
            elif msg[0] == "offer":
                handle_offer(msg[1 :])
            elif msg[0] == "grant":
                handle_grant(msg[1 :])
```



prove：

```python
1. Timestamp Consistency
handle_write_request Method: When a node receives a write request, it creates an "offer" message that includes the current local timestamp (ts). Before sending the "offer," the node appends the request data to its queue and increments the timestamp. This ensures that the timestamp of the sent "offer" reflects the state of the node after processing the request.

handle_offer Method: When the node receives an "offer," if the "offer" is from itself, the node processes the data from its queue and sends a "grant" message. During the processing of the "offer," the node updates its timestamp to be the maximum of the received timestamp and its current timestamp, ensuring that the new "offer" has a valid timestamp. This handling guarantees the monotonicity of timestamps.

2. Event Causality
Processing Logic: When the node sends a "grant" message, it includes the current local timestamp (lts) along with the corresponding data from its queue. Each time the node processes a request (whether "wreq," "offer," or "grant"), it follows a strict order, ensuring that the event causality is maintained. Specifically, if an event from node A affects an event at node B, then the timestamp of node A's event will precede the timestamp of node B's event.
    
Conclusion
The logic implemented in the code ensures that under the conditions of reliable channels and adherence to the algorithm by all nodes, both timestamp consistency and event causality are preserved. Therefore, the logical clock algorithm is correct.
```



### DL Node Behavior for Oriented Ring

```python

```
