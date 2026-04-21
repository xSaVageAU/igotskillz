# Access Wideners (Fabric)

Access Wideners allow you to make classes, fields, and methods accessible (or mutable) without using Mixins or reflection. This is often more efficient and cleaner for simple accessibility changes.

## Configuration

1.  Create a file named `modid.accesswidener` in `src/main/resources`.
2.  Register it in `fabric.mod.json`:

```json
{
  "accessWidener": "modid.accesswidener"
}
```

## Syntax

The syntax is: `<access> <type> <class> <member> <descriptor>`

### 1. Making a Field Accessible
Use `accessible field` to allow reading and writing to a private field.

```text
accessWidener v2 official
accessible field net/minecraft/server/network/ServerLoginPacketListenerImpl authenticatedProfile Lcom/mojang/authlib/GameProfile;
```

### 2. Making a Method Accessible
Use `accessible method` to allow calling a private method.

```text
accessible method net/minecraft/world/entity/LivingEntity tickEffects ()V
```

### 3. Making a Class Accessible
Use `accessible class` to allow extending or instantiating a private class.

```text
accessible class net/minecraft/world/level/storage/PlayerDataStorage
```

### 4. Making a Field Mutable (transitive)
Use `transitive-accessible field` if you need the accessibility to persist through subclasses.

## Best Practices

1.  **Prefer Mixin Accessors** for one-off field access if you're already writing a Mixin for that class.
2.  **Use Access Wideners** for core library-style accessibility that many classes will use.
3.  **Descriptors**: Use the internal JVM descriptors (e.g., `Ljava/util/UUID;` for `UUID`, `V` for `void`).
