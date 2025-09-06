# Mod可配置项

### 客户端配置项

配置文件路径：`config\shape-shifter-curse-client.toml`

- `enableFormModelOnVanillaFirstPersonRender = true`
      
      用于确定是否在原版第一人称渲染不同形态的手臂模型
      如果在第一人称时体验到游戏卡顿的情况，建议设为false

- `newStartBookForBiggerScreen = false`
      
      是否要启用扩大尺寸的新版幻形者之书页面
      对于使用高分辨率屏幕游玩，页面UI过小的情况下建议设为true

### 服务端通用配置项

配置文件路径：`config\shape-shifter-curse-common.toml`

- `transformativeBatSpawnChance = 0.5`
      
      设置咒文蝙蝠的刷新概率

- `transformativeAxolotlSpawnChance = 1.0`
      
      设置咒文美西螈的刷新概率

- `transformativeOcelotSpawnChance = 0.67`
      
      设置咒文豹猫的刷新概率

- `enableNewStartBook = true`
      
      是否要启用新版的幻形者之书页面
      默认启用，设为false来使用旧版本的幻形者之书页面
      注意：若要使用旧版本页面，你需要安装owo-lib mod，否则不会生效