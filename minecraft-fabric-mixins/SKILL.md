---
name: minecraft-fabric-mixins
description: Expertise in Fabric Mixin syntax, common injection points, and access wideners for Minecraft 26.1+. Use this when modifying internal game logic, accessing private members, or implementing safety guards for core game changes.
---

# Fabric Mixins (26.1+)

This skill covers the advanced usage of SpongePowered Mixins within the Fabric environment for Minecraft 26.1+. It provides patterns for common injection targets and safety techniques for modifying core game logic.

## Quick Start

### 1. Basic Mixin Structure
```java
@Mixin(TargetClass.class)
public class MyMixin {
    @Inject(method = "targetMethod", at = @At("HEAD"))
    private void myLogic(CallbackInfo ci) {
        // Code here
    }
}
```

### 2. Preventing Infinite Loops
When a modification triggers the code it's injecting into, use a `ThreadLocal` guard to prevent recursion. See [recursion-guards.md](references/recursion-guards.md).

## Core Concepts

### Injection Points
Common targets include `PlayerList` for join/leave, `Level` for world changes, and `PlayerDataStorage` for saving data. See [injection-points.md](references/injection-points.md).

### Access Wideners
The preferred way to make private members accessible without the overhead of Mixin Accessors for general use. See [access-wideners.md](references/access-wideners.md).

## Reference Files
- [Common Injection Points](references/injection-points.md)
- [Access Widener Guide](references/access-wideners.md)
- [Re-entrancy Guards](references/recursion-guards.md)
