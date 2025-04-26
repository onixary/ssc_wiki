# 基于Origin Furs数据驱动方式的自定义模型

mod基于[Origin Furs](https://modrinth.com/mod/origin-furs)的逻辑来为不同形态实现自定义模型。形态模型应能够很容易地从其他Blockbench角色模型迁移过来

要为特定形态添加自定义模型，请参考[Origin Furs文档](https://originalfur.readthedocs.io/en/latest/)以及源码中`3d_models`目录下的Blockbench项目文件

!!! note

    要导出形态模型，你需要安装`AzureLib Animator`插件，并在导出时选择`Export Azurelib.geo Model`
    请无视模型中左右颠倒的命名。这可能是Origin Furs本身的遗留问题，修复它会导致效果出错，所以..


mod额外实现了用于尾部/飘带等的软骨骼以及动态翅膀。其也需要在json文件中进行注册，示例如下：

```json
{
  "model": "orif-defaults:geo/form_allay_sp.geo.json",
  "texture": "orif-defaults:textures/form_allay_sp/form_allay_sp.png",
  "overlay": "orif-defaults:textures/form_allay_sp/form_allay_sp_overlay.png",
  "hidden": [
    "leftLeg",
    "rightLeg",
    "rightPants",
    "leftPants",
    "body",
    "jacket"
  ],
  "tail_chain": {
    "tail_l": [0, 1, 2],
    "tail_r": [0, 1, 2]
  },
  "wing_chain_l": {
    "wing_l": [0, 1]
  },
  "wing_chain_r": {
    "wing_r": [0, 1]
  },
  "tail_chain_head": {
    "head_tail_l": [0, 1],
    "head_tail_r": [0, 1]
  }
}
```

`tail_chain`：父级为躯干的，用于尾部/飘带的软骨骼链

`tail_chain_head`：父级为头部的，用于头部附属结构的软骨骼链

`wing_chain_l`与`wing_chain_r`：用于翅膀的骨骼链，区分左右

请确保Blockbench中某一骨骼链中所有骨骼的前缀命名保持一致
