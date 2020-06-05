---
title: "Part I: Development Opportunities of Micro-Entrepreneurs: Evidence from Kenya"
tags:
  - Policy development
  - BS Economics
  - Data-Analysis
  - Stata
  - Logistic Regression
  - Multivariate Regression
date: June 3, 2020
header:
  teaser: /assets/images/thumbnails/Bachelor-Thesis.png
excerpt: "In this series I summarize the main findings of my data analysis according micro-entrepreneurs in Kenya"

---
  {: .text-justify}
# 1. Introduction

<i class="far fa-sticky-note"></i> **Note:** If you are interested in the full depth of this analysis as well as recent research visit the [portfolio](/portfolio/) section to view the full bachelor thesis including the Stata code.
  {: .notice--info}

_"Understanding the reason that these firms  have low profits and why they generate so little employment is key for designing policy to improve the welfare of the urban poor.”_ (Brooks, Donovan & Johnson 2018: 197)  
This paper aims to shed light on differences between business sectors to investigate target audiences for specific development approaches. Furthermore it elaborates whether micro-enterprises of different business sectors vary in their growth probabilities and profits while receiving either class training or mentoring. Beyond it explains possible differences on the macroeconomic level. Therefore the research question is specified as follows:

Research Question:  
_To what extent do micro-enterprises of individual business sectors in developing countries benefit differently if they receive either class training or mentoring?_

To answer the research question an investigation of the gathered data during the development study conducted by Brooks et al. (2018) has been made. At the end of 2014, they assigned female micro-entrepreneurs from Kenya to groups that receive class training, mentoring or neither to elaborate differences in profits over the next 17 months after the treatment.

# 2. Descriptive Statistics
## 2.1 Micro-Enterprises and Treatment Characteristics

![Descriptive Statistics of Micro-Enterprises among Business Sectors](/assets/images/posts/03_06_20/table1.png)

The sample includes 368 inexperienced, female business owners who participated in follow-up surveys that generated time series data of variables that may correlate with business performance. The field experiment was conducted from end of 2014 to April 2016 while the treatment period lasted one month in November 2014. For this micro-enterprises were randomly assigned into three groups.
First mentorship, within this group each micro-entrepreneur is paired with an experienced mentor, who has been operating in the same business sector for at least 5 years or is older than 40 years. Second class training, formal business classes are provided to micro-entrepreneurs to improve knowledge in business practices. Third, the control group who receives neither.
Table 1 reproduces baseline profits of micro-enterprises in each business sector, grouped by their assigned treatment group. The pooled values of each group is a simple first indicator to get familiar with the values. Thereby the control group achieves the highest average baseline profit but has also the highest standard deviation. Another important value is the number of observations per group that is nearly similar due to random distribution.

<i class="far fa-sticky-note"></i> **Note:** In fact, the other values are very interesting but go beyond the summary at this point. Recall, check the full paper for further insights!
  {: .notice--info}

## 2.2 Cycling Profits among Business Sectors

![Micro-Enterprise Profits among Business Sectors](/assets/images/posts/03_06_20/figure1.png)

In the following section the average profits of each business sector are described to show off some evidence of economic cycles in Dandora, Kenya. Unfortunately the observed period only has a length of 17 months thus a delimitation of high and low economic cycles is only valid to a limited extent. Hence the follow-up waves in terms of months as "t ∈ {1,2,3,4,7,12,17}" . The economic cycle is assumed to “start” two months after baseline survey (wave 2) with low economic activity with increases in profits. Three months after baseline survey (wave 3 & 4) the economic activity somewhat stagnates seven months after baseline survey (wave 5) and reaches maximum profits and is determined as period of high economic activity. 12 months after baseline survey (wave 6) the economic activity dropped again which suggests this to be a low economic period. In wave 7 (17 months after baseline survey) is determined as a period with rising economic activity in the next economic cycle (See Figure 1).It is important to understand the dynamics of the treatments effects in the business sectors over time. The sample data of Dandora’s Micro-Enterprises is a suitable approximation of the economics activity of the whole country measured by the GDP of Kenya.

<i class="far fa-sticky-note"></i> **Note:** Evidence and further explanations are delivered in the full view!
  {: .notice--info}

Finally, we got an introduction to the topic and got familiar with the business as well as economic characteristics.

Catch up with ANCOVA (ANalysis of COVAriances) and Logistic Regression that delivered the **results in [part II](.......)** or skip this part to directly enter the **discussion and conclusion section in [part III](.......)**.
  {: .text-justify}
