# Event Handling in Minecraft 26.1+ (Fabric)

Fabric provides a wide range of events to hook into game logic.

## Common Server Events

These are registered in `onInitialize` or `onInitializeServer`.

### Lifecycle Events

```java
import net.fabricmc.fabric.api.event.lifecycle.v1.ServerLifecycleEvents;

// Server started
ServerLifecycleEvents.SERVER_STARTED.register(server -> {
    // Logic here
});

// Server stopping
ServerLifecycleEvents.SERVER_STOPPING.register(server -> {
    // Logic here
});
```

### Player Events

```java
import net.fabricmc.fabric.api.networking.v1.ServerPlayConnectionEvents;

// Player joined
ServerPlayConnectionEvents.JOIN.register((handler, sender, server) -> {
    ServerPlayer player = handler.getPlayer();
    // Logic here
});

// Player disconnected
ServerPlayConnectionEvents.DISCONNECT.register((handler, server) -> {
    // Logic here
});
```

### Messaging Events

```java
import net.fabricmc.fabric.api.message.v1.ServerMessageEvents;

// Chat message received
ServerMessageEvents.CHAT_MESSAGE.register((message, sender, params) -> {
    String playerName = sender.getName().getString();
    String chatText = message.signedContent();
    // Logic here
});
```

### Tick Events

```java
import net.fabricmc.fabric.api.event.lifecycle.v1.ServerTickEvents;

// Start of server tick
ServerTickEvents.START_SERVER_TICK.register(server -> {
    if (server.getTickCount() % 20 == 0) {
        // Run every second
    }
});
```

## Best Practices

1. **Keep Listeners Lightweight**: Avoid heavy computations directly in event listeners. Use asynchronous tasks if necessary.
2. **Event Priority**: Some events allow setting a priority if multiple mods listen to the same event.
3. **Thread Safety**: Ensure that any state modified inside a tick event is thread-safe or only accessed from the main server thread.
