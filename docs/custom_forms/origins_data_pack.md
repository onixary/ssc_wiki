# 添加自定义形态能力

模组使用 Apoli / Origins 的体系来定义形态能力——每个形态都对应一个独立的 Origin（放在 `data/<命名空间>/origins/` 下，并在 `data/origins/origin_layers/origin.json` 中注册）

基础能力语法请参考 [Origins 文档](https://origins.readthedocs.io/en/latest/)

!!! note

    **1.10.0 命名空间迁移提示：**
    - Power / Action / Condition 类型建议统一使用 `apoli:` 前缀（旧的 `origins:` 类型目前仍可兼容，但后续版本计划逐步移除）
    - 食性、武器与方块等标签（如 `fish`、`meat`、`raw_meat`、`ignore_diet`）已迁移至 **`shape-shifter-curse:`** 命名空间，建议在自定义能力中使用 `shape-shifter-curse:fish` 等新标签

---

## 一、为自定义形态添加变形入口

为了让玩家在游戏内变身成你的自定义形态，可以通过以下三种方式实现（可组合使用）：

### 1. 挂载变形触发 Power（`origins_power_extra`）

在 `data/<命名空间>/origins_power_extra/` 下新建 JSON，可将自定义 Power 挂载到模组已有形态（如开书后的默认形态 `shape-shifter-curse:form_original_shifter`）上：

```json
{
  "TargetLayerID": "origins:origin",
  "TargetOriginsID": "shape-shifter-curse:form_original_shifter",
  "ExtraPowers": [
    "example_namespace:to_example_form",
    "example_namespace:to_example_form_by_effect"
  ]
}
```

随后在 `data/<命名空间>/powers/` 中编写触发逻辑。例如使用 `shape-shifter-curse:transform_to_form` 直接触发变形：

```json
{
  "type": "apoli:action_on_item_use",
  "entity_action": {
    "type": "shape-shifter-curse:transform_to_form",
    "form_id": "example_namespace:example",
    "instant": true
  },
  "item_condition": {
    "type": "apoli:ingredient",
    "ingredient": {
      "tag": "shape-shifter-curse:fish"
    }
  }
}
```

或者使用 `shape-shifter-curse:give_custom_transform_effect` 给予自定义变形效果（使玩家在下次睡眠时完成变形）：

```json
{
  "type": "apoli:self_action_when_hit",
  "entity_action": {
    "type": "shape-shifter-curse:give_custom_transform_effect",
    "form_id": "example_namespace:example"
  },
  "damage_condition": {
    "type": "apoli:attacker",
    "entity_condition": {
      "type": "apoli:in_tag",
      "tag": "example_namespace:attack_to_example_effect_entity"
    }
  }
}
```

### 2. 添加自定义形态药水（`dynamic_brewing_recipes`）

在 `data/<命名空间>/dynamic_brewing_recipes/` 下新建 JSON，即可为自定义形态注册酿造台配方：

```json
{
  "type": "potion",
  "input": "shape-shifter-curse:moondust_potion",
  "ingredient": "minecraft:bone",
  "output": "shape-shifter-curse:to_custom_statue_potion",
  "target_form": "example_namespace:example"
}
```

### 3. 自定义饰品加成（`accessory_power`）

在 `data/<命名空间>/accessory_power/` 下新建 JSON，可为指定饰品定义在你的自定义形态下新增或移除的 Power（完整示例见仓库的 [accessory_power/example.json](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/custom_form_pack_example/example_form_datapack/data/example_namespace/accessory_power/example.json)）

---

## 二、常用模组特有 Power 与 Action

### 1. 本能系统相关

#### `add_sustained_instinct`（持续增减本能）

当 `instinct_effect_id` 与已有 Power 重复时，后加载的会覆盖前者。以下示例在繁茂洞穴群系中每 tick 持续增加 0.003 本能值：

```json
{
  "type": "shape-shifter-curse:add_sustained_instinct",
  "instinct_effect_id": "FORM_AXOLOTL_BIOME",
  "value": 0.003,
  "duration": 1,
  "condition": {
    "type": "apoli:biome",
    "biome": "minecraft:lush_caves"
  }
}
```

#### `add_instinct`（一次性增减本能）

以下示例在玩家食用生鱼时，在 20 tick（1 秒）内每 tick 增加 0.1 本能值（总计 +2）：

```json
{
  "type": "apoli:action_on_item_use",
  "entity_action": {
    "type": "shape-shifter-curse:add_instinct",
    "instinct_effect_id": "FORM_AXOLOTL_EAT_FISH",
    "value": 0.1,
    "duration": 20
  },
  "item_condition": {
    "type": "apoli:ingredient",
    "ingredient": {
      "tag": "shape-shifter-curse:fish"
    }
  }
}
```

对于阶段变化形态的`初始阶段`与`第一阶段`，通常建议复用模组内置的 [金苹果](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/src/main/resources/data/shape-shifter-curse/powers/form_instinct_use_golden_apple.json) 与 [催化剂](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/src/main/resources/data/shape-shifter-curse/powers/form_instinct_use_catalyst.json) 本能 Power

### 2. 角色缩放（`scale`）

数据包动态形态需要通过 `shape-shifter-curse:scale` Power 控制实际碰撞箱大小（不会影响移速与跳跃高度），并建议与 `ssc_form/*.json` 中的 `eyeScale` 保持一致：

```json
{
  "type": "shape-shifter-curse:scale",
  "scale": 0.55,
  "eye_scale": 0.6
}
```

### 3. 更多自定义 Power 与 Action

模组新增的各类能力、动作与条件（包括主动技能按键绑定 `key.shape-shifter-curse.active_skill_1`~`_6`）均可在源码的 [additional_power](https://github.com/onixary/shape-shifter-curse-fabric/tree/master/src/main/java/net/onixary/shapeShifterCurseFabric/additional_power) 目录中查阅与复用
