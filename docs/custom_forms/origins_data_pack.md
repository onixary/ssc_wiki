# 添加自定义形态能力

mod使用[Origins](https://modrinth.com/mod/origins)的逻辑来定义不同形态的不同能力。换言之，每个形态都对应着Origins中的一个单独的origin

要为自定义形态定义能力，请参考[Origins文档](https://origins.readthedocs.io/en/latest/)

此外，模组额外添加了一些与特有机制有关的power与action可供使用

---
## 模组特有的power与action：

### 本能系统相关

这些power与action用于与模组的本能系统交互，用于在特定条件下增加或减少本能值

#### add_sustained_instinct
      
用于持续增加或减少instinct的power

以下示例用于实现当玩家处于繁茂洞穴群系中时，以0.003每tick的速度持续增加本能值

```json
    {
      "type": "shape-shifter-curse:add_sustained_instinct",
      "instinct_effect_id": "FORM_AXOLOTL_BIOME",
      "value": 0.003,
      "duration": 1,
      "condition": {
        "type": "origins:biome",
        "biome": "minecraft:lush_caves"
      }
    }
```

#### add_instinct
      
一次性增加或减少instinct的action

以下示例用于实现当玩家每次食用生鱼时，在20tick内增加0.1的本能值

```json
    {
      "type": "origins:action_on_item_use",
      "entity_action": {
        "type": "shape-shifter-curse:add_instinct",
        "instinct_effect_id": "FORM_AXOLOTL_EAT_FISH",
        "value": 0.1,
        "duration": 20
      },
      "item_condition": {
        "type": "origins:ingredient",
        "ingredient": {
          "tag": "origins:fish"
        }
      }
    }
```

一般而言，对于“阶段变化形态”的0和1阶段，加入[金苹果](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/src/main/resources/data/shape-shifter-curse/powers/form_instinct_use_golden_apple.json)与[催化剂](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/src/main/resources/data/shape-shifter-curse/powers/form_instinct_use_catalyst.json)的相关power是必要的。当然，你也可以随意定义自己的instinct物品

---

### 角色缩放相关：
   
你可以通过`scale`power调整角色的尺寸缩放，这一缩放不会影响到形态的移动速度与跳跃高度等属性

`eye_scale`字段用于定义视角高度的缩放
   
每个形态都**必须**包含一个`scale`power，否则可能会在变化形态时出现尺寸错乱的情况

```json
{
  "type": "shape-shifter-curse:scale",
  "scale": 0.5,
  "eye_scale": 0.6
}
```

---

### 其他特有的power与action：
   
模组中各形态所用到的自定义power、action与condition均在源码的[additional_power](https://github.com/onixary/shape-shifter-curse-fabric/tree/master/src/main/java/net/onixary/shapeShifterCurseFabric/additional_power)目录下，此处不再赘述

请随意参考与复用它们


