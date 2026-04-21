# Common Mixin Injection Points (Minecraft 26.1+ Fabric)

In Minecraft 26.1+, several key classes are frequently used as injection targets for server-side logic.

## 1. Player Management (`PlayerList`)
Target: `net.minecraft.server.players.PlayerList`

### Handling Player Sync/Join/Leave
Use `@Inject` at the end of `placeNewPlayer` to run logic after a player has fully joined.

```java
@Mixin(PlayerList.class)
public abstract class MyPlayerSyncMixin {
    @Inject(method = "placeNewPlayer", at = @At("TAIL"))
    private void onPlayerJoin(Connection connection, ServerPlayer player, CommonListenerCookie cookie, CallbackInfo ci) {
        // Logic after player joins
    }
}
```

## 2. World & Block Logic (`Level`)
Target: `net.minecraft.world.level.Level`

### Intercepting Block Updates
Target `setBlock` or `sendBlockUpdated`. Note the new `BlockPos` and `BlockState` handling in 26.1+.

```java
@Mixin(Level.class)
public abstract class MyWorldSyncMixin {
    @Inject(method = "sendBlockUpdated", at = @At("HEAD"))
    private void onBlockUpdate(BlockPos pos, BlockState oldState, BlockState newState, int flags, CallbackInfo ci) {
        // Logic for block changes
    }
}
```

## 3. Data Storage (`PlayerDataStorage`)
Target: `net.minecraft.world.level.storage.PlayerDataStorage`

### Custom Player Data IO
Override `load` and `save` to intercept raw NBT data before it's processed by the game.

```java
@Mixin(PlayerDataStorage.class)
public class MyPlayerDataMixin {
    @Inject(method = "load", at = @At("RETURN"), cancellable = true)
    private void onPlayerLoad(ServerPlayer player, CallbackInfoReturnable<Optional<CompoundTag>> cir) {
        // Optional<CompoundTag> contains the loaded data
    }

    @Inject(method = "save", at = @At("HEAD"))
    private void onPlayerSave(ServerPlayer player, CallbackInfo ci) {
        // Logic before player data is written to disk
    }
}
```

## 4. Stats & Advancements
Targets: `net.minecraft.stats.ServerStatsCounter`, `net.minecraft.server.PlayerAdvancements`

Useful for syncing progress across a cluster.

```java
@Mixin(ServerStatsCounter.class)
public abstract class MyStatsMixin {
    @Inject(method = "save", at = @At("HEAD"))
    private void onStatsSave(CallbackInfo ci) {
        // Logic to upload stats
    }
}
```

## 5. Accessors
Use `@Accessor` to reach private fields or `@Invoker` for private methods without complex `@Inject` logic.

```java
@Mixin(Mannequin.class)
public interface MannequinAccessor {
    @Accessor("targetUuid")
    UUID getTargetUuid();

    @Accessor("targetUuid")
    void setTargetUuid(UUID uuid);
}
```
