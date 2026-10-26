---
name: zhiying-site-builder
description: "Use this skill whenever the user wants an AI agent to design and
  build the '职映' (ZhiYing) career/resume web platform end-to-end — the product
  wrapping the resumecraft skill into a full website. Trigger on: 职映网站, 开发网站,
  做个网站, 落地页, 全流程开发, agent建站, UI/UX设计稿, 设计系统, 生图素材, 开发计划书, 技术架构文档. This is a META
  skill: it does not write resume content itself — it drives a multi-phase
  process (requirements brainstorm → PRD → design system → page UX specs →
  visual asset generation → architecture & API docs → pipeline dev plan →
  implementation → self-review → delivery docs) for building the surrounding
  product. Always treat resumecraft (if present in the environment) as the
  backend engine this site exposes; do not duplicate or contradict its rules."
disable-model-invocation: true
---

# 职映网站全流程开发技能（ZhiYing Site Builder）

> 公开归档说明：这是开发过程的Skill，不是职映网站本身。完整执行依赖宿主的编码、图像和浏览器能力，以及用户合法提供的素材；不会因安装自动发布网站。 宿主规则与用户指令优先；目录中的“必调”和“全部”只代表原设计的建议，不扩大授权或证明当前可用性。


## 这个技能是什么

这不是一个写代码的脚本集合，而是一套**强制顺序的开发流程控制器**。目的是防止 AI agent 在需求没搞清楚之前就直接开始堆代码、堆样式——那种结果通常是"看起来像个网站"但留不住人、改起来到处打补丁。

调用本技能后，agent 必须按下面九个阶段顺序推进，**每个阶段都有明确的"产出物"和"是否可以进入下一阶段"的判定，不允许跳阶段、不允许在用户还没确认理解一致前就开始写代码。**

```
阶段0 信息收集与头脑风暴      → references/00_intake_checklist.md, 01_brainstorm_guide.md
阶段1 产品需求文档(PRD)       → references/02_prd_template.md
阶段2 设计系统构建            → references/03_design_system_guide.md
阶段3 页面级 UI/UX 设计稿     → references/04_page_ux_spec_template.md
阶段4 视觉素材生成策略        → references/05_visual_asset_strategy.md
阶段5 技术架构与接口文档      → references/06_architecture_and_api_doc_template.md
阶段6 流水线式开发计划书      → references/07_dev_pipeline_plan_template.md
阶段7 开发执行（前后端实现）   → 本文件第七节
阶段8 自检与体验优化          → references/08_self_review_ux_checklist.md
阶段9 交付文档                → references/09_deliverable_docs_checklist.md
```

每个阶段开始前，先读对应的 reference 文件，再行动；不要凭记忆复述，文件里有具体的问题清单、模板结构和判定标准。

---

## 一、最高优先级原则（贯穿全部九个阶段）

1. **不理解透彻不动工**：阶段0-1 没有得到用户明确确认（哪怕用户说"差不多，可以开始了"），不允许进入设计或开发。理解有歧义时，停下来问，不要自行假设替用户做决定。
2. **设计系统不是凭空发明的**：用户已经有 IP 形象、Logo、部分功能图标，色彩系统、视觉风格必须从这些既有资产里提炼延展，而不是另起一套审美——否则网站会出现"Logo 一套风格、页面又一套风格"的割裂感。
3. **素材缺口用生图能力补，但风格必须对齐**：用户明确知道现有素材不够覆盖全站需求，agent 应主动调用当前环境可用的生图模型/技能去补齐，但每次生成前要先参照已有 IP/Logo 的风格特征（色调、线条粗细、圆角程度、光影风格等），不能各页面风格各异。
4. **诚信原则延伸到网站本身**：resumecraft 技能的核心是"真实匹配，不编造能力"，这个网站作为它的载体，文案、营销话术、功能承诺也不能夸大或暗示"帮你包装出虚假履历"，要在产品层面保持同一套价值观，这样品牌才立得住。
5. **借鉴优秀案例，不照抄版权内容**：鼓励 agent 去研究市场上优秀的 SaaS / 求职类产品的设计语言、交互模式、信息架构思路作为参考和启发，但不能直接复制具体网站的图片、图标、文案、专属配色方案等受版权保护的素材；最终素材要么来自用户提供，要么是 agent 调用生图能力原创生成。
6. **效率：多用现成的，少手搓**：组件库、动效库、图标库、生图工具——只要环境里有现成且质量过关的方案，优先复用，不要重新发明轮子；这能省下的时间应该花在打磨细节体验上。

---

## 二、阶段0：信息收集与头脑风暴

读 `references/00_intake_checklist.md`（需要用户提供哪些信息/素材）和 `references/01_brainstorm_guide.md`（头脑风暴问题清单与交流方式）。

核心要求：
- 用对话方式逐步问，不要一次甩出几十个问题让用户填表
- 每问完一批，做一次小结复述给用户确认理解正确
- 用户提供的 Logo / IP 形象 / 图标文件，第一时间打开实际看一遍（不要只读文件名），分析其色彩、线条风格、情绪调性，这是后面设计系统的原始输入
- 本阶段结束的标志：agent 能用一段话准确说清楚"这个网站是为谁做的、解决什么问题、核心是什么、用户会怎么用它、商业模式是什么"，并得到用户确认

