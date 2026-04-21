# NBT Serialization

NBT (Named Binary Tag) is the standard format for persistent data in Minecraft. In 26.1+, you'll primarily interact with `CompoundTag` to store mod-specific data on entities, tiles, or within custom data components.

## 1. Interacting with `CompoundTag`

### Basic Read/Write
```java
CompoundTag tag = new CompoundTag();
tag.putString("ModId:Key", "Value");
tag.putInt("Level", 5);

String value = tag.getString("ModId:Key");
int level = tag.getInt("Level");
```

### Checking for Keys
```java
if (tag.contains("ModId:Key", Tag.TAG_STRING)) {
    // Safely get string
}
```

## 2. Converting to and from Byte Arrays

### Writing NBT to Bytes
```java
import net.minecraft.nbt.NbtIo;

ByteArrayOutputStream bos = new ByteArrayOutputStream();
NbtIo.writeCompressed(tag, bos); // GZIP compressed by default
byte[] bytes = bos.toByteArray();
```

### Reading NBT from Bytes
```java
import net.minecraft.nbt.NbtIo;

ByteArrayInputStream bis = new ByteArrayInputStream(bytes);
CompoundTag tag = NbtIo.readCompressed(bis, NbtAccounter.unlimitedHeap());
```

## 3. Persistent State in Mod Logic

### Attaching Data to Entities/Blocks
Most persistent game objects provide `load` and `save` methods that you can override or Mixin into to handle custom `CompoundTag` data.

```java
@Override
public void load(CompoundTag tag) {
    super.load(tag);
    if (tag.contains("MyData")) {
        this.myData = tag.getInt("MyData");
    }
}

@Override
protected void saveAdditional(CompoundTag tag) {
    super.saveAdditional(tag);
    tag.putInt("MyData", this.myData);
}
```

## Best Practices
1.  **Prefix Your Keys**: Always prefix your NBT keys with your mod's ID (e.g., `mymod:power_level`) to avoid collisions with other mods or future vanilla updates.
2.  **Safety**: Use `NbtAccounter.unlimitedHeap()` only when you trust the source of the NBT data. For untrusted data, use a limit to prevent memory exhaustion.
3.  **Data Fixers**: If you change your data structure between mod versions, consider using a `DataFixer` pattern to migrate old NBT data.
