---
layout: page
title: MotionAggregator recursive aggregation
status:
  date: December 4, 2016
  is: Draft
knowledgelevel: L3
library: streams
depends_on:
  - /starmap/specifications/streams/MotionAggregator
---

# MotionAggregator recursive aggregation feature specification

This is the engineering specification for creating recursive MotionAggregators.

## Overview

A `MotionAggregator` can generate child instances of `MotionAggregator`. Streams registered with
child instances will also affect the active state of their parents, enabling the expression of
recursive aggregation and is-active monitoring.

## MVP

### Expose createChild API

Should accept an optional name.

```swift
class MotionAggregator<T>: MotionObservable<T> {
  public func createChild(named name: String? = nil) -> MotionAggregator
}
```

### Store the child in the parent

The child should have a weak reference to its parent. The parent should have strong references to
all of its children.

```swift
class MotionAggregator<T>: MotionObservable<T> {

  private weak var parent: MotionAggregator?
  private var children: [MotionAggregator] = []

  func createChild(named name: String? = nil) -> MotionAggregator {
    let child = MotionAggregator(parent: self, named: name)
    children.append(child)
    return child
  }
```

### Report active state changes to the parent

When `active` is invoked on an aggregator it should invoke its parent's `active` method as well.

```swift
class MotionAggregator<T>: MotionObservable<T> {

  private func active(_ token: Token) -> (Bool) -> Void {
    return { [weak self] in
      guard let strongSelf = self else { return }
      if $0 {
        strongSelf.activeTokens.insert(token)
      } else {
        strongSelf.activeTokens.remove(token)
      }
      strongSelf.active = strongSelf.activeTokens.count > 0

      // Recurse upwards to the parent.
      strongSelf.parent?._active(token)($0)
    }
  }
```
