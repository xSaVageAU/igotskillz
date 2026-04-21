# Permissions in Minecraft 26.1+ (Fabric)

Minecraft 26.1+ introduces a more streamlined way to handle permissions directly through the `CommandSourceStack`.

## Vanilla Permissions (Recommended)

Use `source.permissions().hasPermission(Permissions.<LEVEL>)` for standard permission checks. This is the "proper" way to handle permissions in 26.1+ without external dependencies for basic needs.

### Available Levels

- `Permissions.COMMANDS_ADMIN`: Equivalent to OP level 2-4 (standard administrative commands).
- `Permissions.COMMANDS_MODERATOR`: Equivalent to OP level 1-2 (moderation commands).
- `Permissions.COMMANDS_EVERYONE` (if available, or simply omit `.requires()`): No special permissions required.

### Example Usage in Commands

```java
import net.minecraft.server.permissions.Permissions;

// ... inside register method
.requires(source -> source.permissions().hasPermission(Permissions.COMMANDS_ADMIN))
```

## Fabric Permissions API (for Compatibility)

If compatibility with third-party permission managers like LuckPerms is required, use `me.lucko.fabric.api.permissions.v0.Permissions`.

### Dependency (build.gradle)

```gradle
repositories {
    maven { url 'https://oss.sonatype.org/content/repositories/snapshots' }
}

dependencies {
    modImplementation "me.lucko:fabric-permissions-api:0.2.1"
}
```

### Usage

```java
import me.lucko.fabric.api.permissions.v0.Permissions;

// ... inside register method
.requires(source -> Permissions.check(source, "mymod.command.admin", 2))
```

The third parameter is the fallback OP level if no permission manager is present.

## Best Practices

1. **Use Vanilla Levels for Simplicity**: If the mod doesn't need fine-grained permissions, stick to `COMMANDS_ADMIN` and `COMMANDS_MODERATOR`.
2. **Fallback Levels**: Always provide a sensible fallback OP level when using third-party APIs.
3. **Permission Nodes**: Use consistent naming for permission nodes (e.g., `modid.command.name`).
