# Issue Shape

The TDD-ready format the skill writes into each rewritten issue body.
The issue tracker is authoritative for issue bodies; the Design Plan
never duplicates this content (it links to the issue instead).

A TDD-ready issue is a unit of work that `/tdd` can execute without
asking "what should I test?" It names the observable behavior, the
entry point, and the key edge cases.

---

## The Problem with Raw Issues

Issues from `/to-issues` describe *what to build* at a feature level.
They are correct, but they are not TDD-ready because they don't specify:

- Which module owns the behavior
- What the test entry point is
- Which behaviors to test first
- What edge cases matter
- Where to put fakes

A raw issue is a destination. A TDD-ready issue is a map.

---

## The Format

Each rewritten issue body contains, in order:

```
**Module**: <module name>

**Behavior**: As a <actor>, I can <behavior> so that <value>.

**Acceptance criteria**:
- Given <precondition>, when <action>, then <observable outcome>
- Given <different precondition>, when <action>, then <different outcome>
- ...

**TDD notes**:
- Entry point: <public function / command / endpoint>
- Test first: <most critical behavior>
- Edge cases: <list>
- Fake strategy: <what to fake, at which seam>
- Must NOT test: <internal details to avoid coupling>
```

The body replaces (does not extend) the raw `/to-issues` content. The
issue title can stay; the body is rewritten.

---

## Before and After Example

### Before (raw issue from `/to-issues`)

```
Title: Add order fulfillment
Body:  Implement the ability to fulfill orders once payment is confirmed.
       Should integrate with the shipment service and send a
       confirmation email.
```

### After (`/software-design` splits and rewrites into two issues)

**Issue A — OrderFulfillment**

```
**Module**: OrderFulfillment

**Behavior**: As the system, I can fulfill a confirmed order so that
inventory is reserved and a shipment is dispatched.

**Acceptance criteria**:
- Given a confirmed order, when fulfill(orderId) is called,
  then inventory is reserved and ShipmentDispatch.send() is called.
- Given an already-fulfilled order, when fulfill(orderId) is called
  again, then no duplicate shipment is dispatched (idempotent).
- Given a confirmed order where inventory is insufficient,
  when fulfill(orderId) is called, then a FulfillmentFailed result is
  returned.

**TDD notes**:
- Entry point: fulfillOrder(orderId, { inventorySeam, shipmentSeam })
- Test first: happy path — confirmed order → reserved + dispatched
- Edge cases: idempotency, insufficient inventory, missing order
- Fake strategy: FakeInventoryReservation, FakeShipmentDispatch
- Must NOT test: how shipment is sent internally (FedEx vs UPS)
```

**Issue B — NotificationSender**

```
**Module**: NotificationSender

**Behavior**: As a customer, I receive a confirmation notification
when my order ships so that I know when to expect delivery.

**Acceptance criteria**:
- Given a ShipmentDispatched event, when the notification handler
  processes it, then a confirmation is sent to the customer's
  preferred channel.
- Given a customer with no preferred channel set, when the event is
  processed, then the notification is skipped and a warning is logged.

**TDD notes**:
- Entry point: handleShipmentDispatched(event)
- Test first: customer receives notification on happy path
- Edge cases: missing preferred channel, duplicate event (idempotency)
- Fake strategy: FakeNotificationGateway at the outbound boundary
- Must NOT test: email template markup or push payload format
```

---

## Splitting Rules

Split an issue whenever it:

- Touches more than one module.
- Describes both a domain behavior *and* an integration step (e.g.
  "process payment and publish event").
- Mixes a core behavior with an edge case that belongs to a different
  flow.
- Contains the word "and" describing two distinct user-facing outcomes.

When you split, each resulting issue is independently implementable —
one does not require the other to be done first unless it is a real
data-flow dependency.

---

## Ordering Issues Within an Epic

After rewriting all issues, order them:

1. **Tracer bullet first** — the issue that proves the path works
   end-to-end.
2. **Core behavior** — happy-path issues for each module.
3. **Edge cases** — error paths, invariant violations, idempotency.
4. **Integration** — issues that wire adapters to real implementations.

Issues in the same module can often be worked in parallel. Issues that
cross a seam must respect the dependency order.

---

## TDD Notes Anti-Patterns

**"Test that it calls X"** — tests implementation, not behavior.
Replace with "test that the observable outcome Y occurs."

**"Fake the internal helper Z"** — helpers are not seams. If a helper
needs isolation, extract it as a module with its own interface.

**"Test via the database"** — tests should use the module's public
interface. Verifying through the database couples tests to persistence
details.

**"Test all edge cases in one issue"** — each issue is one RED→GREEN
cycle for `/tdd`. Keep each issue to one logical behavior. Edge cases
get their own issues.
