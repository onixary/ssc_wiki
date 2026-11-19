# 配置形态定义 JSON

形态定义 JSON 用于声明一个自定义形态的身份、分组与阶段、行为标记、体型与状态动画，存放在：

`data/<命名空间>/ssc_form/<形态名>.json`

该文件支持 `/reload` 热重载。加载后可使用管理员指令 `/shape_shifter_curse set_dynamic_form <玩家> <命名空间>:<形态名>` 进行测试

---

## 完整示例（1.10.0 格式）

```json
{
  "group": "example_namespace:example_group",
  "group_weight": 1,
  "tier": 1,
  "fallback": "shape-shifter-curse:allay_sp",
  "bodyType": "FERAL",
  "originID": "example_namespace:form_example",
  "originLayerID": "origins:origin",
  "eyeScale": 0.6,
  "flag": [
    "no_instinct",
    "no_cursed_moon_effect",
    "special_form"
  ],
  "anim": {
    "shape-shifter-curse:idle_state": {
      "controllerType": "shape-shifter-curse:sneak_anim_controller",
      "anim": { "animID": "example_namespace:form_example_idle" },
      "sneakAnim": { "animID": "shape-shifter-curse:form_feral_common_sneak_idle" }
    },
    "shape-shifter-curse:walk_state": {
      "controllerType": "shape-shifter-curse:sneak_anim_controller",
      "anim": { "animID": "shape-shifter-curse:form_feral_common_walk", "speed": 1.2, "fade": 2 },
      "sneakAnim": { "animID": "shape-shifter-curse:form_feral_common_sneak_walk" }
    },
    "shape-shifter-curse:jump_state": {
      "controllerType": "shape-shifter-curse:one_anim_controller",
      "anim": { "animID": "shape-shifter-curse:form_feral_common_jump" }
    }
  },
  "animDefault": {
    "controllerType": "shape-shifter-curse:sneak_anim_controller",
    "anim": { "animID": "example_namespace:form_example_idle" },
    "sneakAnim": { "animID": "shape-shifter-curse:form_feral_common_sneak_idle" }
  },
  "powerAnim": {
    "shape-shifter-curse:attach_side": {
      "animID": "shape-shifter-curse:bat_3_attach_side"
    }
  },
  "ExtraPower": [],
  "RemovedPower": [],
  "MasterForm": "shape-shifter-curse:feral_cat_sp",
  "PerkTree": "shape-shifter-curse:empty"
}
```

> 完整的 13 个状态动画配置范例可直接查看仓库中的 [example.json](https://github.com/onixary/shape-shifter-curse-fabric/blob/master/custom_form_pack_example/example_form_datapack/data/example_namespace/ssc_form/example.json)

---

## 核心字段说明

### 身份、分组与阶段

| 字段 | 说明 |
| --- | --- |
| `FormID` *(可选)* | 手动覆写形态 ID。一般建议省略，默认自动采用 `<命名空间>:<文件名>` |
| `group` | 形态所属的组 ID（如 `example_namespace:my_wolf`）。同组内的形态才会按阶段相互推进或回退 |
| `group_weight` | 组内同阶段随机抽取时的权重（通常为 `1`；若不希望被随机选中，如子形态，可设为 `0`） |
| `tier` | 阶段编号（取代旧版的 `groupIndex` 与 `phase`）。阶段变化形态的`初始 / 第一 / 第二 / 永久阶段`分别对应 `1 / 2 / 3 / 4`，特殊形态一般填 `1` 并配合 `special_form` 标记 |
| `fallback` | 回退形态 ID。当该数据包被移除或形态不再可用时，系统会自动将玩家安全回退至此形态 |

### 外观与能力绑定

| 字段 | 说明 |
| --- | --- |
| `bodyType` | 身体类型：`NORMAL`（人形）或 `FERAL`（四足）。四足形态建议复用模组内置的 `form_feral_common_*` 动画以节省工作量 |
| `originID` | 关联的 Origin ID，必须在 `data/<命名空间>/origins/` 及 `origin_layers/origin.json` 中注册 |
| `originLayerID` | Origin 层 ID，保持默认值 `"origins:origin"` 即可，不推荐修改 |
| `eyeScale` | 渲染使用的眼睛高度缩放（实际碰撞箱缩放仍需在 Origin 中挂载 `shape-shifter-curse:scale` power） |
| `ExtraPower` / `RemovedPower` *(可选)* | 直接在形态层额外追加内联 Power，或移除该形态原本挂载的指定 Power ID |
| `MasterForm` *(可选)* | 指定主形态 ID，用于将该形态注册为对应主形态的**子形态**（可用波纹之镜切换） |
| `PerkTree` *(可选)* | 为形态绑定指定的技能树 ID（默认无需填写或填 `shape-shifter-curse:empty`） |

### 行为标记（`flag`）

1.10.0 版本使用 `flag` 数组灵活控制形态是否参与本能、诅咒之月与抑制剂逻辑，常用标记如下：

- `special_form`：标记为特殊形态（触发特殊形态成就与对应规则）
- `no_instinct`：隐藏本能条、不累积本能（常用于特殊形态与永久形态）
- `lock_instinct`：锁定本能条显示（常用于第二阶段）
- `no_cursed_moon_effect` / `no_cursed_moon_tf_target`：不受诅咒之月影响 / 不作为诅咒之月随机变形目标
- `inhibitor_resist` / `inhibitor_immune`：抵抗普通抑制剂 / 免疫所有非创造抑制剂（常用于永久形态）
- `initial_form`：标记为初始阶段形态（允许通过食用金苹果退回变身前形态）

---

## 动画配置（`anim` / `animDefault` / `powerAnim`）

1.10.0 将旧版列表式动画改为按**状态机键（State）**映射**控制器（`controllerType`）**：

- **`anim`**：键为状态 ID（常用包括 `idle_state`、`walk_state`、`sprint_state`、`jump_state`、`fall_state`、`swim_state`、`climb_state`、`mining_state`、`attack_state`、`ride_state`、`sleep_state`、`fall_flying_state`、`flying_state`，均在 `shape-shifter-curse:` 命名空间下）
- **常用 `controllerType`**：
  - `shape-shifter-curse:one_anim_controller`：单一动画（填 `anim`）
  - `shape-shifter-curse:sneak_anim_controller`：区分站立与潜行（填 `anim` 与 `sneakAnim`）
  - `shape-shifter-curse:swim_anim_controller`：区分浮水与游泳（填 `anim` 与 `swimAnim`）
  - `shape-shifter-curse:ride_anim_controller`：区分普通骑乘与载具（填 `anim` 与 `rideVehicleAnim`）
  - `shape-shifter-curse:use_other_state_anim_controller`：直接复用另一个状态的控制器（填 `StateControllerId`）
- **`animDefault`**：默认兜底动画。四足形态（`FERAL`）务必配置，否则在未匹配状态时会恢复直立姿态
- **`powerAnim`** *(可选)*：由特定能力触发的专属覆盖动画（如攀附墙面 `shape-shifter-curse:attach_side` 等）
