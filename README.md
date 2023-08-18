# 智海-三乐

![image](https://github.com/zhihaiLLM/wisdomBot/assets/142485850/212f3b61-a429-4ec6-a906-f172f25203f1)


## 🔥最新动态


## 🔍目录
- [项目背景](#项目背景)
- [技术路线](#技术路线)
- [模型训练](#模型训练)
  - [无监督训练](#无监督训练)
  - [监督微调](#监督微调)
- [模型评测](#模型评测)
  - [中文评测](#中文评测)
- [未来规划](#未来规划)
- [致谢](#致谢)

### 💡项目背景
> 智海-三乐是由[浙江大学](https://www.zju.edu.cn/)与[高等教育出版社](http://www.hep.com.cn/)联合[阿里云计算有限公司](https://cn.aliyun.com/)、[华院计算](http://unidt.cn/)等单位共同设计研发的教育大模型，在新一代人工智能系列教材基础上，以教科书级高质量语料打造人工智能领域教育大模型，以“教材建设、课程共享和平台增效”三位一体形成数字化和智能化的教学基座能力，赋能101计划核心课程《人工智能引论》的教学育人。

### 🔧技术路线
1. 激发中文能力
    - 用通用中文数据微调训练
    - 利用现成中文大模型做初步实验
2. 精确教育知识
    - 用教材等专业语料无监督微调
3. 掌握教育指令
    - 构造指令数据集，涵盖多项教育任务，进行指令微调

我们的方法结合了布鲁姆教育分类法，另外，我们在推理过程中开发了两种检索增强方法，提高了模型响应的准确性和专业性，整个流程概图如下：

![image](https://github.com/zhihaiLLM/wisdomBot/assets/142485850/ea5c766c-432d-4800-bb50-6013695b7c37)






我们的模型基座目前是[Qwen-7B](https://github.com/QwenLM/Qwen-7B)，在此基础上，进行无监督训练以及指令微调得到我们的模型**wisdomBot**，wisdomBot功能图如下：
![image](https://github.com/zhihaiLLM/wisdomBot/assets/142485850/ace17965-15c3-47ce-9879-84bab57684ec)


下面是我们模型的部分应用场景：
![image](https://github.com/zhihaiLLM/wisdomBot/assets/142485850/d96686fe-1015-4d68-8106-fa7e4ba5756d)








### 🚀模型训练
#### 无监督训练

对语料库进行浅度清洗，去除空格和错误文件。

#### 监督微调

目前收集了教育指令模板1000条，用于后续构造高质量教育指令，这些指令主要应用于知识问答、试题生成和智能导学等教育任务。

布鲁姆教育分类法将认知学习目标分为六个层次，从低到高依次为：记忆（Remember）、理解（Comprehension）、应用（Application）、分析 (Analysis)、评价（Evaluation）和创造（Create），下面是结合布鲁姆教育分类法的一些教育指令：

<table>
  <tr>
    <th>任务</th>
    <th>指令</th>
    <th>类别</th>
  </tr>
  <tr>
    <td rowspan="6">知识问答</td>
    <td>神经网络的基本组成元素是什么?</td>
    <td>记忆</td>
  </tr>
  <tr>
    <td>如何理解激活函数在神经网络中的作用? </td>
    <td>理解</td>
  </tr>
  <tr>
    <td>在神经网络与深度学习中，如何选择合适的优化器? </td>
    <td>应用</td>
  </tr>
  <tr>
    <td>在神经网络与深度学习中，为什么需要进行参数初始化，它如何影响模型的训练?</td>
    <td>分析</td>
  </tr>
  <tr>
    <td>神经网络与深度学习相比传统机器学习算法有哪些优势和劣势? </td>
    <td>评价</td>
  </tr>
  <tr>
    <td>在概率图模型中，神经网络如何与概率图模型相结合，形成如变分自编码器(Variational Autoencoder, VAE)或生成对抗网络(Generative Adversarial Networks, GAN)这样的模型?</td>
    <td>创造</td>
  </tr>
  <tr>
    <td>试题生成</td>
    <td>给我生成一个关于概率推理在深度学习或神经网络中的应用的较难的单项选择题，并给出答案和每个选项的解析。要求该题目应涉及该概念的核心原理和深入内涵，需要对概念有较深入理解才能解答。</td>
    <td>创造</td>
  </tr>
  <tr>
    <td>智能导学</td>
    <td>我明天要教授机器学习中的深度学习的内容，请为我生成一个教学大纲。</td>
    <td>创造</td>
  </tr>
</table>

----

经过Azure OpenAI API的筛选和修改，我们目前收集了38784条指令教育指令，同时提交给Azure OpenAI API生成答案，用于wisdomBot的后续监督微调。





### 🚨模型评测
#### 中文评测
[C-Eval](https://cevalbenchmark.com/index.html)数据集是一个全面的中文基础模型评估套件。它包含了13948个多项选择题，涵盖了52个不同的学科和四个难度级别。

wisdomBot模型的zero-shot准确率结果如下：

| model | STEM | Social Science | Humanities | Other | Hard | Average |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Qwen-7B-Chat | 43.46 | 68.55 | 58.34 | 55.22 | 28.69 | 53.92 |
| wisdomBot | 50.34 | 72.91 | 62.98 | 54.95 | 36.01 | 58.33 |

另外，我们针对C-Eval上一些大学学科单独进行评测，另外再加上我们搜集到的一些人工智能试题进行zero-shot评测，结果如下：
| model | computer network | operating system | computer architecture | college programming | probability and statistics | discrete mathmatics | artificial intelligence | average |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|Qwen-7B-Chat | 42.10 | 42.10 | 42.85 | 37.83 | 22.22 | 0.00 | 54.01 | 34.44 |
| wisdomBot | 68.42 | 47.37 | 47.62 | 51.35 | 44.44 | 18.75 | 57.66 | 47.94 |



### ⏰未来规划
- 进一步的调研与训练
- 多模态功能的加入

### 💖致谢
