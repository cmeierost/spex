# Intent-Driven UI

## The Core Idea

Spex does not focus on how a UI should look. The design direction is to describe **what an actor intends to do**, **when that action is allowed**, and **what business state change it means**, so that the UI can become a passive reflection of those capabilities.

This is the same functional/nonfunctional boundary in UI form. **User intents, allowed actions, observable business state, and allowed state modifications are functional requirements.** Rendering, layout, device interaction, accessibility implementation, latency, input methods, and other human-computer interaction mechanics are nonfunctional and infrastructural.

The spec should not say that a drawer is open, a button is enabled, or a modal is visible. It should say that an actor may perform an action when its preconditions hold. The interface can then choose how to make that possible for a human.

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

**Result:** In the target model, the UI would receive available actions from the portable business engine. Buttons, menus, drawers, forms, and gestures would be rendering choices derived from what the actor can do — not business rules in themselves.

## How It Works

### 1. Spec Declares Permissions

The specification would declare, for each actor type, what actions are permitted under what conditions.

### 2. Business Engine Computes Available Actions

Given the current state and the current actor, the portable business engine would evaluate the permission rules to produce a set of available actions.

### 3. UI Renders the Mirror

The UI framework (any framework — React, Vue, native, CLI) would receive the available actions and render them. The UI would stay "dumb" with respect to business rules, knowing only what to display.

The business engine should decide whether an actor may perform an action and what state change that action means. The UI is infrastructure and should decide how that possibility is presented and how the human expresses the intent.

## Benefits

- **No UI drift** — the interface should never show an action the spec doesn't allow
- **Replaceable UIs** — swap the frontend without changing business logic
- **Proactive guidance** — instead of "error: cannot ship," the UI could show "to ship this order, you need: 2 more items in stock"
- **Accessibility by default** — the spec's language could become the UI's explanatory text
