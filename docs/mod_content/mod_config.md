# Mod可配置项

强烈推荐您安装[Mod Menu](https://www.curseforge.com/minecraft/mc-mods/modmenu)来进行模组的配置

### 玩家自定义配置项

配置文件路径：`config\shape-shifter-curse-client-custom.toml`

- `keep_original_skin = true`
        
        是否保留你的原本皮肤

- `enable_form_color = true`

        是否启用形态颜色自定义功能

- `primaryColor = 13822450`

        形态的主要颜色
        这一颜色的数值为将16进制颜色转为10进制数之后的值
        推荐安装ModMenu来在游戏中直观地直接进行Hex颜色配置

- `accentColor1Color = 8967397`

        形态的强调颜色1

- `accentColor2Color = 7829367`

        形态的强调颜色2

- `eyeColor = 1118481`

        形态的眼睛颜色。只有在眼睛颜色被覆盖的形态下才会生效

- `primaryGreyReverse = false`

        是否反转主要颜色的灰度值
        形态的自定义颜色会混合原本贴图的灰度纹路来实现更加自然的效果

- `accent1GreyReverse = false`

        是否反转强调颜色1的灰度值

- `accent2GreyReverse = false`

        是否反转强调颜色2的灰度值

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