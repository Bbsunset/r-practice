# 复旦大学课程项目：社交媒体新闻使用与政治态度分析

---

- 项目名称：*Social Media News Use and Political Attitudes*
- 作者：张三
- 课程：计算社会科学导论
- 时间：2026年春季学期
- 状态：**初稿**
- 版本：v0.3

## 项目简介：
本项目基于 [ANES 2024 Time Series](https://electionstudies.org/) 数据，尝试分析社交媒体新闻使用程度与美国公众在移民、枪支、种族、性别议题上的态度之间是否存在系统性关系。项目使用 Python 进行数据清洗、变量构建、描述统计和回归分析。

---

## 研究问题：
1. 社交媒体新闻使用程度是否与更极化的政治态度相关？
2. 不同平台的新闻使用是否对应不同议题上的态度差异？
3. 在控制年龄、性别、教育、收入、党派认同等变量后，这种关系是否仍然存在？

### 核心变量：
- 自变量：
	- `social_media_news_index`
- 因变量：
	- immigration_crime_attitude
	- birthright_citizenship_attitude
	- gun_control_attitude
	- racial_discrimination_attitude
	- transgender_military_attitude
### 控制变量：
- age_group
- gender
- race
- education
- income
- party_id
- state_fixed_effects

> **注意：**
> social_media_news_index 是由多个社交媒体新闻使用相关变量标准化后平均得到的综合指标。部分变量需要反向编码。

## 数据来源:
[ANES 2024 Time Series Study](https://electionstudies.org/)
代码仓库：https://github.com/example/anes-social-media-project

## 图片占位：
![项目流程图](https://dummyimage.com/800x300/eeeeee/000000&text=Research+Workflow)

## 变量说明表：
| 变量名 | **类型** | 含义 | 处理方式 |
| ----- | ------- | --- | ------- |
| social_media_news_index | continuous | 社交媒体新闻使用指数 | Z-score standardization |
| gun_control_attitude | binary | 是否支持禁用攻击性武器 | Logistic regression |
| party_id | categorical | 党派认同 | 控制变量 |
| education | ordinal | 教育程度 | 控制变量 |
| income | ordinal | 收入水平 | 控制变量 |

## 分析流程：
- 第一步：读取数据
- 第二步：处理缺失值
- 第三步：反向编码部分变量
- 第四步：构建 social_media_news_index
- 第五步：进行描述统计
- 第六步：绘制相关矩阵
- 第七步：建立加权 OLS 或 Logistic 回归模型
- 第八步：解释结果与局限

### Python 示例代码：
```
import pandas as pd
import numpy as np

df = pd.read_csv("anes2024.csv")

media_vars = ["facebook_news", "twitter_news", "instagram_news", "tiktok_news"]

for col in media_vars:
    df[col + "_z"] = (df[col] - df[col].mean()) / df[col].std()

df["social_media_news_index"] = df[[col + "_z" for col in media_vars]].mean(axis=1)

print(df["social_media_news_index"].describe())
```
### R 示例代码：
```
library(dplyr)

anes_clean <- anes %>%
  mutate(
    social_media_news_index = rowMeans(
      across(c(facebook_news_z, twitter_news_z, instagram_news_z, tiktok_news_z)),
      na.rm = TRUE
    )
  )
```
## 数学表达：
social_media_news_index = average of standardized social media news variables
z = x minus mean divided by standard deviation
回归模型：
$$
Y_i = beta_0 + beta_1 social_media_news_index_i + beta_2 controls_i + gamma_state + epsilon_i
$$

## 待办事项：
- [x] 下载 ANES 数据
- [x] 阅读 codebook
- [ ] 完成变量反向编码
- [ ] 完成描述统计表
- [ ] 完成回归模型
- [ ] 撰写结果解释
- [ ] 检查引用格式

## 引用材料：
Green, Palmquist, and Schickler. Partisan Hearts and Minds.
ANES 2024 Time Series Study.
Downs. An Economic Theory of Democracy.

需要删除的旧方案：
原计划直接使用所有社交媒体使用变量相加，不进行标准化。这个方案存在量纲不一致问题，因此废弃。

## 研究提醒：
> 不要把相关关系直接解释为因果关系。
> 不要忽略党派认同这一关键控制变量。
> 如果可能，需要考虑内生性问题。

脚注内容：
标准化的目的在于让不同量纲的变量具有可比性。[^1]
固定效应用于吸收州层面的不可观测差异。[^2]

[^1]: 标准化的目的在于让不同量纲的变量具有可比性。
[^2]: 固定效应用于吸收州层面的不可观测差异。

<details>
	<summary>补充说明：</summary>
如果读者不了解 Z-score，可以展开解释。
如果读者熟悉统计学，可以跳过基础说明。
<\details>
