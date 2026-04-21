# Custom Configuration Patterns

For mod-specific settings, a standard GSON-based configuration system is recommended. This allows for easy reading and writing of JSON files.

## 1. Defining the Config Model
Create a simple POJO for your configuration.

```java
public class MyModConfig {
    public boolean enableFeature = true;
    public int maxCount = 100;
    public String message = "Hello World!";
}
```

## 2. Config Manager
Implement a simple manager to load and save the config file.

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import net.fabricmc.loader.api.FabricLoader;
import java.nio.file.Path;
import java.nio.file.Files;

public class ConfigManager {
    private static final Gson GSON = new GsonBuilder().setPrettyPrinting().create();
    private static final Path CONFIG_FILE = FabricLoader.getInstance().getConfigDir().resolve("mymod.json");
    private static MyModConfig config = new MyModConfig();

    public static void load() {
        if (!Files.exists(CONFIG_FILE)) {
            save(); // Save defaults
            return;
        }
        try (var reader = Files.newBufferedReader(CONFIG_FILE)) {
            config = GSON.fromJson(reader, MyModConfig.class);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void save() {
        try (var writer = Files.newBufferedWriter(CONFIG_FILE)) {
            GSON.toJson(config, writer);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static MyModConfig getConfig() {
        return config;
    }
}
```

## Best Practices
1.  **Defaults**: Always provide default values in the config model class.
2.  **Comments**: Use `transient` fields for runtime-only data that shouldn't be saved to the JSON.
3.  **Validation**: Sanitize values (like min/max limits) after loading the config.
