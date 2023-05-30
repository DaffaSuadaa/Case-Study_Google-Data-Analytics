## **Case Study: How Does A Bike-Share Navigate Speedy Success?**
In this specific case study, I am playing the role of a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, my team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, my team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

You can see the running program with R here: 

**Characters and teams:**
* Cyclistic: A bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day.
* Lily Moreno: The director of marketing and your manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels.
* Cyclistic marketing analytics team: A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy. You joined this team six months ago and have been busy learning about Cyclistic’s mission and business goals — as well as how you, as a junior data analyst, can help Cyclistic achieve them.
* Cyclistic executive team: The notoriously detail-oriented executive team will decide whether to approve the recommended marketing program.


## Road Map Case Study
### Ask
**Guiding Questions:**
* What is the problem you are trying to solve?
* How can your insights drive business decisions?

**Key Tasks**
* Identify the business task
* Consider key stakeholders

**Deliverable**

A clear statement of the business task

`Menentukan perbedaan yang terdapat di costumer antara casual dan member agar dapat meningkatkan keuntungan terhadap bisnis dengan cara meningkatkan pengguna member.`


### Prepare
**Guiding Questions:**
* Where is your data located?
* How is the data organized?
* Are there issues with bias or credibility in this data? Does your data ROCCC?
* How are you addressing licensing, privacy, security, and accessibility?
* How did you verify the data’s integrity?
* How does it help you answer your question?
* Are there any problems with the data?

**Key Tasks**
* Download data and store it appropriately
* Identify how it’s organised
* Sort and filter the data.
* Determine the credibility of the data

**Deliverable**

A description of all data sources used

`Data terdapat pada Kagle. Data ini merupakan data setahun yang dibagi menjadi 4 Querter, yang berarti berisi 4 bagian data(.csv) dengan rentang tahun 2019 - 2020.`


### Process
**Guiding questions**
* What tools are you choosing and why?
* Have you ensured your data’s integrity?
* What steps have you taken to ensure that your data is clean?
* How can you verify that your data is clean and ready to analyze?
* Have you documented your cleaning process so you can review and share those results?

**Key Tasks**
* Check the data for errors
* Choose your tools
* Transform the data so you can work with it effectively
* Document the cleaning process

**Deliverable**
* Documentation of any cleaning or manipulation of data

`Berikut merupakan proses cleaning dan manipulasi data.`

```R
#Loading library
library(tidyverse)
library(lubridate)
library(ggplot2)
```
