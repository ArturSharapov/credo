# Functions and ownership

The Rust fragments illustrate boundaries; surrounding domain types and existing interfaces are omitted. They are not a proposed framework. Preserve the requirements in your implementation, not the example names or method count.

## A behavioral definition contributes through its existing protocol

Requirement: a configured retention policy discards expired records, except records protected by an active hold. `Retention` is a definition type. `Select` is its declared protocol; `holds` is a stable view of another component's state.

Bad: two layers of extra methods hide the actual policy and add an inherent behavior interface.

```rust
impl Retention {
    fn keep(&self, record: &Record, state: &State) -> bool {
        !self.expired(record, state.now) || state.holds.contains(record.id)
    }
    fn expired(&self, record: &Record, now: u64) -> bool {
        record.expires_at <= now
    }
}
impl Select for Retention {
    fn retain(&self, state: &State, records: &mut Vec<Record>) {
        records.retain(|record| self.keep(record, state));
    }
}
```

Good: the protocol implementation contains its condition and uses the stable state view directly.

```rust
impl Select for Retention {
    fn retain(&self, state: &State, records: &mut Vec<Record>) {
        // Keep unexpired records and records protected by a hold.
        records.retain(|record| {
            record.expires_at > state.now || state.holds.contains(record.id)
        });
    }
}
```

Do not move `keep` into a free function, a new trait, or `RetentionHelpers`. Do not add `Holds::keep_for_retention`; the consumer owns that policy. If holds themselves have a nontrivial invariant, their existing neutral operations can enforce it without knowing retention policy.

Different case: an existing batch writer must flush its pending records even when no flush settings are provided. An optional settings definition holds the batch-size limit. The writer reads that state and applies its default when absent. Keep the definition as a state holder; creating a `FlushPolicy` trait solely for that one definition would add an interface without a composition or substitution requirement.

## Several calls do not justify a trivial wrapper

Bad: extracting `fn remaining(&self) -> usize { self.limit - self.used }` solely because two callers use that expression. It removes no substantial logic and introduces another name to follow. Keep the expression at the consumers where `used <= limit` is already guaranteed.

Different case: two production paths reserve intervals in the same bounded buffer. They must validate a range, split a free interval, preserve ordering, and coalesce adjacent intervals when released. Substantial repeated manipulation of that invariant belongs in the existing buffer owner. Its `reserve` operation earns a method; it is not a renamed subtraction. Do not place it on a protocol-only definition type.

A value type in the same module may own such operations. Directory placement alone does not make the value a definition type. Necessary public entry points and existing trait methods are evaluated by their contract, not by an arbitrary call-count threshold.

## Preserve a transaction's sequence without fragmenting its internals

Requirement: validate a batch, reserve capacity, write its records, then publish it. `Batch` already owns the reservation and pending records. Each stage crosses a meaningful state boundary.

Good orchestration:

```rust
let mut batch = Batch::create(store, request)?;
batch.reserve()?;
batch.write()?;
batch.publish()
```

Bad fragmentation inside `reserve`:

```rust
self.read_requested_count();
self.read_remaining_capacity();
self.compare_counts()?;
self.set_reserved_count();
```

Good internal shape:

```rust
if self.requested > self.store.limit - self.store.used {
    return Err(Error::Capacity);
}
self.reserved = self.requested;
```

The accepted stage methods expose a real lifecycle. The four micro-methods only obscure local logic. Conversely, inlining all of `create`, `reserve`, `write`, and `publish` into one caller would destroy the useful flow. Moving those micro-methods into a new empty `CapacityService` also fails: it adds a type without owning the store or an invariant.

Review test: for each added function, identify its real protocol/lifecycle role or its substantial repeated logic. For each added type, identify its owned state or distinct invariant. A persuasive name is not evidence of either.
