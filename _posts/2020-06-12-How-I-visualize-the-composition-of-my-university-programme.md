---
title: "How I Visualize the Composition of my University Programme in Python"
tags:
  - Web Scraping
  - Data Cleansing
  - Data Manipulation
  - Data Mining
  - Data Visualization
  - Selenium
  - BeautifulSoup
  - Python

date: June 12, 2020
header:
  teaser: /assets/images/thumbnails/squarify_treemap.png
excerpt: "A short introduction in Web Scraping and Data Visualization in Python"
toc: true
toc_sticky: false
toc_label: "Let's start from scratch!"
---
{: .text-justify}
In this blog post I am going to explain the **methodology of a data science project** with simplified data starting at **Web Scraping** a data table from a particular web page, over **data cleansing and manipulating** thus we end up with a user-friendly **visualization** of our data.

When performing the analysis I often only present the first few rows of the data to save some space. Furthermore, you may read the comments in the coding-section (indicated with **#**) to get a better understanding of the written code.

The final result will be an **interactive visual representation** of the B.Sc. Economics in Konstanz at the end of this post.

<i class="far fa-sticky-note"></i> **Note:** Unless you are a student of the University of Konstanz the first steps may highly vary. In this case you should examine the website with the web-developer tool first to find the relevant elements. If you need further consultation, do not hesitate to contact me!
  {: .notice--info}

# 1. Scrape some Data from the Web
{: .text-justify}
There are several libraries for web scraping available. The most common used in Python are **BeautifulSoup and Selenium** because they are **intuitive to apply**.  
Due to complexity issues of the exam table we want to scrape in this session I use a combination of both in this project. **Selenium unveils** the required information while **Beautifulsoup extracts** the HTML-code in this case. We urgently have to unveil the full information with help of Selenium! Otherwise we are not able to scrape the full table!

## 1.1 Set-up the Webdriver for your Browser
```python
  #pip install bs4 selenium
  from selenium import webdriver

  options = webdriver.ChromeOptions()
  options.add_argument('--ignore-certificate-errors')
  options.add_argument('--no-sandbox')
  options.add_argument("--disable-setuid-sandbox")
```

## 1.2 Login on the Website and Navigate to Exam Results
```python
  driver = webdriver.Chrome("...path to you chromedriver.exe...", options=options)#

  #open the login page
  driver.get('...Link to login page...')

  #find the elements that are needed for the login
  username = driver.find_element_by_id("...id of element...")
  password = driver.find_element_by_id("...id of element...")

  #enter login details
  username.send_keys("...Username...")
  password.send_keys("...Password...")
  driver.find_element_by_name("...Element name of Login Button...").click()

  #navigate to the exam results
  #(in this case we deal with an collapsed table thus we need to manually expand the table with Selenium)
  driver.get('...Link of the page with your exam results...')
  #find the element to expand the whole table
  driver.find_element_by_id("...id of element that expands the whole table").click()
```

{: .text-justify}
First, the algorithm opens the login page and continues entering the login details and "clicking" submit. Second, it navigates to the page that contains exam results and **expands all the root nodes to unveil all modules**. At this point we are able to **extract the desired HTML-code** from the web page.  
The required information about the web page's elements can be found by the **web developer tool** of your browser. (_Check out the web for further explanation_)  

![Selenium Worksteps](/assets/videos/selenium-login-and-toggle.gif)

## 1.3 Find the Table of interest and extract the HTML Code with BeautifulSoup
{: .text-justify}
In the next step we need to **extract the unveiled information**. For this purpose we make use of the **BeautifulSoup** library to receive the HTML-code of the results table that can be **found by its xpath**. The xpath is also provided by the web developer tool of your browser.
The extracted HTML-code that contains unwrapped information is assigned to the variable _soup_expanded_.

```python
  from bs4 import BeautifulSoup

  #find the table by Xpath
  tables = driver.find_elements_by_xpath("html/body/div/div/div/div/div/form/div/div/div/div/div/div/fieldset/div/table/tbody/tr/td/div/table/tbody/tr")

  #get the HTML-code of the currently "saved" page in your driver with BeautifulSoup
  soup_expanded = BeautifulSoup(driver.page_source)
```

## 1.4 Create a Pandas Dataframe
{: .text-justify}
At this point we scraped the data but **cannot process the information so far** since it is still in HTML-format. In order to perform data cleansing and manipulation with the goal to visualize data, we **assign the HTML-code to a Pandas DataFrame**.

```python
  import pandas as pd

  #Beautiful Soup grabs the HTML table on the page
  table = soup_expanded.find_all("table")

  #Giving the HTML table to pandas to put in a dataframe object
  df = pd.read_html(str(table), header=1)[0]

  print("The dataframe has",df.shape[0], "rows and", df.shape[1],"columns")
  print(df["Bonus"].sum(), "of 180 credits")
```

{: .text-justify}
Finally, we receive a **cross-sectional data** table that provides **observations in rows** and different **variables in the columns** (e.g. Grade, Credits, Attempt, Module_nr). However, since we have some columns that are a bit messy, we are going to tidy them up! The dataframe has a shape of **74x25 [rows x columns]**. I do not remember that I participated in that many exams! There may be a bug!!
Further, when adding up all credits included in the data, we receive unrealistic **1193 credit points**.

![df.head()](/assets/images/posts/25_05_20/1_4_df.png)

# 2. Wrangle Dangle Ding Dong!
## 2.1 Clean Data is good Data
{: .text-justify}
Our dataset is small thus it seems quite easy to be structured.
We face several **problems**:
  1. Many **variables repeat information**  
     (e.g. multiple columns with the same information)
  2. **German** column names  
     (since we want to communicate our results to an **English audience**)
  3. Some modules cannot be distinguished from another
     (caused by the root node style of the original web table)

{: .text-justify}
The following code snippet solves the above mentioned problems. This process is **not universal** since the provided conditions may vary depending on whether a student did an internship or not. Furthermore, some want rather observe only positive results (Grade 1.0 up to 4.0) than every module that has ever been attended.

```python
  #inspect columns and select the data
  print(df.columns)
  df_table = df[["Titel.11", "Bewertung", "Bonus", "Versuch", "Nummer"]]

  #rename the columns
  df_table.columns = ["Module", "Grade", "Credits", "Attempt", "Module_nr"]

  #select unique modules of the whole table
  df_modules = df_table[((df_table["Module_nr"].str.len() > 4)|(df_table["Module_nr"].str.len() == 2)|(df_table["Credits"] == 23)|(df_table["Module"] == "Bachelorarbeit")) &
                      ((df_table["Credits"] <= 9)|((df_table["Credits"] == 23)&df_table["Grade"].isna() == True)) & #maximale Punktzahl pro Modul ist 9 oder Praktikum(23ECTS)
                      ((df_table["Grade"] < 5)|df_table["Grade"].isna() == True) #nur positive Versuche
                     ]
  display(df_modules)

  #check if the DataFrame includes all credits
  print(df_modules["Credits"].sum(), "of 180 credits included in the data")
```
![df_clean.head()](/assets/images/posts/25_05_20/2_1_df.png)  
![df_shape](/assets/images/posts/25_05_20/2_1_after.png)

{: .text-justify}
The shape of our data **reduced from 74x25 to 26x5** [rows x columns]. In addition the number of credit points which **decreased from 1193 to 180** credits.  

{: .text-justify}
This seems to be much **more reliable!**

## 2.2 Rename Modules that cannot be distinguished from others
{: .text-justify}
During **doublechecking** our data after the recent changes, we notice three modules called "Anerkennung" that cannot be distinguished from each other. These are modules I have attended abroad. We should **rename these modules manually** since the scraping process was not able to receive the correct names in the automated process.

```python
  #Manipulate data in column "Module" since "Anerkennung" is not the modules name (course taken abroad and credited in Germany)
  df_modules["Module"][df_modules["Module_nr"] == "22"] = "Econometrics"
  df_modules["Module"][df_modules["Module_nr"] == "21"] = "Financial Markets & Institutions"
  df_modules["Module"][df_modules["Module_nr"] == "20"] = "Operations & Supply Chain Management"

  #check if the modules are correctly renamed
  df_modules[df_modules["Module_nr"].str.len() == 2]
```
![df_unique_modules.head()](/assets/images/posts/25_05_20/2_2_abroad_correction.png)

## 2.3 Provide English Translations of the Modules
{: .text-justify}
We create a first list of the Module column and **zip it pairwise** with a second list including the English translations. The resulting object is a **German(keys)-English(values)** dictionary. Finally, we assign the English module names to a new column.

```python
  #create a list of the "Module" column
  modules_de = list(df_modules["Module"].values)

  #Create a list with translations
  modules_eng = ["Mathematics 1",
                 "Mathematics 2",
                 "Statistics 1",
                 "Statistics 2",
                 "Introduction Economics",
                 "Microeconomics",
                 "Macroeconomics",
                 "Public Economics",
                 "Public Finance",
                 "Accounting 1",
                 "Organizational Economics",
                 "Marketing 1",
                 "Accounting 2",
                 "Corporate Finance",
                 "Private Law",
                 "Investments & Finance",
                 "Marketing 2",
                 "Econometrics",
                 "Financial Markets & Inst.",
                 "Operations & SC Management",
                 "Social Psychology",
                 "Internship",
                 "IT-Module",
                 "Negotiations",
                 "Bachelor-Seminar",
                 "Bachelor-Thesis"
                 ]
  #build up a dictionary of german-english pairs
  modules_dict = dict(zip(modules_de, modules_eng))

  #create a new column that contains english module names
  df_modules["Module_eng"] = df_modules["Module"]
  df_modules["Module_eng"].replace(modules_dict, inplace=True)
```

## 2.4 Sections Matter!
{: .text-justify}
It may be of interest to **classify the modules** in a certain study section. This offers deeper **insights into the curriculum**. Therefore, we write a dictionary with study sections as keys and lists of modules as values. **Each module list (value) is assigned to a certain section (key)**.

```python
  import numpy as np

  #define a dictionary that assigns each section its modules
  section_module_dict = {"Statistics": ["Statistics 1",
                                        "Statistics 2",
                                        "Econometrics"
                                       ],
                         "Mathematics": ["Mathematics 1",
                                         "Mathematics 2"
                                        ],
                         "Business Studies": ["Accounting 1",
                                              "Accounting 2",
                                              "Marketing 1",
                                              "Marketing 2",
                                              "Investments & Finance",
                                              "Corporate Finance",
                                              "Organizational Economics",
                                              "Operations & SC Management"
                                              ],
                         "Economics": ["Introduction Economics",
                                       "Microeconomics",
                                       "Macroeconomics",
                                       "Public Economics",
                                       "Public Finance",
                                       "Financial Markets & Inst.",
                                       "Bachelor-Seminar",
                                       "Bachelor-Thesis"
                                      ],
                        "Law": ["Private Law"],
                        "Psychology": ["Social Psychology"],
                        "Internship": ["Internship",
                                       "IT-Module",
                                       "Negotiations"
                                      ]
                         }

  #search for tags in module names
  df_modules["Section"] = "none"
  for i in range(len(section_module_dict.keys())):
    df_modules["Section"] = np.where(df_modules["Module_eng"].str.contains('|'.join(list(section_module_dict.values())[i])),
                                     list(section_module_dict)[i],
                                     df_modules["Section"]
                                    )
```
{: .text-justify}
The "for-loop" searches for strings that are included in the values (lists) for each index of the dictionary. **If a certain module is included in the list of an index, the key of this index is assigned to a column "Sections"**. Otherwise, the column does not change at all.

## 2.5 Quantitative or Qualitative?
{: .text-justify}
Finally, universities distinguish between **qualitative and quantitative modules**. Since there is no identifier which type a module is assigned to we have to set up a list manually. It is important to work in the respective order of the modules in the dataset to obtain proper allocations. **Doublechecking is mandatory** to avoid typing mistakes!

```python
  #create a list that assign the course type respectively to the module column
  type_list = ["quantitative",
               "quantitative",
               "quantitative",
               "quantitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "quantitative",
               "quantitative",
               "quantitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "qualitative",
               "quantitative",
               "quantitative",
               ]

  #check the lengths of both lists
  print(len(modules_eng), len(type_list))

  #create a column "type"and replace values by dict
  mod_type_dict = dict(zip(modules_eng,type_list))
  df_modules["type"] = df_modules["Module_eng"]
  df_modules["type"].replace(mod_type_dict, inplace=True)

  #double check if every type is assigned correctly
  display(df_modules.head())
  print("The dataframe has",df_modules.shape[0], "rows and", df_modules.shape[1],"columns")
```

After the **previous data mining** the dataframe has a **shape of 26 x 8** [rows x columns]:

![df_unique_modules.head()](/assets/images/posts/25_05_20/2_5_translate_sections_type.png)

# 3. Data Exploration / Data Mining
## 3.1 What is the Composition of the B.Sc. Economics in Konstanz?
{: .text-justify}
The most important information to answer this question is the **proportion of each study section** relative to the total credit points.
The following code snippet prints the **composition of the programme** as follows:  

```python
  #create a list of unique values
  sections = list(df_modules["Section"].sort_values().unique())

  #check for each value in list
  for i in sections:
    value = sum(df_modules["Credits"][df_modules["Section"] == i])
    percent = round(sum((df_modules["Credits"][df_modules["Section"] == i]) / sum(df_modules["Credits"]))*100, 1)
    print(value, "(=", percent, "%)", "credits in", i)
```
Output of the previous code:
* 43.5 (= 24.2%) credits in Business Studies
* 58.5 (= 32.5%) credits in Economics
* 29.0 (= 16.1%) credits in Internship
* 3.0 (= 1.7%) credits in Law
* 18.0 (= 10.0%) credits in Mathematics
* 8.0 (= 4.4%) credits in Psychology
* 20.0 (= 11.1%) credits in Statistics

### 3.1.1 Seaborn's Barplot as visual Communication Tool
{: .text-justify}
As Data Scientists we are interested to **communicate user-friendly**. Therefore I decided to visualize the results in a barplot first since it **gives us very quick impressions** of the data. Moreover, Seaborn as well as Matplotlib deliver well formatted plots that can be drawn easily as well as intuitively.

```python
  import seaborn as sns
  import matplotlib.pyplot as plt

  fig, ax = plt.subplots(figsize=(16,8))
  sns.barplot(data=df_modules, y="Credits", x="Section", alpha=0.8, ci=None, estimator=sum, ax=ax)
  plt.title("Total Credits by Study Sections", fontsize=25)
  plt.ylabel("#Credits", fontsize=20)
  plt.xlabel("", fontsize=20)
  plt.yticks(size=12)
  plt.xticks(size=12)
  plt.show()
```

The barplot looks as follows:
![df_unique_modules.head()](/assets/images/posts/25_05_20/3_1_barplot_credits_sections.png)

### 3.1.2 Squarify's Treemap as visual Communication Tool
{: .text-justify}
First, we introduce two lists. _credits_sec_sizes_ contains the absolute credit points per study section, whereas _credits_sec_labels_ serve as label data and contains the same information plus the name and relative fraction of each section.

```python
  #empty list
  credits_sec_sizes = []
  #append list by credits of each section --> serve as sizes!
  for i in sections:
    value = sum(df_modules["Credits"][df_modules["Section"] == i])
  credits_sec_sizes.append(value)

  #empty list
  credits_sec_labels = []
  #append list by credits with each section --> serve as labels!
  for i in sections:
    value = sum(df_modules["Credits"][df_modules["Section"] == i])
    percent = round(sum((df_modules["Credits"][df_modules["Section"] == i]) / sum(df_modules["Credits"]))*100, 1)
  credits_sec_labels.append(str(i) +": \n"+str(value)+ " credits" +"\n" +"("+str(percent) +"%"+")" )
```
{: .text-justify}
Next, we plot our data using the **Squarify** library and receive the following outcome:

```python
  import squarify #pip install squarify
  import matplotlib.pyplot as plt

  # Get figure and font size
  plt.rc('font', size=40)
  fig = plt.figure(figsize=(50, 25))
  # Plotting
  squarify.plot(sizes=credits_sec_sizes, label=credits_sec_labels, alpha=0.4, color=["red","green","blue", "grey","yellow","pink","orange"])
  # Removing Axis
  plt.axis('off')
  # Title
  plt.title("Study Sections B.Sc. Economics in Konstanz",fontdict={"fontsize": 60, "verticalalignment": "top"})
  plt.savefig("...system path...")
```

{% include Squarify_Treemap.html %}

## 3.2 Create an interactive Treemap of the Degree Composition

Finally, we go one step further and **include the type of modules** in our plot. First we calculate the fraction of both **quantitative and qualitative modules**:

```python
  #create a list of unique values
  type = df_modules["type"].sort_values().unique()

  #check for each value in list
  for i in type:
    value = sum(df_modules["Credits"][df_modules["type"] == i])
    percent = round(sum((df_modules["Credits"][df_modules["type"] == i]) / sum(df_modules["Credits"]))*100, 1)
    print(value, "(=", percent, "%)", "credits in", i, "modules")
```

{: .text-justify}
The types have a division of approximately **2/3 qualitative** and **1/3 quantitative** modules:
![df_unique_modules.head()](/assets/images/posts/25_05_20/3_2_division_types.png)

{: .text-justify}
It may be of interest for the audience to get as much information as possible from an image. Therefore we **expand the treemap by the module types**.
Since Squarify does not provide easy access to this type of set-up we switch to the **Plotly** library:

```python
  import plotly.express as px
  #copy of our DataFrame
  df_plotly = df_modules

  #introduce a new column as single root node
  df_plotly["Programme"] = "B.Sc. Economics" # in order to have a single root node
  #Capitalize the type values
  df_plotly["type"] = df_plotly["type"].str.capitalize()

  #plot the treemap
  fig = px.treemap(df_plotly, path=["Programme","type","Module_eng"], values="Credits",
               color="Credits", hover_data=["Credits"],
               color_continuous_scale="Blues",
               color_continuous_midpoint=np.average(df_plotly["Credits"], weights=df_plotly["Credits"]),
              )
  fig.update_layout(showlegend=False)
  fig.show()
```

{: .text-justify}
We yield the desired **interactive data visualization** that includes the modules as well as their types and sections:
![ects_by_module](/assets/images/posts/25_05_20/3_2_ects_by_modules.png)
{% include posts_25.05.20_3_2_ects_by_modules.html %}

Beyond this, you can think of including a further layer that reflects your grades in the treemap or plot your grades over time to explore whether your results are constant or vary over time.

Just **feel free** and **be creative**!

**ENJOY :)**
