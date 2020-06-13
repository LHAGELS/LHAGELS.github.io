---
title: "Part I: Development Opportunities of Micro-Entrepreneurs: Evidence from Kenya"
tags:
  - Policy development
  - BS Economics
  - Data-Analysis
  - Stata
  - Logistic Regression
  - Multivariate Regression
  - Micro-Entrepreneurs

date: June 3, 2020
header:
  teaser: /assets/images/thumbnails/bachelor_thesis.png
excerpt: "In this series I summarize the main findings of my data analysis according micro-entrepreneurs in Kenya"
toc: true
toc_sticky: false
toc_label: "Dive into the topic and get a first Impression!"

---

# 1. Introduction

<i class="far fa-sticky-note"></i> **Note:** If you are interested in the full depth of this analysis as well as recent research check out the full paper [Development Opportunities of Micro-Entrepreneurs: Evidence from Kenya](/assets/docs/bachelor_thesis.pdf) including the Stata code.
  {: .notice--info}

{: .text-justify}
<blockquote>
"Understanding the reason that these firms have low profits and why they generate so little employment is key for designing policy to improve the welfare of the urban poor."  
(Brooks, Donovan & Johnson 2018: 197)
</blockquote>
{: .text-justify}
This paper aims to **shed light on differences between business sectors to investigate target audiences for specific development approaches**. Furthermore, it elaborates whether micro-enterprises of different business sectors vary in their **growth probabilities** and **profits** while receiving either class training or mentoring. Beyond it explains possible differences on the **macroeconomic level**.

Research question:
{: .text-justify}
<blockquote>
To what extent do micro-enterprises of individual business sectors in developing countries benefit differently if they receive either class training or mentoring?
</blockquote>

{: .text-justify}
To answer the research question an **investigation of the gathered data during the development study conducted by Brooks et al. (2018)** has been made. At the end of 2014, they assigned **female micro-entrepreneurs** from Kenya to groups that **receive class training, mentoring or neither** to elaborate differences in profits over the next 17 months after the treatment.

# 2. Descriptive Statistics
## 2.1 Micro-Enterprises and Treatment Characteristics
<center>
  <figure style="width: 80%">
    <img src="{{ site.url }}{{ sitebaseurl }}/assets/images/posts/03_06_20/table1.png" alt="Descriptive Statistics of Micro-Enterprises among Business Sectors">
  </figure>
</center>

{: .text-justify}
The sample includes **368 inexperienced, female business owners** who participated in **7 follow-up surveys** that generated time series data of **variables that may correlate with business performance**. The **field experiment** was conducted from end of 2014 to April 2016 while the treatment period lasted one month in November 2014. For this micro-enterprises were **randomly assigned into three groups**.
First **mentorship**, within this group each micro-entrepreneur is **paired with an experienced mentor**, who has been operating in the same business sector for at least 5 years or is older than 40 years. Second **class training**, formal **business classes are provided** to micro-entrepreneurs to improve knowledge in business practices. Third, the **control group who receives neither**.
Table 1 reproduces baseline profits of micro-enterprises in each business sector, grouped by their assigned treatment group. The pooled values of each group is an easy indicator to get familiar with the values. Thereby the control group achieves the highest average baseline profit but has also the highest standard deviation. Another important value is the number of observations per group that is nearly similar due to random distribution (this is getting important when conducting an ANCOVA Regression!).

<i class="far fa-sticky-note"></i> **Note:** In fact, the other values are very interesting but go beyond the summary at this point. Recall, check the full paper for further insights!
  {: .notice--info}

## 2.2 Cycling Profits among Business Sectors
<center>
  <figure style="width: 60%">
    <img src="{{ site.url }}{{ sitebaseurl }}/assets/images/posts/03_06_20/figure1.png" alt="Micro-Enterprise Profits among Business Sectors">
  </figure>
</center>

{: .text-justify}
In the following section the **average profits of each business sector** are described to show off some **evidence of economic cycles** in Dandora, Kenya. Unfortunately the observed period only has a length of 17 months thus a delimitation of high and low economic cycles is only valid to a limited extent. Hence the **follow-up waves in terms of months as t ∈ {1,2,3,4,7,12,17}** . The economic cycle is assumed to “start” two months after baseline survey (wave 2) with low economic activity with a trend to increasing profits. Three months after baseline survey (wave 3 & 4) the economic activity somewhat stagnates seven months after baseline survey (wave 5) and reaches maximum profits and is determined as period of high economic activity. 12 months after baseline survey (wave 6) the economic activity dropped again which suggests this to be a low economic period. In wave 7 (17 months after baseline survey) is determined as a period with rising economic activity in the next economic cycle (See Figure 1).The **sample data of Dandora’s Micro-Enterprises is a suitable approximation of the economic activity of the whole country measured by the GDP of Kenya**.

<i class="far fa-sticky-note"></i> **Note:** Evidence and further explanations are delivered in the full paper!
  {: .notice--info}

{: .text-justify}
Finally, we got an introduction to the topic and got familiar with the business as well as economic characteristics. It is important to understand the dynamics of the treatment effects in the business sectors over time. Therefore the analysis in part II focusses on delimitations within treatment groups as well as business sectors.

{: .text-justify}
Catch up with ANCOVA (ANalysis of COVAriances) and Logistic Regression that deliver the **results in [part II](https://lhagels.github.io/Part-II-Development-Opportunities-of-Micro-Entrepreneurs-Evidence-from-Kenya/)** or skip this part to directly enter the **discussion and conclusion section in [part III](https://lhagels.github.io/Part-III-Development-Opportunities-of-Micro-Entrepreneurs-Evidence-from-Kenya/)** where I discuss possible policy implications.
