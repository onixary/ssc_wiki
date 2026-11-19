# 资源包 / 数据包结构

请结合[GitHub 仓库示例](https://github.com/onixary/shape-shifter-curse-fabric/tree/master/custom_form_pack_example)阅读本页

一个完整的自定义形态通常由**资源包**与**数据包**两部分组成：

- **资源包**：负责形态的 3D 模型、贴图、染色蒙版、软骨骼/眨眼配置、动作动画以及本地化文本
- **数据包**：负责形态定义（分组、阶段、标记、动画绑定）、能力（powers）、变形触发条件、饰品加成与药水配方

两者需要同时加载，且建议**始终使用自己的独立命名空间**（以下示例均以 `example_namespace` 为例）以防冲突

---

## 资源包结构（1.10.0）

1.10.0 版本起，模型定义与资源不再放在 `assets/orif-defaults/furs/` 下，而是统一放在你自己的命名空间目录中：

```text
example_form_resourcepack/
├── pack.mcmeta
└── assets/
    └── example_namespace/
        ├── geo/                          # GeckoLib 导出的 .geo.json 模型文件
        │   └── form_example.geo.json
        ├── textures/                     # 形态贴图、colormask 蒙版、发光贴图
        │   └── form_example.png
        ├── ssc_form_model/               # 形态模型配置文件（取代旧版 orif-defaults/furs）
        │   └── origins.origin.example_namespace.form_example.json
        ├── player_animation/             # GeckoLib 导出的玩家动作动画文件
        │   └── form_example_idle.json
        ├── lang/                         # 纯文本本地化文件
        │   ├── en_us.json
        │   └── zh_cn.json
        └── rich_lang/                    # 可选：带颜色格式的富文本本地化文件
            ├── en_us.json
            └── zh_cn.json
```

---

## 数据包结构（1.10.0）

```text
example_form_datapack/
├── pack.mcmeta
└── data/
    ├── example_namespace/
    │   ├── ssc_form/                     # 形态核心定义 JSON（支持 /reload 热重载）
    │   │   └── example.json
    │   ├── origins/                      # 形态对应的 Origin 定义
    │   │   └── form_example.json
    │   ├── powers/                       # 自定义能力（Apoli / SSC 扩展 Power）
    │   │   ├── custom_scale.json
    │   │   ├── custom_night_vision.json
    │   │   ├── to_example_form.json
    │   │   └── to_example_form_by_effect.json
    │   ├── origins_power_extra/          # 可选：向已有形态额外挂载 Power（常用于添加变形入口）
    │   │   └── append_transform.json
    │   ├── accessory_power/              # 可选：自定义饰品对形态增删的 Power
    │   │   └── example.json
    │   ├── dynamic_brewing_recipes/      # 可选：自定义形态药水酿造配方
    │   │   └── example_potion_recipe.json
    │   └── tags/                         # 可选：自定义实体/物品标签
    │       └── entity_types/
    │           └── attack_to_example_effect_entity.json
    └── origins/
        └── origin_layers/
            └── origin.json               # 注册自定义 Origin，必须将 example_namespace:form_example 加入其中
```
