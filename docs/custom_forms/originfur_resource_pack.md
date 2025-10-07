# 添加形态自定义模型与动画

## 特别注意：模型Blockbench项目与动画Blockbench项目的格式差异

自定义动画与模型均需要使用[Blockbench](https://www.blockbench.net/)来进行制作

形态的自定义模型基于[Origin Furs](https://modrinth.com/mod/origin-furs)的实现，而形态动画则基于[playerAnimator](https://modrinth.com/mod/playeranimator)的实现

两者模型的**面向轴向**与**左右命名**都有所差异，不能混用：

1. 形态模型的面对方向为+Z方向，而形态动画模型则面向-Z方向

2. 形态模型中，腿部骨骼的命名是**左右颠倒**的。这可能是Origin Furs本身的遗留问题，由于修复它会导致效果出错，请维持当前命名

建议您通过修改已有的[形态模型项目](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/3d_models/player_form/axolotl/form_axolotl_2.bbmodel)与[形态动画项目](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/3d_models/player_form/0_common/feral_form/animation/form_feral_common_anim.bbmodel)来实现自己的形态模型与动画

## 添加自定义模型

mod基于[Origin Furs](https://modrinth.com/mod/origin-furs)的逻辑来为不同形态实现自定义模型。形态模型应能够很容易地从其他Blockbench角色模型迁移过来

要为特定形态添加自定义模型，请参考[Origin Furs文档](https://originalfur.readthedocs.io/en/latest/)以及源码[player_form目录](https://github.com/onixary/shape-shifter-curse-fabric/tree/master/3d_models/player_form)的Blockbench项目文件

要导出形态模型，你需要在Blockbench中安装`AzureLib Animator`插件，并在导出时选择`Export Azurelib.geo Model`

### 注册模型贴图与软骨骼

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

## 添加自定义动画

mod基于[playerAnimator](https://modrinth.com/mod/playeranimator)的方法来为不同形态实现自定义动画

要添加自定义动画，您同样需要在Blockbench中安装`AzureLib Animator`插件，并参考[已有动画项目](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/3d_models/player_form/0_common/feral_form/animation/form_feral_common_anim.bbmodel)，在右上角多出的`Animate`标签页中制作帧动画

您的动画json文件应该放置在`assets/example_namespace/player_animation`下

在制作与导出完成后，您还需要在[JSON配置文件](https://ssc-wiki.readthedocs.io/zh-cn/latest/custom_forms/form_definition_json/)中进行注册

建议您首先复用模组中已经实现的动画，在没有合适动画时再自行制作