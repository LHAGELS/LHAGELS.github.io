---
title: "Visualizing University Modules in Python"
tags:
  - Web Scraping
  - Selenium
  - BeautifulSoup
  - Data Visualization
  - Python

date: May 25, 2020
header:
  teaser: /assets/images/thumbnails/joel-filipe-thumb-800.jpg
excerpt: "A short introduction in Web Scraping and Data Visualization in Python"
---

## Let's scrape some data from the web
There are several libraries for web scraping available. The most common in python are BeautifulSoup and Selenium. Both have their advantages and disadvantages but are intuitively and easy to apply.
Due to complexity issues of the exam table I use a combination of both in this project.

### 1.1 Set-up the webdriver for your browser
```Python
    #pip install bs4 selenium
    from selenium import webdriver

    options = webdriver.ChromeOptions()
    options.add_argument('--ignore-certificate-errors')
    options.add_argument('--no-sandbox')
    options.add_argument("--disable-setuid-sandbox")
```

### 1.2 Login on the website and navigate to the Exam Results
```Python
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

    #navigate to the exam results (in this case we deal with an collapsed table thus we need to manually expand the table with Selenium)
    driver.get('...Link of the page with your exam results...')
    #find the element to expand the whole table
    driver.find_element_by_id("...id of element that expands the whole table").click()
```

First, the algorithm opens the login page to enter the login details and "click" submit. Second, it navigates to the page that contains exam results and expands all the root nodes to unveil all modules.

### 1.3 Find the table of interest and get the HTML code with BeautifulSoup
In the next step I need to extract the unveiled information. For this purpose I use BeautifulSoup to receive the HTML-code of the result table that is found by the xpath. The xpath is provided by the chrome developer tool. (Check out the web for further explanation)
Soup_expanded provides the HTML-code of the exam results.

```Python
    from bs4 import BeautifulSoup

    #find the table by Xpath
    tables = driver.find_elements_by_xpath("html/body/div/div/div/div/div/form/div/div/div/div/div/div/fieldset/div/table/tbody/tr/td/div/table/tbody/tr")

    #get the HTML-code of the currently "saved" page in your driver with BeautifulSoup
    soup_expanded = BeautifulSoup(driver.page_source)
```

### 1.4 Create a Pandas Dataframe
At this point we scraped the data but cannot process the information we received so far. In order to clean, manipulate and visualize the data we assign the HTML-code to a Pandas DataFrame.

```Python
    import pandas as pd

    #Beautiful Soup grabs the HTML table on the page
    table = soup_expanded.find_all("table")

    #Giving the HTML table to pandas to put in a dataframe object
    df = pd.read_html(str(table), header=1)[0]
```

Finally, we received a cross-sectional data table that provides observations in rows and different variables (e.g. Grade, Credits, Attempt, Module_nr and much more needless variables)

## 2. Wrangle Dangle Ding Dong!

### 2.1 Clean Data is good Data
Since our dataset is quite small it seems relatively easy to structure our data.
We face several problems:
  * Many variables that do not make sense
  * German column names
  * Modules are not unique caused to the root node style of the original web table

The following code snippet solves the above mentioned problems. This step is not universal for all students since the provided conditions may vary depending on whether a student did an internship or not. Furthermore, some want to observe rather passed modules than every module that has ever been attended.

```Python

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

### 2.2 Rename Modules that cannot be distinguished from Others
During the double check of the most recent changes we notice 3 modules "Anerkennung" that cannot be distinguished from each other. These are modules I have attended abroad and should be renamed since the scraping process was not able to receive the correct names.

```Python
    #Manipulate data in column "Module" since "Anerkennung" is not the modules name (course taken abroad and credited in germany)
    df_modules["Module"][df_modules["Module_nr"] == "22"] = "Econometrics"
    df_modules["Module"][df_modules["Module_nr"] == "21"] = "Financial Markets & Institutions"
    df_modules["Module"][df_modules["Module_nr"] == "20"] = "Operations & Supply Chain Management"

    #check if the modules are correctly renamed
    df_modules[df_modules["Module_nr"].str.len() == 2]
```

### 2.3 Provide English Translations of the Modules
We create a first list of the Module column and zip it (pairwise) with a second list that includes english translations. The resulting object is a german(keys) - english(values) dictionary. Finally, we assign the english names to a new column.

```Python
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

### 2.4 Sections Matter!
It may be of interest which module is assigned to as certain study section for a brief insight into the curriculum. Therefore, I write a dictionary with study sections as keys and lists of modules as values. Each list (value) is assigned to a certain key.

```Python
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
                                         "Negotiations"                                  ]
                          }

    #search for tags in module names
    df_modules["Section"] = "none"
    for i in range(len(section_module_dict.keys())):
      df_modules["Section"] = np.where(df_modules["Module_eng"].str.contains('|'.join(list(section_module_dict.values())[i])),list(section_module_dict)[i], df_modules["Section"])

    #check if all sections are assigned properly
    df_modules
```

The "for-loop" searches for strings that are included in the values (lists) for each index of the dictionary. If a certain module is included in the list of an index, the key of this index is assigned to a column "Sections". If the value is not in this list, the column does not change at all.

### 2.5 Quantitative or Qualitative?
Finally, universities distinguish between qualitative and quantitative modules. Since there is no identifier which type a module is assigned we have to set up a list manually, again. It is important to work in the respective order of the modules in the dataset to obtain proper allocations. Doublechecking is mandatory!

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

    #create a dictionary and assign a new column that contains the course type
    mod_type_dict = dict(zip(modules_eng,type_list))
    df_modules["type"] = df_modules["Module_eng"]
    df_modules["type"].replace(mod_type_dict, inplace=True)

    #double check if every type is assigned correctly
    df_modules
```

## 3. Data Exploration / Data Mining

### 1. How many Credits has each Section?
