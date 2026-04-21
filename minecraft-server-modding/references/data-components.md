# Data Components in Minecraft 26.1+ (Fabric)

Minecraft 26.1+ (1.20.5+) replaced most item NBT data with a structured "Data Components" system.

## Overview

Instead of an unstructured `CompoundTag` under `nbt`, items now have discrete components.

### Key Classes

- `DataComponents`: Contains constants for built-in components.
- `CustomData`: Used for arbitrary mod data (similar to the old `nbt` tag).
- `DataComponentType`: The type of a component.

## Getting Data

To get data from an `ItemStack`:

```java
import net.minecraft.core.component.DataComponents;
import net.minecraft.world.item.component.CustomData;

// Get custom data (NBT-like)
CustomData customData = stack.get(DataComponents.CUSTOM_DATA);
if (customData != null) {
    CompoundTag tag = customData.copyTag();
    if (tag.contains("MyModValue")) {
        int value = tag.getInt("MyModValue");
    }
}

// Get display name
Component name = stack.get(DataComponents.CUSTOM_NAME);
```

## Setting Data

To set data on an `ItemStack`:

```java
import net.minecraft.core.component.DataComponents;
import net.minecraft.world.item.component.CustomData;
import net.minecraft.nbt.CompoundTag;

CompoundTag tag = new CompoundTag();
tag.putInt("MyModValue", 100);

stack.set(DataComponents.CUSTOM_DATA, CustomData.of(tag));

// Set custom name
stack.set(DataComponents.CUSTOM_NAME, Component.literal("Special Item"));
```

## Custom Data Component Types

For more type-safe data, you can register your own `DataComponentType`.

### Registration

```java
public static final DataComponentType<Long> MY_VALUE = Registry.register(
    BuiltInRegistries.DATA_COMPONENT_TYPE,
    new ResourceLocation("mymod", "my_value"),
    DataComponentType.<Long>builder().codec(Codec.LONG).build()
);
```

### Usage

```java
// Set
stack.set(MyMod.MY_VALUE, 12345L);

// Get
Long value = stack.get(MyMod.MY_VALUE);
```

## Best Practices

1. **Prefer Typed Components**: If you have a specific piece of data, register a custom `DataComponentType` instead of stuffing it into `CUSTOM_DATA`.
2. **Immutability**: Data components are often immutable or copied when set.
3. **Data Fixers**: Note that data components have a different serialization format than old NBT.