## 三、阶段1：产品需求文档（PRD）

读 `references/02_prd_template.md`。把阶段0 收集到的信息整理成结构化 PRD，包含完整的网站地图（sitemap）、核心用户旅程、功能优先级（MVP vs 后续迭代）。**PRD 写完后呈现给用户确认，不要直接跳去设计。**

## 四、阶段2：设计系统构建

读 `references/03_design_system_guide.md`。基于阶段0 提炼的 Logo/IP 风格特征，产出完整设计系统：色彩体系（主色/辅助色/中性色/语义色，含具体色值）、字体系统（中英文字体配对、字号阶梯）、间距与网格、组件视觉规范（圆角/阴影/边框）、动效原则（缓动曲线、时长、使用场景）、目标组件库选型（如 shadcn/ui + Tailwind + Framer Motion，或其他匹配技术栈的方案）。这一份是后续所有页面设计和开发的共同依据，必须先于页面设计完成。

## 五、阶段3：页面级 UI/UX 设计稿

读 `references/04_page_ux_spec_template.md`。逐页面、逐模块产出设计稿说明（用文字+布局结构描述，或调用环境内可用的可视化工具产出实际线框/视觉稿），**首页 Hero 区是重中之重**，要写清楚主视觉构图、核心文案、视觉焦点引导路径、首屏转化目标。每个功能模块区（知识库构建、岗位匹配分析、简历生成预览、个人中心等）都要单独过一遍：用户在这一区会做什么、看到什么、视觉如何引导他完成下一步动作。

设计追求"自然沉浸"：减少用户需要思考"接下来该干什么"的瞬间，关键路径上的下一步动作要在视觉上明显、唯一、有吸引力。

## 六、阶段4：视觉素材生成策略

读 `references/05_visual_asset_strategy.md`。盘点阶段3 设计稿里每个位置需要什么视觉素材，标注"已有素材可用 / 需要生图补齐"，对需要生图的部分整理出一致的风格 prompt 基准（基于阶段2 的设计系统），再调用环境内可用的生图模型/技能批量生成，生成后实际打开检查风格是否统一、是否与 Logo/IP 协调，不一致的重新生成而不是凑合用。

## 七、阶段5-7：架构文档、开发计划、动工实现

依次读 `references/06_architecture_and_api_doc_template.md`、`references/07_dev_pipeline_plan_template.md`。

技术架构选型原则：选成熟、生态好、招得到人维护的方案，不为了炫技选小众技术栈；前后端接口要先把 API 文档写清楚（路径、方法、请求/响应结构、错误码、鉴权方式），再开始写实现代码，避免前后端对不齐返工。

开发计划书要拆到"可执行粒度"——不是"做前端"这种笼统任务，而是具体到"实现 Hero 区组件 + 接入设计系统 token + 响应式适配"这种可以直接认领的任务单元，按里程碑分组，每个里程碑有明确的可验收产出。

动工实现阶段的执行要求：
- 严格遵循阶段2 的设计系统，不要在写代码时随手发明新的颜色/字号/间距
- 多用现成组件库，但要做二次定制，避免"一眼看出是默认组件库样式没改过"的廉价感
- 动效服务于体验而不是炫技：页面切换、状态反馈、加载态都该有动效，但克制，不要让动效拖慢操作或喧宾夺主
- 前后端都要写，后端要把 resumecraft 的四阶段流程封装成网站可调用的服务接口，状态管理（用户在哪个阶段、知识库内容）要真正持久化到数据库，不能只存在单次会话里

## 八、阶段8：自检与体验优化

网站第一版做完后，读 `references/08_self_review_ux_checklist.md`，**真正代入一个新用户的角色**，从打开首页到生成出一份简历完整走一遍流程，记录每一个"卡顿、犹豫、看不懂、想退出"的瞬间，列出具体问题和对应的优化方案，改完后再走一遍，直到体验顺畅为止。不要自己说"做完了""挺好的"就结束——必须有这个真实走查的产出记录。

## 九、阶段9：交付文档

读 `references/09_deliverable_docs_checklist.md`，产出开发文档、测试文档、运维文档、用户文档四类交付物。文档要让"换一个新人接手"也能看懂怎么部署、怎么测、出了问题怎么排查、用户怎么用。

---

## 十、参考文件索引

```
references/
├── 00_intake_checklist.md             阶段0：需要用户提供的信息与素材清单
├── 01_brainstorm_guide.md             阶段0：头脑风暴问题与交流方式
├── 02_prd_template.md                 阶段1：PRD 结构模板
├── 03_design_system_guide.md          阶段2：设计系统构建方法论
├── 04_page_ux_spec_template.md        阶段3：页面级 UI/UX 设计稿模板
├── 05_visual_asset_strategy.md        阶段4：视觉素材生成策略
├── 06_architecture_and_api_doc_template.md  阶段5：技术架构与API文档模板
├── 07_dev_pipeline_plan_template.md   阶段6：流水线式开发计划书模板
├── 08_self_review_ux_checklist.md     阶段8：用户视角自检清单
└── 09_deliverable_docs_checklist.md   阶段9：交付文档清单
```
