# 附加功能

Workflow 和 Chatflow 应用均支持开启附加功能以增强使用者的交互体验。例如添加文件上传入口、给 LLM 应用添加一段自我介绍或使用欢迎语，让应用使用者获得更加丰富的交互体验。

点击应用右上角的 **“功能”** 按钮即可为应用添加更多功能。

{% @arcade/embed flowId="a0tbwuEIT5I3y5RdHsJp" url="https://app.arcade.software/share/a0tbwuEIT5I3y5RdHsJp" %}

### Workflow

Workflow 类型应用仅支持 **“图片上传”** 功能。开启后，Workflow 应用的使用页将出现图片上传入口。

{% @arcade/embed flowId="DqlK9RV79K25ElxMq1BJ" url="https://app.arcade.software/share/DqlK9RV79K25ElxMq1BJ" %}

**用法：**

**对于应用使用者而言：**已开启图片上传功能的应用的使用页将出现上传按钮，点击按钮或粘贴文件链接即可完成图片上传，你将会收到 LLM 对于图片的回答。

**对于应用开发者而言：**开启文件图片上传功能后，使用者所上传的图片文件将存储在 `sys.files` 变量内。接下来添加 LLM 节点，选中具备视觉能力的大模型并在其中开启 VISION 功能，选择 `sys.files` 变量，使得 LLM 能够读取该图片文件。

最后在 END 节点内选择 LLM 节点的输出变量即可完成设置。

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption><p>开启视觉分析能力</p></figcaption></figure>

### Chatflow

Chatflow 类型应用支持以下功能：

*   **对话开场白**

    让 AI 主动发送一段话，可以是欢迎语或 AI 的自我介绍，以拉近与使用者的距离。
*   **下一步问题建议**

    在对话完成后，自动添加下一步问题建议，以提升对话的话题深度与频率。
*   **文字转语音**

    在问答文字框中添加一个音频播放按钮，使用 TTS 服务（需在[模型供应商](../../getting-started/readme/model-providers.md)内置）并朗读其中的文字。
*   **文件上传**

    支持以下文件类型：文档、图片、音频、视频以及其它文件类型。开启此功能后，应用使用者可以在应用对话的过程中随时上传并更新文件。最多支持同时上传 10 个文件，每个文件的大小上限为 15MB。

    <figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>文件上传功能</p></figcaption></figure>


*   **引用和归属**

    常用于配合[“知识检索”](node/knowledge-retrieval.md)节点共同使用，显示 LLM 给出答复的参考源文档及归属部分。
*   **内容审查**

    支持使用审查 API 维护敏感词库，确保 LLM 能够回应和输出安全内容，详细说明请参考[敏感内容审查](../application-orchestrate/app-toolkits/moderation-tool.md)。

**用法：**

除了**文件上传**功能以外，Chatflow 应用内的其它功能用法较为简单，开启后可以在应用交互页直观使用。

本章节将主要介绍**文件上传**功能的具体用法：

**对于应用使用者而言：**已开启文件上传功能的 Chatflow 应用将会在对话框右侧出现 “回形针” 标识，点击后即可上传文件并与 LLM 交互。

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption><p>使用文件上传</p></figcaption></figure>

**对于应用开发者而言：**

开启文件上传功能后，使用者所上传的文件将存储在 `sys.files` 变量内。如果用户在对话的过程中上传了新文件，该变量中的文件将会被覆盖。

根据上传的文件差异，不同类型的文件对应不同的应用编排方式。

* **文档文件**

LLM 并不具备直接读取文档文件的能力，因此需要使用 [文档提取器](node/doc-extractor.md) 节点预处理 `sys.files` 变量内的文件。编排步骤如下：

1. 开启 Features 功能，并在文件类型中仅勾选 “文档”。
2. 在[文档提取器](node/doc-extractor.md)节点的输入变量中选中 `sys.files` 变量。
3. 添加 LLM 节点，在系统提示词中选中文档提取器节点的输出变量。
4. 在末尾添加 “直接回复” 节点，填写 LLM 节点的输出变量。

使用此方法搭建出的 Chatflow 应用无法记忆已上传的文件内容。应用使用者每次对话时都需要在聊天框中上传文档文件。如果你希望应用能够记忆已上传的文件，请参考 [《文件上传：在开始节点添加变量》](file-upload.md#fang-fa-er-zai-tian-jia-wen-jian-bian-liang)。

<figure><img src="../../.gitbook/assets/image (372).png" alt=""><figcaption><p>文档文件编排</p></figcaption></figure>

* **图片文件**

部分 LLM 支持直接获取图片中的信息，因此无需添加额外节点处理图片。

编排步骤如下：

1. 开启 Features 功能，并在文件类型中仅勾选 “图片”。
2. 添加 LLM 节点，启 VISION 功能并选择 `sys.files` 变量。
3. 在末尾添加 “直接回复” 节点，填写 LLM 节点的输出变量。

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>开启视觉分析能力</p></figcaption></figure>

* **混合文件类型**

若希望应用具备同时处理文档文件 + 图片文件的能力，需要用到 [列表操作](node/list-operator.md) 节点预处理 `sys.files` 变量内的文件，提取更加精细的变量后发送至对应的处理节点。编排步骤如下：

1. 开启 Features 功能，并在文件类型中勾选 “图片” + “文档文件” 类型。
2. 添加两个列表操作节点，在 “过滤” 条件中提取图片与文档变量。
3. 提取文档文件变量，传递至 “文档提取器” 节点；提取图片文件变量，传递至 “LLM” 节点。
4. 在末尾添加 “直接回复” 节点，填写 LLM 节点的输出变量。

应用使用者同时上传文档文件和图片后，文档文件自动分流至文档提取器节点，图片文件自动分流至 LLM 节点以实现对于文件的共同处理。

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption><p>混合文件处理</p></figcaption></figure>

* **音视频文件**

LLM 尚未支持直接读取音视频文件，Dify 平台也尚未内置相关文件处理工具。应用开发者可以参考 [外部数据工具](../extension/api-based-extension/external-data-tool.md) 接入工具自行处理文件信息。

### 常见问题

**在 Dify Cloud 中，应用使用者在聊天框中上传的文件将保存多久？**

持久保存。
