# TTS 文本归一化评测集

`online_data/en.jsonl` 收录英语用例，`online_data/mul.jsonl` 收录混合语言用例。每个文件使用 UTF-8，每行一个 JSON 对象。后续批次直接追加到相应语言文件，在 `online_data/manifest.json` 登记来源、批次及筛选时的条数。

| 字段 | 含义 |
| --- | --- |
| `id`、`batch` | 稳定样例编号及来源批次。更新规则或读法时保留原编号。 |
| `language`、`locale` | 语言及地区。地区未知时取 `null`。 |
| `tags`、`rule` | 可多选的现象标签，以及本条要覆盖的具体规则。 |
| `input_text` | 人工筛选后的精简输入。多个摘录用换行连接，供阅读。 |
| `excerpts` | 按原文顺序排列的摘录。每项含 `text`、`start`、`end`。位置按原文 Unicode 码点计数，从 0 起，左闭右开。 |
| `source_sha256` | 原文 UTF-8 字节的 SHA-256，用于核对来源。 |
| `gold_text`、`human_reviewed` | 待标注时分别为 `null` 和 `false`。审核完成后，`gold_text` 填写与摘录逐项对应的标准答案字符串数组，并将 `human_reviewed` 设为 `true`。单摘录也使用单元素数组。 |

回归测试按顺序将每项 `excerpts[].text` 单独送入 normalize。每个片段的输出与对应标准答案精确匹配，整条才计为通过。通过率的分母只包含已审核且答案齐全的用例。首批 56 条已确认选用，标准答案均待审核。
