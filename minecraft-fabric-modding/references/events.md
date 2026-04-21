# Event Handling in Minecraft 26.1+ (Fabric)

Fabric provides a wide range of events to hook into game logic, ranging from basic lifecycle hooks to advanced networking filters.

## 1. Server Lifecycle Events
Registered via `net.fabricmc.fabric.api.event.lifecycle.v1.ServerLifecycleEvents`.

### Basic Lifecycle
```java
ServerLifecycleEvents.SERVER_STARTED.register(server -> { /* Initial setup */ });
ServerLifecycleEvents.SERVER_STOPPING.register(server -> { /* Cleanup */ });
```

### World Save Hooks
Useful for auto-saves or manual `/save-all` triggers.
```java
ServerLifecycleEvents.AFTER_SAVE.register((server, flush, force) -> {
    // Logic after world data has been written to disk
});
```

## 2. Player Connection Events (Play Phase)
Registered via `net.fabricmc.fabric.api.networking.v1.ServerPlayConnectionEvents`.

### Join & Disconnect
```java
ServerPlayConnectionEvents.JOIN.register((handler, sender, server) -> {
    ServerPlayer player = handler.getPlayer();
});

ServerPlayConnectionEvents.DISCONNECT.register((handler, server) -> {
    // Logic when player leaves the "Play" state
});
```

## 3. Login & Authentication Events (Early Phase)
Registered via `net.fabricmc.fabric.api.networking.v1.ServerLoginConnectionEvents`. These are critical for mods that perform authentication, data pre-fetching, or early-stage player blocking.

### Early Initialization
```java
ServerLoginConnectionEvents.INIT.register((handler, server) -> {
    if (/* cluster offline */) {
        handler.disconnect(Component.literal("Service Unavailable"));
    }
});
```

### Async Data Fetching (`QUERY_START`)
Allows you to pause the login process while performing asynchronous tasks (like fetching data from a database).
```java
ServerLoginConnectionEvents.QUERY_START.register((handler, server, sender, synchronizer) -> {
    CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
        // Fetch data...
    });
    // Tell the server to wait for this future before continuing
    synchronizer.waitFor(future);
});
```

### Login Disconnect
Handles cases where a player’s connection is lost *before* they fully join the game world.
```java
ServerLoginConnectionEvents.DISCONNECT.register((handler, server) -> {
    // Release early locks or clean up pre-fetched memory
});
```

## 4. Configuration Phase Events
Registered via `net.fabricmc.fabric.api.networking.v1.ServerConfigurationConnectionEvents`. This phase occurs between Login and Play.

```java
ServerConfigurationConnectionEvents.DISCONNECT.register((handler, server) -> {
    // Final cleanup if they drop during the loading screen
});
```

## 5. Messaging & Chat
Registered via `net.fabricmc.fabric.api.message.v1.ServerMessageEvents`.

```java
ServerMessageEvents.CHAT_MESSAGE.register((message, sender, params) -> {
    String chatText = message.signedContent();
});
```

## Best Practices
1.  **Thread Safety**: Events like `QUERY_START` often run logic asynchronously. Use `server.execute(() -> { ... })` if you need to run code back on the main server thread.
2.  **Synchronizer Usage**: Always use `synchronizer.waitFor()` for async login tasks to prevent the player from joining before their data is ready.
3.  **Graceful Disconnects**: When disconnecting players in early phases (INIT/QUERY), provide clear, localized components as reasons.
