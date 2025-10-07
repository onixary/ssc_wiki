# 添加自定义形态能力

mod使用[Origins](https://modrinth.com/mod/origins)的逻辑来定义不同形态的不同能力。换言之，每个形态都对应着Origins中的一个单独的origin

要为自定义形态定义能力，请参考[Origins文档](https://origins.readthedocs.io/en/latest/)

此外，模组额外添加了一些与特有机制有关的power与action可供使用

---

## 挂载在现有形态下的变形触发power

### 变形power的挂载

要通过游戏内条件触发变形，你需要首先在`custom_form_pack_example/example_form_datapack/data/example_namespace/origins_power_extra`目录下挂载一个自己的变形power

```
{
  "TargetOriginsID": "shape-shifter-curse:form_original_shifter",
  "ExtraPowers": [
    "example_namespace:to_example_form"
  ]
}
```

在上例中，`example_namespace:to_example_form`这一power会挂载到`shape-shifter-curse:form_original_shifter`这一形态下

如此一来，你就可以在处于`form_original_shifter`时，触发`to_example_form`这一power中实现的变形效果

若你实现了自己的基础形态，而不是复用模组中的基础形态的话，你可以跳过挂载步骤，直接在自己的基础形态中实现变形power

### 变形power的实现

在JSON中挂载之后，你需要实现自己的变形power

与其他power一致，变形power需要被放置在`custom_form_pack_example/example_form_datapack/data/example_namespace/powers`目录下

你可以在[这里](https://github.com/onixary/shape-shifter-curse-fabric/tree/master/src/main/resources/data/shape-shifter-curse/origins)找到当前的所有形态id

使用`shape-shifter-curse:transform_to_form`action来触发变形逻辑：

```
{
  "type": "origins:action_on_item_use",
  "entity_action": {
    "type": "shape-shifter-curse:transform_to_form",
    "form_id": "example_namespace:example",
    "instant": true
  },
  "item_condition": {
    "type": "origins:ingredient",
    "ingredient": {
      "tag": "origins:fish"
    }
  }
}
```

以上示例中，当玩家食用任何鱼类时，会触发`shape-shifter-curse:transform_to_form`，将玩家变形为`example_namespace:example`形态

由于你已经通过注册将power挂载在了`form_original_shifter`形态下，只有在该形态中，这一power才会生效

---

## 模组特有的power与action：

### 本能系统相关

这些power与action用于与模组的本能系统交互，用于在特定条件下增加或减少本能值

#### add_sustained_instinct
      
用于持续增加或减少instinct的power

当power的`instinct_effect_id`字段与现有power重复时，则后加载的power会覆盖掉已有的power

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

以下示例用于实现当玩家每次食用生鱼时，在20tick的时间内，每tick增加0.1的本能值。即在1秒内增加2本能值

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


