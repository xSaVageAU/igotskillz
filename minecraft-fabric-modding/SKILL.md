---
name: minecraft-fabric-modding
description: Expert guidance for Minecraft 26.1+ Fabric modding. Covers command registration, vanilla permissions, Mixins (injection/guards), Data Components, NBT, and configuration patterns.
---

# Minecraft Fabric Modding Expert (26.1+)

This skill provides a comprehensive set of patterns and standards for server-side modding in the 26.1+ environment. It is designed to ensure consistent, safe, and modern development practices.

## 1. Commands & Permissions
Register commands using the Fabric Command API v2 and prioritize vanilla permission levels for simplicity and compatibility.
- [Command Registration](references/commands.md)
- [Vanilla Permissions](references/permissions.md)

## 2. Event Handling
Hook into game lifecycle events, player connections, and server ticks.
- [Common Events](references/events.md)

## 3. Fabric Mixins & Safety
Modify core game logic safely using injection points, access wideners, and re-entrancy guards to prevent recursion loops.
- [Injection Points](references/injection-points.md)
- [Access Wideners](references/access-wideners.md)
- [Re-entrancy Guards](references/recursion-guards.md)

## 4. Data Management
Handle persistent state using the Data Component system for items, NBT for entities/world data, and GSON for mod settings.
- [Data Components](references/data-components.md)
- [NBT Serialization](references/nbt-serialization.md)
- [Configuration Patterns](references/config-patterns.md)

## Best Practices
- **Prefer Vanilla**: Use built-in Minecraft systems (like Permissions or Data Components) before adding external dependencies.
- **Async IO**: Never perform disk or network operations on the main server thread.
- **Prefixing**: Always prefix keys, commands, and identifiers with your mod ID.
