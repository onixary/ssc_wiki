# Mod可用指令列举

### 管理员指令

- 触发形态变化：`/shape_shifter_curse transform_to_form <USER> <FORM>`

- 触发数据包自定义形态变化：`/shape_shifter_curse transform_to_dynamic_form <USER> <FORM>`

- 触发子形态变化：`/shape_shifter_curse transform_to_sub_form <USER> <FORM>`

- 直接设定形态：`/shape_shifter_curse set_form <USER> <FORM>`

- 直接设定数据包自定义形态：`/shape_shifter_curse set_dynamic_form <USER> <FORM>`

- 直接设定子形态：`/shape_shifter_curse set_sub_form <USER> <FORM>`

- 管理当前形态技能树：`/shape_shifter_curse perk <list | toggle_free <BOOLEAN> | unlock_all | reset_all>`

- 直接触发诅咒之月事件：`/shape_shifter_curse jump_to_next_cursed_moon`

- 调整世界时间：`/shape_shifter_curse world_time <set | add> <TIME>`

### 所有用户指令

- 是否保留自己的角色皮肤：`/shape_shifter_curse keep_original_skin <BOOLEAN>`

- 打开或管理形态配色预设：`/shape_shifter_curse form_color <menu | list | save | load | delete>`

- 通过指令设置形态颜色：`/shape_shifter_curse set_form_color ...`

### 游戏规则

- 控制是否以服务端配置的初始形态开局（默认 `true`，需配合通用配置项 `enableInitialForm` 使用）：`/gamerule sscUseInitialForm <true | false>`