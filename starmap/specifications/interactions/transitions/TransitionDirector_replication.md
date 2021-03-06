---
layout: page
title: "TransitionDirector replication"
status:
  date: Oct 25, 2016
  is: Drafting
---

# TransitionDirector replication feature specification

**ReplicaController API**: Transition directors have a private read-only `replicaController` API.

Provide the replica controller to the director's initializer.

This API is not accessible to sub-classes.

Example pseudo-code:

```swift
TransitionDirector {
  private readonly var replicaController
  init(replicaController)
```

**ReplicaControllerDelegate API**: Transition directors can assign a `replicaControllerDelegate`.

Subclasses are expected to set a custom replica controller delegate using this API.

Example pseudo-code:

```swift
TransitionDirector {
  var replicaControllerDelegate
}
```
