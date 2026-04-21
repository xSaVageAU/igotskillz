# Re-entrancy & Recursion Guards

When modifying core game logic with Mixins, you may encounter "re-entrancy" issues—where your modification triggers the same method you are injecting into, causing an infinite loop or a `StackOverflowError`.

## The Guard Pattern

The standard way to prevent this in Minecraft's multi-threaded environment is using a `ThreadLocal<Boolean>`.

### 1. Define the Guard
Place the guard in a utility class or your main mod class.

```java
public class MyMod {
    public static final ThreadLocal<Boolean> isProcessing = ThreadLocal.withInitial(() -> false);
}
```

### 2. Apply the Guard in a Mixin
Check the guard at the "HEAD" of your injection.

```java
@Inject(method = "someInternalMethod", at = @At("HEAD"), cancellable = true)
private void onInternalMethod(CallbackInfo ci) {
    if (MyMod.isProcessing.get()) return; // Exit if we are already in this call stack

    MyMod.isProcessing.set(true);
    try {
        // Perform your logic that might trigger 'someInternalMethod' again
    } finally {
        MyMod.isProcessing.set(false);
    }
}
```

## Common Scenarios
*   **Block Updates**: Modifying a block in response to a block change event.
*   **Inventory Changes**: Moving items when an inventory is accessed.
*   **Entity Spawning**: Spawning an entity inside a spawn event.

## Best Practices
*   **Always use `finally`**: This ensures the guard is reset even if your code throws an exception, preventing the mod from "locking up" that logic for the rest of the session.
*   **Thread Safety**: Always use `ThreadLocal` rather than a simple `static boolean` to ensure that logic on different threads (like async world gen vs. main tick) doesn't interfere with each other.
