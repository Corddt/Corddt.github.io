在前面，小说模拟器开发碰壁后，就进行了，反思，原因可能在于以AI目前的能力限制，还无法记忆那么多的代码内容。
后面，就浏览到了Cursor+Mcp的组合，初步看上去是通过api插件扩展了ai的知识范畴。在这个过程中，偶然了解到了Dify,然后去搜索，又发现国内的Coze起到了和其差不多的效果，因此，打开了我对Coze的一系列研究。
Coze我之前一直听说过，但一直没有去仔细的研究。
这一次，我进行了仔细的研究。
大体的浏览了Coze的基本界面后，我想到，能否通过Coze去实现一个小说模拟器的效果呢？ **说干就干**。
## 结合AI去开发AI?!
在Coze的基本界面，我犯了难，我主要发愁在于自己对于Coze的了解不多，所以很多组件都不知道该如何使用，该如何是好？
后面我偶然想起来之前不是用过ChatGpt的GPTs嘛，那么为什么不可以利用AI来帮我协助开发呢？
为了验证这个思想，我先去打开了Coze的官方技术文档。

![Image](https://github.com/user-attachments/assets/6132eaf9-03ac-4dea-8465-198f012bfc31)

在这里有对Coze的各个节点的详细使用叙述，因此，我将技术文档的这些叙述汇总到了一个markdown文档中，然后把这个变成AI的内置知识库。
我汇总总结的工作流节点文档：https://github.com/Corddt/coze-workflow-docs

![Image](https://github.com/user-attachments/assets/66dee6bc-baf0-474a-8a36-f4a98ebc3781)

后面就开始了结合这个文档去开发对应AI工作流的尝试。

## Coze工作流创建的微调提示词与微调后的工作流模板
由于我前面的文档只是把官方技术文档中的核心知识板块进行了一个汇总，因此可能有遗漏的部分。另外，AI去看这个文档，也可能会存在很多没有看到的地方，于是，在开发中，我也碰到了很多问题。

![Image](https://github.com/user-attachments/assets/6aa77057-566d-4fb7-8db5-01ca8d3b4a67)
所以出来了很多版本的工作流测试，也反复进行了多次试错。最终出来了一版较为不错的提示词，但是也经历了一些后期的微调。
分享一下我试错后的Coze工作流创建模板：
```markdown
中文回答。根据我的workflow_nodes.md文档和我的要求，帮我搭建一套工作流。
具体要说明以下问题：
1、要调用几个节点，每个节点的具体内容。
2、输入和输出的参数是什么
3、作为一个专攻AI大语言模型的提示词专家，如果某个节点需要提示词，要详细描述出提示词的具体内容。
4、作为一个计算机专家，详细说明各个节点所使用的数据的数据类型。
5、作为一个数据库专家，如果需要创建变量或者数据库信息也要详细说明。
6、变量赋值里的变量是应用变量，应用变量是用于配置应用中多处开发场景需要访问的数据，每次新请求均会初始化为默认值。
7、如果变量存在子变量，子变量要自动缩进。
8、详细说明变量是否是光在节点中创建还是说属于应用变量。

有一些注意事项：
1、根据技术文档，除了开始节点、输入节点、知识库写入节点外，其他所有节点都能读取变量值。所以输入节点没有输出的功能,所以输入节点不要给我预设输出功能。输出节点也没有输入功能，所以输出节点不要预设输入功能
2、Coze的数据库里有一些无法删除的预制内容，并且最多新增20个字段。
"""存储字段名称
id 数据的唯一标识（主键）Integer
sys_platform 数据产生或使用的渠道 String
uuid 用户唯一标识，由系统生成 String
bstudio_create_time 数据插入的时间 Time
"""
3、我不需要api密钥，因为coze内自带大模型，可以直接使用。
4、Coze的变量名长度有限制，最多不超过20个字符。
5、变量一共这几种类型，每一个变量都要详细说明属于那种类型:"""String、Integer、Boolean、Number、Object、Array<String>、Array<Integer>、Array<Boolean>、Array<Number>、Array<Object>"""

6、只有Object变量类型可以新增子项变量

整个工作流的宏观情况请用mermaid图表绘制流程图。
```
也分享我创建的工作流（基于Claude 3.7 Sonnet thinking）
[I am System](https://github.com/Corddt/coze-workflow-docs/blob/main/WorkFlowTest/I%20am%20System/I%20am%20System.md)

这是我初步搭建的工作流：

![Image](https://github.com/user-attachments/assets/666c615f-932a-4088-867e-75935a29d147)

暂时还需要较大程度的优化，所以暂时未发布到扣子商店。