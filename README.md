# SafeChat-Guard D 组前端与测试用例交付

这是 SafeChat-Guard 项目中 D 组负责的前端展示、批量测试用例和报告素材整理部分，面向全国大学生人工智能安全竞赛定向题目 1：面向对话场景的大模型输入/输出违规内容过滤系统。

## 负责范围

D 组主要负责：

- Streamlit 前端界面
- 检测流程可视化
- 输入侧过滤基线对比展示
- 分级处理与情感保留式安全改写展示
- 输出侧二次校验展示
- 批量测试用例表与评测面板
- 日志审计、规则配置和报告截图素材页面

当前版本内置的是演示用检测逻辑，方便前端、测试和汇报先跑通。后续和组员代码融合时，应把 A/B/C 组的真实模块接入到前端调用位置。

## 文件说明

```text
.
├── app.py                 # Streamlit 前端主程序
├── requirements.txt       # 运行依赖
├── test_cases_sample.csv  # 批量评测测试用例样表
└── README.md              # 项目说明
```

运行后会自动生成：

```text
outputs/
└── interaction_logs.csv   # 前端交互日志
```

## 运行方法

建议使用 Python 3.10 或更高版本。

```powershell
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

启动后浏览器会打开本地地址，通常是：

```text
http://localhost:8501
```

如果浏览器没有自动打开，可以复制终端中的 Local URL 手动访问。

## 页面功能

- `安全总览`：查看整体命中类别、处理动作、价值观评分等统计。
- `实时检测工作台`：输入一句话，展示输入过滤、语义二次判定、分级处理、输出校验结果。
- `基线对比`：对比普通关键词过滤与增强过滤在中文变体、谐音、拆字、空格绕过上的效果。
- `安全改写`：展示中低风险内容的情感保留式安全改写效果。
- `批量评测`：上传或使用内置 CSV 测试用例，统计处理匹配率、违规处理率、误判率。
- `日志审计`：查看前端检测记录。
- `规则配置`：演示词库和规则配置入口。
- `报告素材`：集中展示可截图放入作品报告的结果。

## 测试用例表字段

`test_cases_sample.csv` 中的主要字段含义：

- `id`：测试用例编号
- `input_text`：输入文本
- `case_type`：样例类型，如正常文本、中文对抗变体、情感保留式改写
- `expected_category`：期望风险类别
- `expected_risk`：期望风险等级
- `expected_action`：期望处理动作，取值为 `pass`、`sanitize`、`block`
- `baseline_expected`：普通关键词基线预期结果
- `enhanced_expected`：增强过滤预期结果
- `expected_sentiment`：期望保留的情感倾向
- `expected_rewrite_type`：期望改写类型
- `mock_model_output`：模拟大模型输出
- `expected_output_action`：输出侧二次校验预期动作
- `note`：备注

## 和其他组员融合

建议最终融合时保持以下顺序：

1. A 组提供 `filter_input(text)`，返回归一化文本、命中类别、命中规则、风险分数、处理建议。
2. B 组提供 `semantic_classify(text)` 和大模型调用接口，返回语义风险分数和模型回复。
3. C 组提供 `filter_output(text)`、`safe_rewrite(text)` 和日志接口。
4. D 组将本前端中的演示检测逻辑替换为真实接口调用。

前端需要的统一结果格式建议包含：

```python
{
    "input": "...",
    "normalized": "...",
    "category": "ad",
    "risk": "medium",
    "score": 0.72,
    "action": "sanitize",
    "matched_rules": ["联系方式引流"],
    "rewrite": "...",
    "model_output": "...",
    "output_action": "pass"
}
```

## 注意

这个仓库是 D 组交付版，不包含完整后端模型、真实大模型 API key，也不包含其他组员尚未完成的模块。当前版本适合用于演示、截图、批量测试设计和后续系统融合。
