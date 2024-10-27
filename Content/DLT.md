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
In other words, $n'\in\mathrm{addresses}(n)$ indicates the existence of a ***one-way channel*** from $n$ to $n'$.

We will consider two kinds of channels
1. ***FIFO-channels*** that guarantee that messages can not outstrip one another during transmission,
2. ***general channels*** that do not support that guarantees.

Also, we distinguish
1. a ***reliable channel*** if it guarantees the delivery of any sent message eventually,
2. an ***unreliable channel*** is characterized by a nonzero probability of losing a message during delivery by the channel.

## Local Behavior of Network Nodes

To achieve the common aim each node in a distributed system should provide some agreed local behavior determined by its ***local algorithm***.

Each node's local algorithm step changes the current local state of this node and, maybe, sends the corresponding message.

Nodes that always follow their local algorithms are called ***good nodes***.<br/>
In contrast, nodes that do not always follow their local algorithms are called ***bad nodes***.

The presence of bad nodes in a system is the reason for so-called Byzantine faults.

A ***Byzantine fault*** in a distributed system occurs when different system nodes have received different data about the same situation.

## Conceptual Model of DL

