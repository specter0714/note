# JSONUtil全局配置的原理

下面是 JSONUtil 类的简要代码，他有一个静态变量**全局配置**，每次调用他的其他静态方法的时候都会调用这个全局配置，所以，我们只需要修改这个静态变量，就可以达到全局生效的效果，因为静态变量是属于类的，有唯一性

```java
public class JSONUtil {
    // 内部维护一个全局配置实例
    private static JSONConfig globalConfig = new JSONConfig();
    
    // 提供设置全局配置的方法
    public static void setConfig(JSONConfig config) {
        globalConfig = config; // 替换全局配置
    }
    
    // 静态方法内部使用全局配置
    public static String toJsonStr(Object obj) {
        return toJsonStr(obj, globalConfig); // 使用全局配置
    }
    
    // 实际的序列化逻辑
    public static String toJsonStr(Object obj, JSONConfig config) {
        // 根据传入的配置进行序列化
        // config.getSerializeNulls() 决定是否序列化null值
    }
}
```

下面就是对 JSONUtil 的全局配置，不需要使用 @Bean 把他存到 IOC 容器中

```java
import cn.hutool.json.JSONConfig;
import cn.hutool.json.JSONUtil;
import org.springframework.context.annotation.Configuration;
import javax.annotation.PostConstruct;

@Configuration
public class HutoolJsonConfig {
    @PostConstruct
    public void configureHutoolJson() {
        // 当 Spring 容器初始化这个配置类后，自动执行此方法
        // 设置 Hutool JSON 全局配置
        JSONConfig jsonConfig = new JSONConfig();
        jsonConfig.setSerializeNulls(true); // 序列化 null 值
        JSONUtil.setConfig(jsonConfig);
    }
}
```

