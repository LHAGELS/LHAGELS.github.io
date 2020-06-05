---
title: "PART I Getting familiar with the Topic - Development Opportunities of Micro-Entrepreneurs: Evidence from Kenya""
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

# 1. Introduction
<i class="far fa-sticky-note"></i> **Note:** If you are interested in the full depth of this analysis as well as recent research visit the [portfolio](/portfolio/) section to view the full bachelor thesis including the Stata code.
  {: .notice--info}

_"Understanding the reason that these firms  have low profits and why they generate so little employment is key for designing policy to improve the welfare of the urban poor.”_ (Brooks, Donovan & Johnson 2018: 197)
This paper aims to shed light on differences between business sectors to investigate target audiences for specific development approaches. Furthermore it elaborates whether micro-enterprises of different business sectors vary in their growth probabilities and profits while receiving either class training or mentoring. Beyond it explains possible differences on the macroeconomic level. Therefore the research question is specified as follows:

Research Question:
_To what extent do micro-enterprises of individual business sectors in developing countries benefit differently if they receive either class training or mentoring?_

To answer the research question an investigation of the gathered data during the development study conducted by Brooks et al. (2018) has been made. At the end of 2014, they assigned female micro-entrepreneurs from Kenya to groups that receive class training, mentoring or neither to elaborate differences in profits over the next 17 months after the treatment.

# 2. Literature Review
Sohns and Revilla Diez (2018) split micro-enterprises into two motivational groups: opportunity-driven  and necessity-driven . Fulton et al. (2012) determined female street-food vendors in the informal sector as necessity-driven micro-entrepreneurs who lack education and possibilities to work for wage. Tinker (2003) mentioned polygamous  men as one reason for women to be self-employed in the informal sector whereby the focus stays on feeding children instead of achieving business growth. In addition, four-fifth of street-food micro-enterprises are owned by women. Roy and Wheeler (2006) state the street-food and retail sector as highly competitive and therefore highlight unique features as salient factor to separate the own business from competitors. However since most micro-enterprises lack in money supply local street-food stalls often try to minimise risk by supplying the same products as other local vendors, though such behaviour leads to even higher competition.
Acho-Chi (2002), Tinker (2003) and Groota et al. (2017) provide common problems like a poorly developed infrastructure that cannot ensure access to clean water and electricity as well as the appropriate storage of foods. This has negative effects in terms of hygiene, germs and food quality. Especially Acho-Chi (2002) emphasizes 32% of customers choose their food stall due to its quality.
Klapper and Richmond (2011) have shown evidence of lower exit rates of micro-enterprises in the service sector compared to other sectors in Africa. Further the fraction of employment in the service sector in Africa is estimated to be 37%.
A series of studies also highlight the importance of non-financial factors like motivation and local expertise. Bruhn and Zia (2013) indicated that training programs particularly provide motivation to female businesses. More recently, Lafortune et al. (2018) studied the impact of role models on the effectiveness of training programs in Chile and found significant non-financial barriers like motivational factors, faced by female businesses who lack experience.
One possibility to transfer knowledge to entrepreneurs is given by assigning them to business classes. Bruhn and Zia (2013) have shown improved business practices combined with higher sales. In contrast, De Mel et al. (2014) measured the impact of the most common business training  on female entrepreneurs in Sri Lanka. In fact, a change in business practices was noticed but they did not translate into higher profits or improved business performances. The latter goes in line with results of Brooks et al. (2018) who found evidence of constant profits despite changes in business practices. Additionally however, De Mel et al. (2014) highlighted limited impacts on business practices of participants who already owned a business for several years. McKenzie and Puerto (2017) conducted a market-level experiment in Kenya and detected increased profits for female entrepreneurs three years after the participation in a business training. Further they mention a growing market instead of spill-over effects, e.g. stealing sales from competitors within the market, as a reason for the latter effect. Similar results are shown by Brooks et al (2018) who found lower costs as driving factor for higher profits, but not spill-over effects. Higuchi et al. (2019) also experienced an increased business performance coupled with improved business practices three years after assigning small and micro-enterprises to a training program in Tanzania that features the Kaizen  approach.
McKenzie and Woodruff (2017) determined a positive correlation between the quality in business practices and growth in business performances such as firm survival rates. Therefore they criticise current business programs and their modest impact on business practices. Moreover they claim for more intensive business programs as well as a specified target audience to raise effects on business performance.
Another possibility to transfer information might be achieved by introducing micro-entrepreneurs to mentors. Dennis et al. (2003) assigned mentors to small- and medium-sized retailers in London and explored significant increases in sales. Cai and Szeidl (2017) conducted inter-firm meetings between managers from 2,820 firms and found permanent increased revenues in the treatment group of monthly meetings. This effect is enhanced by networking activities during meetings since managers provide business relevant information if they are non-competitors (peer-to-peer). In contrast, Brooks et al. (2018) simultaneously indicated a flow benefit provided by mentors to their mentees. The effect diminished once matches started to dissolve. Further they indicated the major role of local expertise on business performance provided by mentors.
Lastly, McKenzie and Woodruff (2013) evaluated previous studies in terms of set-up and measurement methods of experiments. They demanded a raise of sample sizes from 100-500 (previous studies) up to several thousands. Further an extension of time periods in studies might be reasonable to interpret treatment effects over time in the right manner. Heterogeneity of the data also seems to be a problem. Researchers should be able to divide the sample into subsets with identical characteristics to create more homogeneity and further examine more detailed effects.

