# 文档提取器

### 定义

LLM 自身无法直接读取或解释文档的内容。因此需要将用户上传的文档，通过文档提取器节点解析并读取文档文件中的信息，转化文本之后再将内容传给 LLM 以实现对于文件内容的处理。

### 应用场景

* 构建能够与文件进行互动的 LLM 应用，例如 ChatPDF 或 ChatWord；
* 分析并检查用户上传的文件内容；

### 节点功能

文档提取器节点可以理解为一个信息处理中心，通过识别并读取输入变量中的文件，提取信息后并转化为 string 类型输出变量，供下游节点调用。

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption><p>文档提取器节点</p></figcaption></figure>

文档提取器节点结构分为输入变量、输出变量。

#### 输入变量

文档提取器仅接受以下数据结构的变量：

* `File`，单独一个文件
* `Array[File]`，多个文件

文档提取器仅能够提取文档类型文件中的信息，例如 TXT、Markdown、PDF、HTML、DOCX 格式文件的内容，无法处理图片、音频、视频等格式文件。

#### 输出变量

输出变量固定命名为 text。输出的变量类型取决于输入变量：

* 输入变量为 `File`，输出变量为 `string`
* 输入变量为 `Array[File]`，输出变量为 `array[string]`

> Array 数组变量一般需配合列表操作节点使用，详细说明请参考 [list-operator.md](list-operator.md "mention")。

### 配置示例

在一个典型的文件交互问答场景中，文档提取器可以作为 LLM 节点的前置步骤，提取应用的文件信息并传递至下游的 LLM 节点，回答用户关于文件的问题。

本章节将通过一个典型的 ChatPDF 示例工作流模板，介绍文档提取器节点的使用方法。

<figure><img src="../../../.gitbook/assets/image (373).png" alt=""><figcaption><p>ChatPDF 工作流</p></figcaption></figure>

**配置流程：**

1. 为应用开启文件上传功能。在 [“开始”](start.md) 节点中添加**单文件变量**并命名为 `pdf`。
2. 添加文档提取节点，并在输入变量内选中 `pdf` 变量。
3. 添加 LLM 节点，在系统提示词内选中文档提取器节点的输出变量。LLM 可以通过该输出变量读取文件中的内容。

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption><p>填写文档提取器的输出变量</p></figcaption></figure>

4\. 配置结束节点，在结束节点中选择 LLM 节点的输出变量。

配置完成后，应用将具备文件上传功能，使用者可以上传 PDF 文件并展开对话。

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
如需了解如何在聊天对话中上传文件并与 LLM 互动，请参考 [附加功能](../additional-features.md)。
{% endhint %}
