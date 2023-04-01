---
title: "DataCleaner Tool Review"
date: 2023-04-01
---

## Intro:
This blog is a review of the [DataCleaner](https://datacleaner.github.io/) tool and its possible uses for MLOps. I found this tool to be easy to install and fairly helpful for creating a data pipeline. While I only used the GUI, there is also a CLI which could help in the automation of certain pipelines. Overall, I would recommend this tool for people who are trying to parse unstructured data, ensure columns meet specific criteria, and analyze trends. 

## How to Install:
Installing this tool was extremely easy. The steps are listed below:
1. Ensure your system has JRE installed
2. Download the latest version of DataCleaner from this [link](https://datacleaner.github.io/downloads.)
3. Unzip the download
4. Navigate into the folder that contains all of the java files
5. Launch DataCleaner by running the command <code>java -jar .\DataCleaner.jar</code>

Note that the first time launching DataCleaner took a fairly long time (on the order of minutes) as it had to do some installation. After the first time, it loads quicker (on the order of seconds) using the same command.

Here is an image below showing a successful launch of DataCleaner:
![image of commands running in a terminal launching DataCleaner](https://github.com/srutherford2000/blog_post_on_DataCleaner/blob/eb89426983af92d3afb6576fe363f62f9df5ea01/images/opening_datacleaner2.PNG?raw=true)


## What Problem Does This Tool Address:
As the name suggests, this tool aims to solve problems that are related to data. Mainly it serves as a way to clean, analyze, visualize, and transform data through the creation of jobs which run a series of steps. When you first open DataCleaner, it allows you to choose a datastore. This datastore can be in the form of a .csv, .xls, .mdb, .xml, .json, and other formats which allows large flexibility in the type of data that can be imported. Once you've imported your datastore(s), you can then start to create pipelines using the 4 built in libraries: Transform, Improve, Analyze, and Write. 

The Transform library contains functions which run on every row and allow you to convert old data into some new form. I found the "String contains filter", "Validate with String Pattern", and "Regex parser" to be super powerful for splitting and ensuring this new data met specific formatting criteria. 

The Improve library contains functions which run on every row and attempt to augment data. It contains functions such as http request, synonym lookup, country standardizer and more. The function HTTP request was really interesting to me, as it allowed API calls to be made for every row based on specific values of that row. While I did not find myself using any of these functions in the upcoming demo, I still see tremendous value in being able to augment data in a simplified pipeline.

The Analyze library contains functions that run on the entire data set and try to help visualize trends. Some of the most useful functions I found when trying out the tool were "Unique key check", "Value distribution", and "Pattern finder". Value distribution allows you to get the frequency of certain values in any specified column, which is useful for determining which values were the most/least common in a specific row. Pattern finder is also exceptionally helpful in understanding what the format is for specific columns as it runs on any specified column and returns a list of patters(in an almost regular expression sort of way) and their frequency in the data. If you were given a dataset you knew nothing about, running it through pattern finder would be useful in determining how to start parsing the data.

Finally, the write library gives you the option of writing these new datastores or metrics to csvs, excels, staging tables, and more. This is essential if you are changing the format of data, as you need to be able to store the changes.

Overall, this tool aims to solve many of the problems faced in the data pipeline. It solves problems in cleaning, analyzing, and augmenting by providing tools which allow for the creation of jobs that can be run in an automated form. This is useful for anyone working on data engineering in MLOps.

## Example Tool Usage for a Movie Recommendation System:
This next section will look at how this tool can be applied to our class movie recommendation system problem. For context, we are provided data from a Kafka stream that can be in 3 different forms movie data requests, movie ratings, and movie recommendations with corresponding format of data shown in the image below. The goal is to use the provided data in order to produce the best movie recommendations for a specific user. I used the DataCleaner tool to recreate the data pipeline my group is using to clean data for our project. 
![data structure](https://github.com/srutherford2000/blog_post_on_DataCleaner/blob/main/images/data_structure.PNG?raw=true)

The pipeline my group used in the previous milestone is as follows:
1. Collect 12 hours’ worth of data
2. Separate the requests by type and create 3 separate csvs
3. Clean the requests so that only the useful fields are kept

This pipeline required a kcat command and a very long python script (and not very readable) to be run. While DataCleaner requires a datastore and we would still need to run the kcat command this tool is able to recreate the long python script in a much more readable fashion. The job I created is shown in the image below but follows this general procedure. First the data is run through a "Validate with string pattern" function which filters out rows with improperly formatted Date Time Groups. Then it checks if the User Id is an integer using another "Validate with string pattern" function. If both of those criteria are met, it then attempts to match the request field with either the data template or the rating template. If it matches either template, it uses a "Regex parser" to extract the applicable fields (title_year and rating for the rating data, title_year and minute_watched for the movie data). Once it has finished extracting data it writes the applicable columns to a corresponding csv. 
![datacleaning pipeline done in datacleaner](https://github.com/srutherford2000/blog_post_on_DataCleaner/blob/main/images/basic_pipeline.PNG?raw=true)

After I finished extracting and cleaning this data, I wanted to further use the DataCleaner tool to analyze the data. I could see this being helpful for telemetry data collection if this tool was used in production. As is shown below I made two more simple jobs. Both jobs take a datastore and run a completeness analyzer and value distribution function on it. 
![datacleaning analyzing done in datacleaner](https://github.com/srutherford2000/blog_post_on_DataCleaner/blob/main/images/basic_pipeline2.PNG?raw=true)

The results from these results were pretty interesting. It demonstrated that the most watched movies were also the most rated movies. This is helpful to verify assumptions made for our group when training a model (i.e. that we can train a model on ratings and not data as it reflects what users are generally watching anyway). I was also able to see the distribution of users who rated movies to ensure one user wasn't the one rating all the movies.
![datacleaning results done in datacleaner](https://github.com/srutherford2000/blog_post_on_DataCleaner/blob/main/images/compare_clean_data_and_ratings_results.PNG?raw=true)

Another important result came from the completeness analyzer. As you can see below, there were 82 incomplete rating records and 2333 incomplete data records. This can be traced back to the cleaning process, and they correspond to records that were filtered out for not meeting the formatting criteria required. It is somewhat frustrating that these are not automatically removed from the datastore, but it is useful to know how much information is being incorrectly formatted. This could help detect if the incoming data has shifted shape completely or if it is just a few poorly formatted records. It can also help to inform the decision of whether to remove these rows or fill in these rows with arbitrary values.
![datacleaning results_part2 done in datacleaner](https://github.com/srutherford2000/blog_post_on_DataCleaner/blob/main/images/completness_report.PNG?raw=true)

## Strengths and Limitations of DataCleaner:
Through my use of the tool I found several strengths and limitations. An overview of my observations is below.
- Strenghts:
  - Premade Data Processing Functions. The tool has a ton of common preprocessing functions already created that are optimized. This means you can focus on what needs to be done to your data and not how to do that.
  - Relatively Quick Processing. When running the job that had to do 2 formatting checks, 2 filters, and 2 regex parsers it only took about 5 minutes to run 36,000,000 rows. This is powerful.
  - Makes pipelines more readable.
- Limitations
  - Struggles with unstructured data. When loading in the original csv, DataCleaner chose to truncate the datastore at 3 columns which caused me to lose all of the recommendation data. (Movie and Rates data have 3 columns, but recommendations have 24 columns) I tried to get it to read more columns but was unsuccessful. 
  - Cannot merge tables effectively. DataCleaner only has union capabilities and not merge capabilities. I was hoping to merge movie data with movie metadata(genre, cast/crew, etc.) using the title_year as a merging key. I found I was unable to do this as there was no merge.
  -  Documentation for CLI is difficult to understand. This is a big limitation because the CLI is what would make this tool useful in MLOps. If you only use the GUI it is a static tool.

Even after reflecting on the limitations, I think it is still a useful tool that could be used to conduct the majority of cleaning. It at least gets dataframe 90% clean so that a python script to finish cleaning would be small and easy to write. Makes the entire data pipeline more understandable which is important when working in MLOps.


## Citations:
- https://datacleaner.github.io/
- https://datacleaner.github.io/docs/5.4.0/components/index.html
- https://github.com/skills/github-pages
