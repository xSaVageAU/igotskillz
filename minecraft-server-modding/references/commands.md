# Command Registration in Minecraft 26.1+ (Fabric)

In Minecraft 26.1+, command registration is handled via the Fabric Command API v2.

## Registration Pattern

Commands should be registered using the `CommandRegistrationCallback.EVENT`.

```java
import net.fabricmc.fabric.api.command.v2.CommandRegistrationCallback;
import net.minecraft.commands.Commands;
import com.mojang.brigadier.CommandDispatcher;
import net.minecraft.commands.CommandSourceStack;

public class MyMod implements ModInitializer {
    @Override
    public void onInitialize() {
        CommandRegistrationCallback.EVENT.register((dispatcher, registryAccess, environment) -> {
            MyCommand.register(dispatcher);
        });
    }
}
```

## Command Class Structure

It is recommended to use a dedicated class for each command or a group of related commands.

```java
public final class MyCommand {
    public static void register(CommandDispatcher<CommandSourceStack> dispatcher) {
        dispatcher.register(
            Commands.literal("mycommand")
                .requires(source -> source.permissions().hasPermission(Permissions.COMMANDS_ADMIN))
                .then(Commands.argument("message", StringArgumentType.greedyString())
                    .executes(MyCommand::execute))
        );
    }

    private static int execute(CommandContext<CommandSourceStack> context) {
        // Implementation
        return 1; // Success
    }
}
```

## Argument Types

Commonly used argument types:
- `StringArgumentType.string()`: A single word.
- `StringArgumentType.greedyString()`: The rest of the command line.
- `EntityArgument.player()`: A single player.
- `EntityArgument.players()`: Multiple players.
- `LongArgumentType.longArg(min, max)`: A long integer.
- `DoubleArgumentType.doubleArg(min, max)`: A double.

## Suggestions

Providing suggestions for arguments:

```java
.then(Commands.argument("target", StringArgumentType.string())
    .suggests((context, builder) -> SharedSuggestionProvider.suggest(List.of("option1", "option2"), builder))
    .executes(...))
```
