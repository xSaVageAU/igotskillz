---
name: minecraft-data-management
description: Patterns for managing data in Minecraft 26.1+, including NBT manipulation, Data Components, and custom configuration. Use this when handling item data, player state, or mod settings.
---

# Minecraft Data Management

This skill covers the standard patterns for handling data in Minecraft 26.1+. It focuses on interacting with the game's internal data structures (NBT and Data Components) and implementing clean configuration systems.

## Quick Start

### 1. Item Data (Data Components)
Use `stack.get(DataComponents.CUSTOM_DATA)` for arbitrary mod data on items. See [data-components.md](references/data-components.md) (shared reference).

### 2. Global Data (NBT)
Use `CompoundTag` for persistent data attached to entities, tiles, or world saves. See [nbt-serialization.md](references/nbt-serialization.md).

### 3. Mod Configuration
Use GSON-based JSON files for mod-specific settings. See [config-patterns.md](references/config-patterns.md).

## Core Concepts

### NBT Serialization
Converting between binary tags and Java objects. See [nbt-serialization.md](references/nbt-serialization.md).

### Configuration Lifecycle
Loading, saving, and providing default values for mod settings. See [config-patterns.md](references/config-patterns.md).

## Reference Files
- [Configuration Patterns](references/config-patterns.md)
- [NBT Serialization](references/nbt-serialization.md)
