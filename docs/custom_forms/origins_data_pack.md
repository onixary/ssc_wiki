mod使用[Origins](https://modrinth.com/mod/origins)的逻辑来定义不同形态的不同能力。换言之，每个形态都对应着Origins中的一个单独的origin

要为自定义形态定义能力，请参考[Origins文档](https://origins.readthedocs.io/en/latest/)

此外，mod额外添加了一些与特有机制有关的power与action可供使用

---
## mod中额外添加的power与action：

### instinct值相关

这些power与action用于与instinct系统交互，用于增加或减少instinct值。具体值通过预定义的enum决定
   
```json
   {
       // 立即效果（值类型）
       FORM_OCELOT_ATTACK_LIVESTOCK(3.0f),
       // 供自定义形态使用的效果
       FORM_INSTANT_INSTINCT_MEDIUM(3.0f),
       FORM_INSTANT_INSTINCT_LARGE(5.0f),
       // 持续效果（速率类型）
       // 催化剂与金苹果的生效时间都是2秒
       FORM_USE_GOLDEN_APPLE(-4.25f / 2),
       FORM_USE_CATALYST(1.25f / 2),
       FORM_BAT_IN_DARK(0.004f),
       FORM_BAT_EAT_FRUIT(0.1f),
       FORM_BAT_NEAR_DRIPSTONE(0.006f),
       FORM_AXOLOTL_IN_WATER(0.004f),
       FORM_AXOLOTL_EAT_FISH(0.1f),
       FORM_AXOLOTL_NEAR_DRIPLEAF(0.008f),
       FORM_OCELOT_EAT_RAW_MEAT(0.1f),
       FORM_OCELOT_ON_LEAF(0.008f),
       // 供自定义形态使用的效果
       FORM_SUSTAINED_INSTINCT_ENVIRONMENT_MEDIUM(0.004f),
       FORM_SUSTAINED_INSTINCT_ENVIRONMENT_LARGE(0.008f),
       FORM_SUSTAINED_INSTINCT_FOOD(0.1f);
   }
```

#### add_sustained_instinct_in_time
      
在一定时间内增加或减少instinct的power，示例如下

```json
   {
      "type": "origins:add_sustained_instinct_in_time",
      "instinct_effect_type": "FORM_AXOLOTL_EAT_FISH",
      "duration": 20,
      "is_on_item_finished": true,
      "condition": {
         "type": "origins:using_item",
         "item_condition": {
            "type": "origins:ingredient",
            "ingredient": {
               "tag": "origins:fish"
            }
         }
      }
   }
```

#### add_instinct
      
瞬时增加或减少instinct的action，只适用于立即效果的enum定义，示例如下

```json
   {
      "type": "origins:self_action_on_hit",
      "entity_action": {
      "type": "origins:add_instinct",
         "instinct_effect_type": "FORM_OCELOT_ATTACK_LIVESTOCK"
      },
      "target_condition": {
         "type": "origins:in_tag",
         "tag": "origins:livestock"
      }
   }
```

一般而言，对于“阶段变化形态”的0和1阶段，定义金苹果与催化剂的`add_sustained_instinct_in_time power`是必要的。当然，你也可以随意定义自己的instinct物品
   
---

### 角色缩放相关：
   
你可以通过`scale`power调整角色的尺寸缩放，这一缩放不会影响到形态的移动速度与跳跃高度等属性
   
每个形态都**必须**包含一个`scale`power，否则可能会在变化形态时出现尺寸错乱的情况

```json
{
   "type": "origins:scale",
   "scale" : 1.0
}
```

---

### 漂浮能力：
   
给与角色长按跳跃键时飞行一定高度与空中悬浮能力的power，`“continuous”`的值必须为`true`

```json
{
   "type": "origins:levitate",
   "ascent_speed": 0.3,
   "max_ascend_duration" : 30,
   "key": {
      "key": "key.jump",
      "continuous": true
   }
}
```
