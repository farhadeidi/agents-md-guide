# Simplicity First

- **Status:** Optional
- **Common best home:** Personal, team, or project guidance

## Intent

Choose the least complex implementation that fully meets current requirements.
Avoid speculative abstraction, configuration, compatibility layers, and
extension points that have no demonstrated consumer.

Simplicity includes the whole system. A slightly larger use of an established
library can be simpler than a smaller custom implementation that the project
must maintain.

## Good fit

- New features with clear present-day requirements
- Small or rapidly evolving products
- Repositories prone to premature frameworks or single-use abstractions
- Tasks where existing dependencies already provide the needed capability

## Skip or adjust when

- A public contract already requires compatibility
- Security, reliability, or regulatory requirements demand explicit controls
- Multiple current consumers demonstrate a real abstraction boundary
- The task explicitly includes a planned migration or supported extension point

## Suggested instruction

```md
Implement the simplest solution that fully meets the current requirements. Do
not add speculative features, configuration, compatibility layers, or
abstractions without a concrete present need. Reuse established project
capabilities when they reduce total complexity.
```

## Trade-off

This pattern reduces maintenance cost and overengineering. Applied without
context, it can underrepresent real compatibility, reliability, or multi-consumer
requirements and create a deceptively small local solution with larger system
costs.
