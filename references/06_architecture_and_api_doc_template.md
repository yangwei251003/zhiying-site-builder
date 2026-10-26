# 阶段5 · 技术架构与 API 文档模板

## 1. 技术选型原则

- 优先选成熟、社区活跃、文档完善的方案，不为追新选实验性技术栈
- 前后端分离，方便独立迭代和未来扩展（如后续做移动端）
- 后端要把 resumecraft 的"状态机四阶段"概念映射为真实的数据模型和服务接口，而不是停留在对话提示词层面

## 2. 推荐架构示例（按用户的实际技术偏好调整，没有偏好时可参考）

- 前端：React 生态（如 Next.js）+ Tailwind CSS + shadcn/ui + Framer Motion
- 后端：Node.js（与前端同语言，团队上手成本低）或 Python（如果要更紧密复用 resumecraft 已有的 Python 模块）
- 数据库：PostgreSQL（关系型数据为主，知识库内容可用 JSONB 字段存储半结构化数据）
- 文件存储：对象存储服务，用于简历 docx 文件、用户上传的证件照
- 部署：容器化 + 主流云平台或 Vercel 类平台，视用户预算和技术能力而定

## 3. 核心数据模型（示例，需按实际功能调整）

```
User: id, email, created_at, subscription_status
KnowledgeBase: id, user_id, content(jsonb), version, updated_at
JobPosting: id, user_id, raw_text, parsed_requirements(jsonb), created_at
MatchingReport: id, knowledge_base_id, job_posting_id, result(jsonb), created_at
Resume: id, user_id, matching_report_id, content(jsonb), file_url, created_at
```

## 4. API 文档模板（每个接口照此结构写清楚）

```markdown
### POST /api/knowledge-base

**描述**：创建或更新用户知识库

**请求体**
```json
{ "basic_info": {...}, "education": [...], ... }
```

**响应**
```json
{ "id": "...", "completeness_check": {"passed": true, "missing": []} }
```

**错误码**
- 400：数据格式不合法
- 401：未登录
```

每个接口都要写清楚：路径、方法、鉴权要求、请求体结构、响应结构、可能的错误码。前后端开发者各自照着这份文档实现，避免边写边对齐导致的反复返工。

## 5. 安全与隐私要求

- 用户简历内容属于敏感个人信息，传输全程加密（HTTPS），数据库敏感字段考虑加密存储
- 鉴权机制明确（如 JWT / Session），接口要做权限校验，避免用户能访问到他人的知识库或简历数据
- 文件上传（证件照、JD截图）做格式和大小校验，防止恶意文件上传
