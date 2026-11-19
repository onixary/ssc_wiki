# 添加形态自定义模型与动画

自定义形态的模型与动作动画均使用 [Blockbench](https://www.blockbench.net/) 制作

自 1.10.0 版本起，模组的运行时渲染库已全面迁移至 **GeckoLib 4.x**，因此无论导出模型还是动作动画，现在均统一使用 Blockbench 的 **`GeckoLib Animation Utils`** 插件

---

## 特别注意：模型工程与动画工程的差异

形态模型沿用了 Origin Furs 的骨骼体系，而动作动画基于 [playerAnimator](https://modrinth.com/mod/playeranimator) 的实现。两者的工程格式存在以下差异，**不能混用**：

1. **朝向相反**：形态模型面向 **+Z** 方向，而动作动画模型面向 **-Z** 方向
2. **腿部命名颠倒**：形态模型中左右腿骨骼的命名是颠倒的（Origin Furs 的历史遗留），请保持原样切勿自行对调

建议直接以源码中的 [形态模型模板](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/3d_models/player_form/axolotl/form_axolotl_2.bbmodel) 与 [四足通用动画模板](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/3d_models/player_form/0_common/feral_form/animation/form_feral_common_anim.bbmodel) 为基础进行修改

---

## 一、配置自定义模型（`ssc_form_model`）

1. 使用 Blockbench 的 GeckoLib 插件导出 `.geo.json` 模型，放入 `assets/<命名空间>/geo/`
2. 将贴图放入 `assets/<命名空间>/textures/`
3. 在 `assets/<命名空间>/ssc_form_model/` 下新建模型配置 JSON（取代旧版的 `assets/orif-defaults/furs/`），建议命名为 `origins.origin.<命名空间>.<origin名>.json`

### 模型配置示例（1.10.0 格式）

```json
{
  "load_priority": 0,
  "name": "example_namespace.form_example.model_name",
  "layer": "origins:origin",
  "form": "example_namespace:form_example",
  "wide_only": true,

  "model": "example_namespace:geo/form_example.geo.json",
  "texture": "example_namespace:textures/form_example.png",
  "hidden": [
    "leftLeg", "rightLeg", "rightPants", "leftPants",
    "leftSleeve", "rightSleeve", "leftArm", "rightArm",
    "body", "jacket", "head", "hat"
  ],

  "animation_system": "shape-shifter-curse:default",
  "animation_system_config": {
    "tail": {
      "chain": {
        "tail": [0, 1, 2, 3]
      }
    },
    "head_tail": {
      "chain": {
        "ear_l": [0],
        "ear_r": [0]
      }
    },
    "wing_l": {
      "chain": {
        "wing_l": [0, 1]
      }
    },
    "wing_r": {
      "chain": {
        "wing_r": [0, 1]
      }
    },
    "eye_blink": {
      "eye": "eyeRoot",
      "open_scale": 1.0,
      "close_scale": 0.01
    }
  }
}
```

### 关键字段说明

- **`layer` / `form`**：对应数据包中的 Origin 层（通常为 `origins:origin`）与 Origin ID（如 `example_namespace:form_example`）
- **`hidden`**：隐藏原版玩家模型的对应身体部件
- **`animation_system` 与 `animation_system_config`**（1.10.0 新版软骨骼与眨眼配置）：
  - `tail`：父级为躯干的尾巴/飘带软骨骼链
  - `head_tail`：父级为头部的耳朵/毛发等附属软骨骼链
  - `wing_l` / `wing_r`：左右翅膀骨骼链
  - `eye_blink`：眨眼动画配置，在模型 `bipedHead` 下放置名为 `eyeRoot` 的眼部骨骼即可启用自动眨眼
  - 请确保同一骨骼链中所有骨骼的前缀命名一致（例如 `tail_0`、`tail_1`、`tail_2`）

!!! note

    **旧版迁移注意**：1.9.2 及以前使用的顶层 `tail_chain` / `wing_chain_l` / `builtin_controller_data` 在 1.10.0 中**已不再读取**（会静默失效，导致尾巴与翅膀不动）。请务必改用上方的 `animation_system` + `animation_system_config` 格式

---

## 二、添加自定义动作动画

1. 在 Blockbench 中安装 **GeckoLib** 插件，参考[已有动画项目](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/3d_models/player_form/0_common/feral_form/animation/form_feral_common_anim.bbmodel)在 `Animate` 标签页中制作帧动画并导出
2. 将导出的动画 JSON 放入 `assets/<命名空间>/player_animation/`
3. 在数据包的形态定义 JSON（`ssc_form/*.json`）的 `anim`、`animDefault` 或 `powerAnim` 中绑定对应的动画 ID

建议优先复用模组内置的通用动画（如 `shape-shifter-curse:form_feral_common_*`），仅对需要突出形态特色的动作自行制作
