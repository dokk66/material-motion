---
layout: page
title: SpringTo
status:
  date: Oct 18, 2016
  is: Stable
knowledgelevel: L2
library: spring
depends_on:
  - /starmap/specifications/runtime/Plan
proposals:
  - proposal:
    initiation_date: Nov 6, 2016
    completion_date: Nov 7, 2016
    state: Accepted
    discussion: "Make SpringConfiguration a required property on SpringTo"
    discussion_url: https://github.com/material-motion/starmap/issues/67
availability:
  - platform:
    name: Android
    label: family-rebound-android as of v1.0.0
    url: https://github.com/material-motion/material-motion-family-rebound-android
  - platform:
    name: iOS
    label: family-pop-swift as of v1.0.0
    url: https://github.com/material-motion/material-motion-family-pop-swift
---

# SpringTo specification

## Example: Rounded corners

To animate a rounded-corners square to a new dimension:

    Interaction Morphing {
      let roundedCornerShape
      
      func setUp() {
        runtime.addPlan(SpringTo(.layerBounds, destination: bounds),
                        to: roundedCornerShape)
        runtime.addPlan(SpringTo(.layerCornerRadius, destination: radius),
                        to: roundedCornerShape)
      }
    }

## Contract

One or more one-dimensional springs pull a property's current value to a provided `destination`.

Configuration should be initialized to a default set of values as defined below.

```swift
Plan SpringTo {
  property
  destination
  configuration
}
```

## Types

### SpringConfiguration

Configures the behavior of a SpringTo plan.

```swift
SpringConfiguration {
  friction: float
  tension: float
}
```

This is an object because the two variables are very closely related.

### Default SpringConfiguration

Provide a default spring configuration on the SpringTo type.

The default friction is 30. The default tension is 342.

```swift
SpringTo {
  static let defaultConfiguration = SpringConfiguration(friction: 30, tension: 342)
}
```

## Performer considerations

Use springs to pull the target property's current value towards the last-provided destination.

Create state required to simulate a spring when `SpringTo` is provided for a new property.

Preserve velocity when a property's destination changes.
