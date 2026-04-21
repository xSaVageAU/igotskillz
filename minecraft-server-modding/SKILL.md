---
name: minecraft-server-modding
description: Essential patterns and workflows for building server-side Minecraft mods on Fabric for version 26.1+. Use this when creating commands, handling permissions, listening to events, or working with Data Components.
---

# Minecraft Server Modding (26.1+ Fabric)

This skill provides the essential patterns and standards for developing server-side mods in the Minecraft 26.1+ environment using Fabric. It prioritizes the "vanilla" way of handling permissions and the modern Data Component system.

## Quick Reference

### Command Registration
Use `CommandRegistrationCallback.EVENT` to register commands. See [commands.md](references/commands.md) for the full registration pattern and argument types.

### Permissions
Prioritize using `net.minecraft.server.permissions.Permissions` for vanilla-style permission levels.
- `COMMANDS_ADMIN`: Administrative commands.
- `COMMANDS_MODERATOR`: Moderation commands.
See [permissions.md](references/permissions.md) for details and third-party API compatibility.

### Events
Common hooks include `SERVER_STARTED`, `JOIN`, and `CHAT_MESSAGE`. See [events.md](references/events.md) for common event registration patterns.

### Data Components
Structured data for items (replacing NBT). Use `DataComponents` and `CustomData`. See [data-components.md](references/data-components.md) for getting/setting data on items.

## Workflows

### Creating a New Command
1. Define a class with a static `register` method.
2. Hook into `CommandRegistrationCallback.EVENT` in your `ModInitializer`.
3. Use `.requires()` with a vanilla permission check.
4. Implement the execution logic using `CommandContext<CommandSourceStack>`.

### Handling Custom Item Data
1. Check if you should use `CUSTOM_DATA` for simple tags or register a custom `DataComponentType` for type-safe data.
2. Use `stack.get()` and `stack.set()` to interact with the components.

## Reference Files
- [Command Registration Guide](references/commands.md)
- [Permissions Guide](references/permissions.md)
- [Event Handling Guide](references/events.md)
- [Data Components Guide](references/data-components.md)
