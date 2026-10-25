# oralenglish-skill — 专四口语思辨陪练

一个可开源的 AI Agent Skill：用一轮一轮的英文对话训练专四口语思辨能力。

## 项目解决什么问题

专四口语考试要求考生对话题做有条理的英语即兴陈述与观点论证，但很多备考者有两个痛点：

- **不敢说、没思路**：面对一个思辨话题不知道从哪个角度切入，也没有可模仿的英文句型；
- **说了没人改**：开口之后不知道自己哪里不地道——中式英语（中文直译语序、中式搭配、逻辑断层、母语负迁移）反复出现却无人指出。

本项目用「AI 出题 + 参考句型 → 用户口头作答 → 转写、优化、点评 → 继续深挖或重答」的逐轮训练闭环，同时解决"没话可说"和"说了不知道错在哪"两个问题，让口语练习像找陪练一样有反馈、可循环、能坚持。

## 主要功能

- **逐轮训练**：每轮 = 英文思辨 Topic + 2 句参考句型（一句肯定立场、一句否定立场，纯英文、口语化、可直接模仿）
- **反馈三段式**：
  - ①【我的原句】—— 原样转写用户口述，保留原始表达
  - ②【优化句】—— 仅 1 版；优先修中式英语（直译语序、中式搭配、逻辑断层、母语负迁移），拒绝浅层同义词替换，核心改动加粗，适配口头表达
  - ③【简短点评】—— 点明本次中式英语问题，并说明逻辑、句式、用词地道度
- **【继续】**：基于当前话题深度追问，并给出新的参考句型，进入新一轮作答
- **【重新回答】**：不换话题，等待重新作答，再次转写、优化、点评，循环直到【继续】
- **边界**：不支持网页抓取、PDF、图形界面；不评判立场对错、不纠音

## 安装方法

本 Skill 是一个普通文件夹，采用"复制即安装"，不依赖第三方库、无需构建：

1. 获取 `oralenglish-skill` 文件夹（直接复制本项目的该目录即可，当前尚未托管到 GitHub）；
2. 将整个文件夹放入宿主 Agent 的 skills 目录，例如：
   - 用户级技能目录：`C:\Users\<用户名>\Doubao\skills\`
   - 工作区技能目录：`<工作区>\.user_skills\`
   - 或其他平台规定的 skills 根目录（以宿主平台为准）；
3. 重新加载或重启 Agent，技能出现在列表中即安装完成。

目录结构：

```
oralenglish-skill/
├── SKILL.md                 # 训练流程规则（状态机 + 反馈规范 + 中式英语清单）
└── agents/
    └── openai.yaml          # Agent 入口配置（显示名、默认提示词、允许隐式触发）
```

## 使用方法

1. 对 Agent 说「开始一轮专四口语思辨训练」（支持隐式触发，如"练口语""帮我出个思辨话题"）；
2. Agent 给出英文 Topic 和 2 句参考句型（一正一反）；
3. 用户用英文口头作答；
4. Agent 输出 ①【我的原句】②【优化句】③【简短点评】和两个选项；
5. 输入【继续】进入同话题深层追问；输入【重新回答】重答本轮；循环训练。

## 输入输出示例

**第 1 轮：AI 出题**

> **Topic: Should college students take part-time jobs during the semester?**
>
> **Positive:** I believe working part-time during college is a good idea, because it gives us real-world experience that textbooks simply cannot offer.
>
> **Negative:** Although part-time jobs can bring in some money, I think they may take too much time away from studying and leave students exhausted.

**用户口述作答**：

> I think part-time job is very helpful. It can let us earn some money and learn some social experience. Although it spend time, but it worth it.

**AI 反馈**：

①【我的原句】
I think part-time job is very helpful. It can let us earn some money and learn some social experience. Although it spend time, but it worth it.

②【优化句】
I think **a part-time job** is very helpful. It **allows us to** earn some money and **gain** social experience. Although it **takes up** time, it **is worth it**.

③【简短点评】
主要有三处中式英语：一是 "learn social experience" 是中式搭配，地道说法是 "gain experience"；二是 "Although...but" 是中文"虽然…但是"的直译，英文只用 Although 即可；三是 "it spend time / it worth it" 缺谓语动词形式（takes up time / is worth it）。逻辑上是"好处+让步"结构，思路成立；句式偏简单但口语中可接受；用词基础，注意搭配和主谓。最该先改的是 "learn experience → gain experience"。

请选择：
- 【继续】— 基于当前话题深度追问，开启新一轮作答
- 【重新回答】— 不换话题，等待你再次作答

**用户输入【重新回答】**：

> I think a part-time job is helpful, because it gives students money and experience. But we should not spend too much time on it, or our study will be influenced.

**AI 再次反馈**：

①【我的原句】
I think a part-time job is helpful, because it gives students money and experience. But we should not spend too much time on it, or our study will be influenced.

②【优化句】
I think a part-time job is helpful, because it gives students money and experience. But we should not spend too much time on it, or our **studies will suffer**.

③【简短点评】
这一版主谓、搭配整体自然，逻辑也清楚（正面+约束），基本无需大改。唯一明显的中式英语是 "our study will be influenced"——这是中文"受到影响"的直译，表达含糊、偏被动，地道说法是 "our studies will suffer" 或 "it will hurt our grades"。用词基础但准确，口语感好。最该改的就是 "be influenced → suffer" 这类中式被动表达。

请选择：
- 【继续】
- 【重新回答】

**用户输入【继续】**：

> **Topic（追问）: Some say part-time jobs teach students financial independence better than any course. Do you think schools should even require work experience before graduation?**
>
> **Positive:** I think requiring some work experience before graduation makes sense, because it pushes students to step out of their comfort zone and learn what the real world expects.
>
> **Negative:** While work experience is valuable, I don't think it should be required, because students have very different situations and a mandatory rule would be unfair to many of them.

（等待用户作答，进入下一轮……）

## 验证

- 官方验证脚本 `quick_validate.py` 通过（Skill is valid!）
- 已用多个思辨话题（AI 辅助作业、大学生兼职等）走查完整状态机：出题 → 作答 → 反馈 → 【重新回答】重答循环 → 【继续】同话题追问，流程与输出格式均符合规则