# 3. Descriptive Statistics
## 3.1 Micro-Enterprises and Treatment Characteristics
![Descriptive Statistics of Micro-Enterprises among Business Sectors](/assets/images/posts/03_06_20/Table_1.png)
The sample includes 368 inexperienced, female business owners who participated in follow-up surveys that generated time series data of variables that may correlate with business performance. The field experiment was conducted from end of 2014 to April 2016 while the treatment period lasted one month in November 2014. For this micro-enterprises were randomly assigned into three groups.
First mentorship, within this group each micro-entrepreneur is paired with an experienced mentor, who has been operating in the same business sector for at least 5 years or is older than 40 years. Second class training, formal business classes are provided to micro-entrepreneurs to improve knowledge in business practices. Third, the control group who receives neither.
Table 1 reproduces baseline profits of micro-enterprises in each business sector, grouped by their assigned treatment group. The pooled values of each group is a simple first indicator to get familiar with the values. Thereby the control group achieves the highest average baseline profit but has also the highest standard deviation. Another important value is the number of observations per group that is nearly similar due to random distribution.
<i class="far fa-sticky-note"></i> **Note:** In fact, the other values are very interesting but go beyond the summary at this point. Recall, check the full paper for further insights!
  {: .notice--info}

## 3.2 Cycling Profits among Business Sectors
![Micro-Enterprise Profits among Business Sectors](/assets/images/posts/03_06_20/Figure_1.png)
In the following section the average profits of each business sector are described to show off some evidence of economic cycles in Dandora, Kenya. Unfortunately the observed period only has a length of 17 months thus a delimitation of high and low economic cycles is only valid to a limited extent. Hence the follow-up waves in terms of months as "t ∈ {1,2,3,4,7,12,17}" . The economic cycle is assumed to “start” two months after baseline survey (wave 2) with low economic activity with increases in profits. Three months after baseline survey (wave 3 & 4) the economic activity somewhat stagnates seven months after baseline survey (wave 5) and reaches maximum profits and is determined as period of high economic activity. 12 months after baseline survey (wave 6) the economic activity dropped again which suggests this to be a low economic period. In wave 7 (17 months after baseline survey) is determined as a period with rising economic activity in the next economic cycle (See Figure 1).It is important to understand the dynamics of the treatments effects in the business sectors over time. The sample data of Dandora’s Micro-Enterprises is a suitable approximation of the economics activity of the whole country measured by the GDP of Kenya.

<i class="far fa-sticky-note"></i> **Note:** Evidence and further explanations are delivered in the full view!
  {: .notice--info}

Finally, we got an introduction to the topic and got familiar with the business as well as economic characteristics.

Catch up with ANCOVA (ANalysis of COVAriances) and Logistic Regression that delivered the **results in [PART II](.......)** or skip this part to directly enter the **discussion and conclusion section in [PART III](.......)**.
