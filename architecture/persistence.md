# State Responsibility and Persistence Model

## Writing Is Infrastructure

In Spex, the mechanism of data persistence is invisible to the specification. The spec declares what entities exist, what rules govern them, and what responsibility the system has for their state. The surrounding infrastructure decides how to store, recover, observe, or recompute that state for the portable, distributable business engine.

That storage choice is an implementation of nonfunctional requirements, not business meaning. A database engine, indexing strategy, replication mode, or caching layer exists to satisfy concerns such as performance, durability, availability, and cost.

The responsibility for state is different. Whether the system must keep state, may forget it, or relies on another authority for it is specification meaning because it changes the business contract.

| State responsibility | Meaning in the spec | Implementation remains physical |
|----------------------|---------------------|---------------------------------|
| Must be kept | Losing the state would violate the contract | Database, event log, snapshot, replicated store |
| May be forgotten | The state may expire, be recomputed, or be requested again without violating the contract | Memory, cache, temporary table |
| Provided by another authority | The system is not responsible for maintaining the truth of the state; it uses an authoritative value under declared rules | API, message bus, import, human input |

Forgettable state should still name its allowed lifetime. It may live for a process, a user session, a transaction, or another declared boundary. The implementation may use memory or a cache, but the specification defines when forgetting is allowed.

Outside authorities should be declared as named concepts. If Payment Provider is authoritative for Payment Status, both the authority and the value should be defined in the specification before rules depend on them.

## How It Works

### 1. Spec Declares Entities

```
An Order shall consist of:
  - an Order Number;
  - a Customer;
  - at least one Item; and
  - a Status.
```

### 2. Infrastructure Maps State to Storage

Infrastructure around the portable, distributable business engine creates or selects the physical storage structure:

```
Infrastructure decision (invisible to spec):
  → PostgreSQL table: orders
  → Columns: order_number (PK), customer_id (FK), status
  → Separate table: order_items (junction)
  → Index on customer_id for query performance
```

### 3. Actions Update the Mathematical Model

When an action executes (e.g., "Ship Order"), it changes the state of the business model. The infrastructure connected to the relevant boundary ports handles the physical write.

The physical write path may vary because nonfunctional requirements vary. One deployment may optimize for strict consistency and auditability, while another may optimize for latency or regional failover, without changing the business contract.

## Reading Is Business Logic

The specification defines **who can see what** using declarative views:

```
A Customer may view an Order if and only if:
  - the Order belongs to the Customer.

A Warehouse Agent may view an Order if and only if:
  - the Order is Confirmed; or
  - the Order is Shipped.
```

These are not SQL queries — they are logical filters evaluated by the portable, distributable business engine. Infrastructure may implement projections or indexes to satisfy them efficiently, but those are physical choices.

## Key Properties

| Property | Description |
|----------|-------------|
| **Storage-agnostic** | Spec doesn't know if state lives in SQL, NoSQL, an event log, or memory |
| **Migration-independent** | Schema migration is an infrastructure concern, not a business rule |
| **Permission-bound** | Every read is filtered by the actor's permissions |
| **Contract-bound** | Physical storage must satisfy the signed state responsibilities |
