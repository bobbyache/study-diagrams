# Leader Election

### Case Study
A company has a database in the cloud  with user subscriptions and wants to bill users for 1 year or 1 month subscriptions. A third party service is used to actually make the payments. Lets say its PayPal. So this service handles the payments, and there is a broker service in the middle of the two that ensures that:
- PayPal doesn't talk directly to the organizations database.
- When a user subscription is due for renewal, a request is sent to PayPal to charge the user's account. Tells PayPal when to charge and how much.


However this is a single point of failure. If the broker service goes down, no charges will be sent. If redundancy was added:
- Run the risk of double charging.
- Don't want duplicate requests.

### Enter Leader Election

- Group machines doing the same thing.
- One is elected as the leader.
- Other followers are on standby.
- Leader goes down, another takes over.

However it is not a trivial task or problem to solve:
- Shared state.
- Who's the leader?
- Multiple machines need to agree on something - its the act of sharing consensus.

It requires the user of a consensus algorithm to achieve this. This is very math heavy and multiple nodes in cluster must agree:
- on some data value
- or who is now the leader


There are [two popular packages](https://www.youtube.com/watch?v=JQss0uQUc6o) that implement this consensus algorithm:
- Paxos
- Raft

If not using these 2, you'll use a 3rd party services that uses 1 of these under the hood.
- Zookeeper
- Etcd

These tools allow you to implement your own leader election in a simple way.

### Etcd
- key value store
- highly available and strongly consistent
- Achieve tis by using leader election using Raft

- Multiple services communicating w/ Etcd at any given point in time you would have 1 key value pair representing a leader.
- value is the IP Address of the server

**Lease** Allows Etcd to know whether a leader is running. Leader must keep refreshing the lease. If it expires (5 secs or so), the lease is revoked and another follower becomes the leader. If an exception occurs, the leader can revoke its own lease.

```
while has lease
    refresh lease
    do business logic
    ...
    if exception or keyboard_interupt
        revoke lease
```

**Server** Initially will try to insert key and if it succeeds, it becomes the leader. Otherwise it wait for another event.

**Callback event loop**: followers watch for delete event, key has expired...

