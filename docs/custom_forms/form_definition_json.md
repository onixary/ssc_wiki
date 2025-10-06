# 配置形态定义JSON

形态定义JSON文件用于配置自定义形态的动画、形态种类、体型等

其需要被存放在`/data/example_namespace/ssc_form`目录下，其名称与您的自定义形态ID相同

### 形态定义JSON格式

```json
{
  "__comment__": "具体看PlayerFormBase | See PlayerFormBase for details",
  "groupID": "example_namespace:example_group",
  "__groupID_comment__": "组ID 用于注册组 与数据包结构相对应 | Group ID used for registering groups, corresponding to the datapack structure",
  "groupIndex": 5,
  "__groupIndex_comment__": "组内索引 用于变形逻辑 | Index within group used for transformation logic",
  "phase": "PHASE_SP",
  "__Phase_comment__": "变形阶段一般和groupIndex一样 Enum:[PHASE_CLEAR, PHASE_0, PHASE_1, PHASE_2, PHASE_3, PHASE_SP] | Transformation phase usually matches groupIndex. Enum:[PHASE_CLEAR, PHASE_0, PHASE_1, PHASE_2, PHASE_3, PHASE_SP]",
  "bodyType": "NORMAL",
  "__BodyType_comment__": "身体类型 Enum:[NORMAL, FERAL] | Body type. Enum:[NORMAL, FERAL]",
  "isCustomForm": true,
  "originID": "example_namespace:form_example",
  "__originID_comment__": "OriginID 与内置的Origin mod联动 与origin_layer的名称相对应 **必须在 data/[NameSpace]/origins 里注册** | OriginID works with the built-in Origin mod, corresponding to the origin layer name. **Must be registered in data/[NameSpace]/origins**",
  "originLayerID": "origins:origin",
  "__originLayerID_comment__": "OriginLayerID 与内置的OriginFur mod联动 **极度不推荐修改** | OriginLayerID works with OriginFur **Highly not recommended to modify**",
  "hasSlowFall": false,
  "overrideHandAnim": false,
  "canSneakRush": false,
  "canRushJump": false,
  "anim": [
    {
      "state": "ANIM_IDLE",
      "animID": "example_namespace:form_example_idle"
    },
    {
      "state": "ANIM_SNEAK_IDLE",
      "animID": "shape-shifter-curse:form_feral_common_sneak_idle"
    }
  ],
  "__anim_comment__": "动画列表 State 在 PlayerAnimState 中定义 | Animation list. State is defined in PlayerAnimState",
  "animDefault": {
    "animID": "shape-shifter-curse:form_feral_common_idle",
    "speed": 1.0,
    "fade": 2
  },
  "__animDefault_comment__": "默认动画 如果没有可以不用添加此键 | Default animation. If not present, this key can be omitted"
}
```

示例中的comment字段应已经解释了对应字段的含义，以下是对于一些字段的补充说明：

### groupID

用于标识当前形态处于哪个组内

只有处于同一个`groupID`下的，并且`groupIndex`与`phase`注册正确的形态才能够按照阶段变化形态的方式正常触发变形逻辑

例如`bat_0`,`bat_1`,`bat_2`,`bat_3`都应该处于`example_namespace:bat`组中

### groupIndex 与 phase

分别标识组内索引与变形阶段，用于阶段变化形态的变形逻辑

两者是一一对应的，其含义与对应关系如下，请参考下表来根据您的想要的自定义形态的阶段来注册对应的groupIndex与phase：

| groupIndex: |        -2         |        -1        |      0      |      1      |      2      |         3         |     5      |
|:-----------:|:-----------------:|:----------------:|:-----------:|:-----------:|:-----------:|:-----------------:|:----------:|
|   phase:    |    PHASE_CLEAR    |   PHASE_CLEAR    |   PHASE_0   |   PHASE_1   |   PHASE_2   |      PHASE_3      |  PHASE_SP  |
|    形态阶段：    | 未开启模组内容时的形态，自定义无用 | 变形前的原始形态占位，自定义无用 | 阶段变化形态的第一阶段 | 阶段变化形态的第二阶段 | 阶段变化形态的第三阶段 | 阶段变化形态的第四阶段，即永久阶段 | 可随时恢复的特殊形态 |

例如：`bat_0`的`groupIndex`应设置为`0`，`phase`应设置为`PHASE_0`

groupIndex 4目前没有被使用，它是留给之后的特殊形态的

### bodyType

人形为`NORMAL`，四足形态为`FERAL`

这一字段只影响视角高度以及物品的渲染方式。在`FERAL`bodyType下，你需要搭配一套完整的动画才能够实现预期效果。建议复用项目中的已有动画来节省精力

### isCustomForm

设置为`true`。这一字段影响修改形态的命令，为`true`时，其会出现在`set_custom_form`中，而不是`set_form`中

### originID

这一字段需要与origin_layer中的名称相匹配

一个形态需要对应一个origin，用于实现形态能力以及与形态模型相链接

### anim

用于注册不同状态下的动画，其中的`state`字段对应PlayerAnimState中的状态，`animID`字段对应你的命名空间名与导出的动画JSON文件的名称

当前实现的state枚举可在[PlayerAnimState](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/src/main/java/net/onixary/shapeShifterCurseFabric/player_animation/PlayerAnimState.java)中找到。其值与含义列举如下：

| state |         含义         |
|:-----:|:------------------:|
| ANIM_IDLE |        站立静止        |
| ANIM_SNEAK_IDLE |        潜行静止        |
| ANIM_WALK |         移动         |
| ANIM_RUN |         疾跑         |
| ANIM_JUMP |         跳跃         |
| ANIM_FALL |         坠落         |
| ANIM_CLIMB_IDLE |        攀爬静止        |
| ANIM_CLIMB |        攀爬移动        |
| ANIM_SWIM |        游泳状态        |
| ANIM_SWIM_IDLE |        水中静止        |
| ANIM_BOAT_IDLE |        乘船静止        |
| ANIM_RIDE_IDLE |        骑乘静止        |
| ANIM_ELYTRA_FLY |        鞘翅飞行        |
| ANIM_CREATIVE_FLY |       创造模式飞行       |
| ANIM_CRAWL |        爬行移动        |
| ANIM_CRAWL_IDLE |        爬行静止        |
| ANIM_SLOW_FALL |         缓降         |
| ANIM_ATTACK_ONCE |    攻击一次（点击左键一下）    |
| ANIM_TOOL_SWING |   挥动工具（长按左键挖掘等）    |
| ANIM_SNEAK_RUSH |   潜行冲刺（原版用于豹猫形态）   |
| ANIM_RUSH_JUMP | 冲刺跳跃（原版用于豹猫与美西螈形态） |
| ANIM_SLEEP |         睡眠         |
| ANIM_SNEAK_JUMP |      按住潜行时跳跃       |
| ANIM_SNEAK_FALL |      按住潜行时坠落       |
| ANIM_SNEAK_ATTACK_ONCE |    按住潜行时攻击一次（点击左键一下）|
| ANIM_SNEAK_TOOL_SWING | 按住潜行时挥动工具（长按左键挖掘等） |

### animDefault

默认动画。当没有其他动画时，会播放此动画。通常注册为`ANIM_IDLE`状态的动画

对于`FERAL`bodyType而言，如果没有注册此字段，则会在静止时出现直立姿态
