# Event Handling in Minecraft 26.1+ (Fabric)

Fabric provides a wide range of events to hook into game logic, ranging from basic lifecycle hooks to advanced networking and interaction filters.

## 1. Server Lifecycle Events
Registered via `net.fabricmc.fabric.api.event.lifecycle.v1.ServerLifecycleEvents`.

### Basic Lifecycle
```java
ServerLifecycleEvents.SERVER_STARTED.register(server -> { /* After server has loaded world */ });
ServerLifecycleEvents.SERVER_STOPPING.register(server -> { /* Before server starts shutting down */ });
ServerLifecycleEvents.SERVER_STOPPED.register(server -> { /* After server has fully stopped */ });
```

### World Save Hooks
```java
ServerLifecycleEvents.AFTER_SAVE.register((server, flush, force) -> {
    // Logic after world data (and player data) has been written to disk
});
```

## 2. Player Connection Events
Registered via `net.fabricmc.fabric.api.networking.v1.ServerPlayConnectionEvents`.

### Play Phase
```java
ServerPlayConnectionEvents.JOIN.register((handler, sender, server) -> {
    ServerPlayer player = handler.getPlayer();
});

ServerPlayConnectionEvents.DISCONNECT.register((handler, server) -> {
    // Logic when player leaves the "Play" state
});
```

### Early Phase (Login & Config)
Registered via `ServerLoginConnectionEvents` and `ServerConfigurationConnectionEvents`.
- `INIT`: Early login rejection.
- `QUERY_START`: Async data pre-fetching (use `synchronizer.waitFor(future)`).
- `DISCONNECT`: Cleanup if connection lost before "Play" phase.

## 3. World Interaction Events

### Item & Block Interaction
Registered via `net.fabricmc.fabric.api.event.player.UseItemCallback` and `UseBlockCallback`.
```java
UseItemCallback.EVENT.register((player, world, hand) -> {
    // Logic when a player right-clicks with an item
    return TypedActionResult.pass(player.getItemInHand(hand));
});

UseBlockCallback.EVENT.register((player, world, hand, hitResult) -> {
    // Logic when a player right-clicks a block
    return ActionResult.PASS;
});
```

### Block Breaking
Registered via `net.fabricmc.fabric.api.event.player.PlayerBlockBreakEvents`.
```java
PlayerBlockBreakEvents.BEFORE.register((world, player, pos, state, blockEntity) -> {
    // Return false to cancel block breaking
    return true;
});
```

## 4. Player Gameplay Events
Registered via `net.fabricmc.fabric.api.entity.event.v1.ServerPlayerEvents`.

### Respawn
```java
ServerPlayerEvents.AFTER_RESPAWN.register((oldPlayer, newPlayer, alive) -> {
    // Sync data or update state for the new player instance
});
```

## 5. Messaging & Chat Filters
Registered via `net.fabricmc.fabric.api.message.v1.ServerMessageEvents`.

```java
ServerMessageEvents.CHAT_MESSAGE.register((message, sender, params) -> {
    // Logic for received chat messages
});

ServerMessageEvents.ALLOW_CHAT_MESSAGE.register((message, sender, params) -> {
    // Return false to block the message from being sent
    return true;
});
```

## 6. Tick Events
Registered via `net.fabricmc.fabric.api.event.lifecycle.v1.ServerTickEvents`.

```java
ServerTickEvents.START_SERVER_TICK.register(server -> {
    // Logic at the beginning of every tick
});

ServerTickEvents.END_SERVER_TICK.register(server -> {
    // Logic at the end of every tick
});
```

## Best Practices
1.  **Return Values**: Interaction events (like `UseItemCallback`) often require a specific return type (`ActionResult` or `TypedActionResult`). Ensure you return `.PASS` unless you are explicitly handling/canceling the action.
2.  **Thread Safety**: For async networking (`QUERY_START`), use `server.execute()` to return to the main thread for game logic.
3.  **Lightweight Ticks**: Tick events run 20 times per second. Minimize logic here or use a modulo check (`server.getTickCount() % 20 == 0`) to run tasks periodically.
