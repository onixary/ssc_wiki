# 概览

请首先参考[GitHub 仓库示例](https://github.com/onixary/shape-shifter-curse-fabric/tree/master/custom_form_pack_example)中的完整数据包与资源包模板

从 1.10.0 版本开始，模组重写了底层的形态与模型动画系统。现在，你可以通过**数据包** + **资源包**的方式，更加灵活地实现自定义形态、子形态、模型与动作动画、专属能力、动态药水以及饰品效果

!!! note

    **从 1.9.2 及更早版本升级的作者请注意：**
    1.10.0 版本对形态定义 JSON（`ssc_form`）、模型配置路径（迁移至 `ssc_form_model`）、软骨骼动画配置（迁移至 `animation_system`）以及部分标签命名空间进行了破坏性升级。旧版自定义形态包需要对照本章节进行迁移

---

### 你可以通过资源包 / 数据包做到的事情

- **定义完整的新形态或子形态**：通过 `ssc_form` 配置形态分组、阶段（tier）、权重、体型（人形/四足）、行为标记（flag）、回退形态（fallback），也可通过 `MasterForm` 将其注册为已有形态的子形态，或通过 `PerkTree` 挂载技能树
- **添加形态触发途径**：支持通过 `origins_power_extra` 挂载变形触发器、使用 `give_custom_transform_effect` 给予自定义变形效果，以及通过 `dynamic_brewing_recipes` 注册自定义形态药水配方
- **自定义模型与动作动画**：通过 `ssc_form_model` 与 GeckoLib 模型配置自定义形态外观、换色蒙版、自发光贴图、眨眼骨骼与软骨骼链，并为不同状态绑定 `playerAnimator` 动画
- **灵活定制能力与饰品效果**：使用 Apoli / Origins 语法编写能力，支持在形态 JSON 中直接通过 `ExtraPower` / `RemovedPower` 追加或移除能力，也可通过 `accessory_power` 自定义饰品在不同形态下的加成效果
- **自定义图鉴文本**：通过 `lang` 与 `rich_lang` 自定义形态在幻形者之书中的名称、描述、优劣势与本能说明

### 当前的一些限制

- 尚无法仅通过数据包添加新的独立咒文生物实体（但可以通过 power 监听受击/交互事件触发自定义变形效果）
- 数据包动态形态的碰撞箱缩放仍需通过 `shape-shifter-curse:scale` power 进行控制（配合形态 JSON 中的 `eyeScale` 调整渲染眼高）
