# Persistence Model

## Writing Is Infrastructure

In Spex, data persistence is entirely invisible to the specification. The spec declares what entities exist and what rules govern them — the runtime decides how to store them.

## How It Works

### 1. Spec Declares Entities

```
An Order shall consist of:
  - an Order Number;
  - a Customer;
  - at least one Item; and
  - a Status.
```

### 2. Runtime Maps to Storage

The runtime autonomously creates the physical storage structure:

```
Runtime decision (invisible to spec):
  → PostgreSQL table: orders
  → Columns: order_number (PK), customer_id (FK), status
  → Separate table: order_items (junction)
  → Index on customer_id for query performance
```

### 3. Actions Update the Mathematical Model

When an action executes (e.g., "Ship Order"), it only changes the state of the mathematical model. The persistence manager handles the physical write.

## Reading Is Business Logic

The specification defines **who can see what** using declarative views:

```
A Customer may view an Order if and only if:
  - the Order belongs to the Customer.

A Warehouse Agent may view an Order if and only if:
  - the Order is Confirmed; or
  - the Order is Shipped.
```

These are not SQL queries — they are logical filters evaluated by the view engine at runtime.

## Key Properties

| Property | Description |
|----------|-------------|
| **Storage-agnostic** | Spec doesn't know if data lives in SQL, NoSQL, or RAM |
| **Migration-free** | Change the spec, the runtime adapts the storage |
| **Permission-bound** | Every read is filtered by the actor's permissions |
| **Consistent** | The mathematical model is the source of truth, not the database |
