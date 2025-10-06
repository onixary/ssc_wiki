# 包结构示例

请参考[Github仓库](https://github.com/onixary/shape-shifter-curse-fabric/tree/master/custom_form_pack_example)中的示例

一个自定义形态由资源包与数据包两部分组成。资源包负责形态模型与动画的实现，而数据包负责形态能力、种类、体型等的定义

对于一个完整的自定义形态而言，两者需要被同时加载并应用

## 资源包结构

```
example_form_resourcepack // 资源包根目录
├── assets 
│   ├── example_namespace // 自定义形态的命名空间。始终使用自己的命名空间来防止冲突
│   │   ├── lang // 本地化文件目录
│   │   │   ├── en_us.json // 英语本地化文件
│   │   │   └── zh_cn.json // 中文本地化文件
│   │   ├── rich_lang // 富文本本地化文件目录
│   │   │   ├── en_us.json // 英语本地化文件
│   │   │   └── zh_cn.json // 中文本地化文件
│   │   └── player_animation // 自定义动画文件目录
│   │       └── form_example_idle.json // 自定义动画文件示例，使用AzureLib格式导出
│   └── orif-defaults // 自定义模型与动画文件目录，使用OriginFur mod的格式
│       ├── furs // 自定义形态模型json定义目录
│       │   └── example_namespace.form_example.json // 自定义形态模型的json定义文件，包含贴图与软骨骼等的相关配置
│       └── geo // 自定义形态模型目录
│           └── form_example.geo.json // AzureLib geo格式的导出模型文件
└── pack.mcmeta // 包描述文件
```

## 数据包结构

```
example_form_datapack // 数据包根目录
├── data
│   ├── example_namespace // 自定义形态的命名空间，与资源包中的命名空间保持一致
│   │   ├── origins // origins的origin定义目录
│   │   │   └── form_example.json // 自定义形态所对应的origin定义
│   │   ├── powers // origins的powers 自定义能力目录
│   │   │   ├── custom_eat_amethyst_shard.json // powers自定义能力定义示例
│   │   │   └── custom_night_vision.json
│   │   └── ssc_form // 形态定义JSON文件目录
│   │       └── example.json // 自定义的形态定义JSON文件。动画、形态种类、体型等都在此处定义
│   └── origins\origin_layers // origin_layers目录
│       └── origin.json // origin_layers注册文件。只有将example_namespace:example添加在其中才能够被origins识别
└── pack.mcmeta // 包描述文件

```