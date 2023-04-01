---
title: "DataCleaner Tool Review"
date: 2023-04-01
---

## Intro:
This blog is a review of the [DataCleaner](https://datacleaner.github.io/) tool and its possible uses for MLOps. I found this tool to be easy to install and fairly helpful for creating a data pipeline. While I only used the GUI, there is also a CLI which could help in the automation of certain pipelines. Overall, I would recommend this tool for people who are trying to parse unstructured data, ensure columns meet specific criteria, and analyze trends. 

## How to Install:
Installing this tool was very easy. The steps are listed below:
1. Ensure your system has JRE installed
2. Download the latest version of DataCleaner from this [link](https://datacleaner.github.io/downloads.)
3. Unzip the download
4. Navigate into the folder that contains all of the java files
5. Launch DataCleaner by runnning the command <code>java -jar .\DataCleaner.jar</code>

Note that the first time launching DataCleaner took a fairly long time (on the order of minutes) as it had to do some installation. After the first time, it loads quicker (on the order of seconds) using the same command.

Here is an image below showing a successful launch of DataCleaner:
![image of commands running in a terminal launching DataCleaner](https://github.com/srutherford2000/blog_post_on_DataCleaner/blob/main/images/opening_datacleaner.PNG)


## What Problem this Tool Addresses:
As the name suggests, this tool aims to solve problems that are related to data. Mainly it serves as a way to clean, analyze, visualize, and transform data through the creation of jobs which run a series of steps. When you first open DataCleaner, it allows you to choose a datastore. This datastore can be in the form of a .csv, .xls, .mdb, .xml, .json, and other formats which allows large flexibility in the type of data that can be imported. Once you've imported your datastore(s), you can then start to create pipelines using the 4 built in libraries: Transform, Improve, Analyze, and Write. 

The Transform library contains functions which run on every row and allow you to convert old data into some new form. I found the "String contains filter", "Validate with String Pattern", and "Regex parser" to be super powerful for splitting and ensuring this new data met specific formatting criteria. 

The Improve library contains functions which run on every row and attempt to augment data. It contains functions such as http request, synonym lookup, country standardizer and more. The function HTTP request was really interesting to me, as it allowed API calls to be made for every row based on specific values of that row. While I did not find myself using any of these functions in the upcoming demo, I still see tremendous value in being able to augment data in a simplified pipeline.

The Analyze library contains functions that run on the entire data set and try to help visualize trends. Some of the most useful functions I found when trying out the tool were "Unique key check", "Value distribution", and "Pattern finder". Value distribution allows you to get the frequency of certain values in any specified column, which is useful for determining which values were the most/least common in a specific row. Pattern finder is also exceptionally helpful in understanding what the format is for specific columns as it runs on any specified column and returns a list of patters(in an almost regular expression sort of way) and their frequency in the data. If you were given a dataset you knew nothing about, running it through pattern finder would be useful in determining how to start parsing the data.

Finally, the write library gives you the option of writing these new datastores or metrics to csvs, excels, staging tables, and more. This is essential if you are changing the format of data, as you need to be able to store the changes.

Overall, this tool aims to solve many of the problems faced in the data pipeline. It solves problems in cleaning, analyzing, and augmenting by providing tools which allow for the creation of jobs that can be run in an automated form. This is useful for anyone working on data engineering in MLOps.

## Example Tool Usage for a Movie Recommendation System:

## Strengths and Limitations of DataCleaner:

## Citations:
https://datacleaner.github.io/
https://datacleaner.github.io/docs/5.4.0/components/index.html
https://github.com/skills/github-pages
