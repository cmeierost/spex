# Intent-Driven UI

## The Core Idea

Spex does not describe how a UI should look. It describes **what an actor is allowed to do**, and the UI becomes a passive reflection of that capability.

## Traditional UI vs Intent-Driven UI

### Traditional (Imperative)

```
// Developer manually wires up every state
if (order.status === 'pending') {
  shipButton.disabled = true;
  cancelButton.visible = true;
}
if (order.status === 'confirmed') {
  shipButton.disabled = false;
  cancelButton.visible = false;
}
// ... dozens more branches
```

**Problems:**
- UI state diverges from business logic
- New states break existing UI code
- Permissions scattered across components

### Spex (Declarative)

```
An Actor may Ship an Order if and only if:
  - the Order is Confirmed; and
  - all Items of the Order are Available; and
  - the Actor holds the Warehouse role.
```

**Result:** The UI computes available actions from the spec. Buttons, menus, and forms are generated from what the actor can do — not from hardcoded state machines.

## How It Works

### 1. Spec Declares Permissions

The specification declares, for each actor type, what actions are permitted under what conditions.

### 2. Runtime Computes Available Actions

Given the current state and the current actor, the runtime evaluates all permission rules to produce a set of available actions.

### 3. UI Renders the Mirror

The UI framework (any framework — React, Vue, native, CLI) receives the available actions and renders them. The UI is "dumb" — it doesn't know about business rules, only about what to display.

## Benefits

- **No UI drift** — the interface can never show an action the spec doesn't allow
- **Replaceable UIs** — swap the frontend without changing business logic
- **Proactive guidance** — instead of "error: cannot ship," the UI shows "to ship this order, you need: 2 more items in stock"
- **Accessibility by default** — the spec's language becomes the UI's explanatory text
