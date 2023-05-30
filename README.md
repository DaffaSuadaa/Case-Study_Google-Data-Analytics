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

#Importing the CSV files
q2_2019 <- read.csv("../input/data-cycleistic/Divvy_Trips_2019_Q2.csv")
q3_2019 <- read.csv("../input/data-cycleistic/Divvy_Trips_2019_Q3.csv")
q4_2019 <- read.csv("../input/data-cycleistic/Divvy_Trips_2019_Q4.csv")
q1_2020 <- read.csv("../input/data-cycleistic/Divvy_Trips_2020_Q1.csv")

#Data Inspection
head(q2_2019)
head(q3_2019)
head(q4_2019)
head(q1_2020)

#Rename data
q4_2019 <- rename(q4_2019,
                  ride_id = trip_id,
                  rideable_type = bikeid,
                  started_at = start_time,
                  ended_at = end_time,
                  start_station_name = from_station_name,
                  start_station_id = from_station_id,
                  end_station_name = to_station_name,
                  end_station_id = to_station_id,
                  member_casual = usertype)

q3_2019 <- rename(q3_2019,
                  ride_id = trip_id,
                  rideable_type = bikeid,
                  started_at = start_time,
                  ended_at = end_time,
                  start_station_name = from_station_name,
                  start_station_id = from_station_id,
                  end_station_name = to_station_name,
                  end_station_id = to_station_id,
                  member_casual = usertype)

q2_2019 <- rename (q2_2019,
                   ride_id = "X01...Rental.Details.Rental.ID",
                   rideable_type = "X01...Rental.Details.Bike.ID",
                   started_at = "X01...Rental.Details.Local.Start.Time",
                   ended_at = "X01...Rental.Details.Local.End.Time",
                   start_station_name = "X03...Rental.Start.Station.Name",
                   station_id = "X03...Rental.Start.Station.ID",
                   end_station_name = "X02...Rental.End.Station.Name",
                   end_station_id = "X02...Rental.End.Station.ID",
                   member_casual = "User.Type")

#Changing ride_id from chr to int
q4_2019 <- mutate(q4_2019, ride_id = as.character(ride_id),
                  rideable_type = as.character(rideable_type))
q3_2019 <- mutate(q3_2019, ride_id = as.character(ride_id),
                  rideable_type = as.character(rideable_type))
q2_2019 <- mutate(q2_2019, ride_id = as.character(ride_id),
                  rideable_type = as.character(rideable_type))

#Combining the individual files into one
all_trips <- bind_rows(q2_2019, q3_2019, q4_2019, q1_2020)

str(all_trips)

#Remove some fields that are not important
all_trips <- all_trips %>%
  select(-c(start_lat,start_lng,end_lat,birthyear,gender,
            "X01...Rental.Details.Duration.In.Seconds.Uncapped",
            "X05...Member.Details.Member.Birthday.Year","Member.Gender",
            tripduration))

nrow(all_trips)

head(all_trips)

#Rename subscriber and costumer to member and casual
all_trips <- all_trips %>%
  mutate(member_casual = recode(member_casual,
                                "Subscriber" = "member",
                                "Customer" = "casual"))

#Cleaning the dates
all_trips$date <- as.Date(all_trips$started_at)
all_trips$month <- format(as.Date(all_trips$date),"%m")
all_trips$day <- format(as.Date(all_trips$date), "%d")
all_trips$year <- format(as.Date(all_trips$date), "%Y")
all_trips$day_of_week <-format(as.Date(all_trips$date), "%A")

#Create and calculate ride length
all_trips$ride_length <- difftime(all_trips$ended_at, all_trips$started_at)

#Changing ride_id from chr to int
is.factor(all_trips$ride_length)
all_trips$ride_length <- as.numeric(as.character(all_trips$ride_length))
is.numeric(all_trips$ride_length)

#Removing data
all_trips_v2 <- all_trips[!(all_trips$start_station_name == "HQ QR" | all_trips$ride_length<0),]

summary(all_trips_v2$ride_length)

aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = mean)
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = median)
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = max)
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = min)

aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)

rider_type_total <- table(all_trips_v2$member_casual)
View(rider_type_total)

#Average number of member and casual users by weekly
all_trips_v2 %>%
  mutate(weekday=wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n(), average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x = weekday, y = number_of_rides, fill = member_casual)) + 
  geom_col(position = "dodge")
 
#Average usage duration of members and casuals by weekly
all_trips_v2 %>%
  mutate(weekday = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n(), average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x=weekday, y = average_duration, fill = member_casual)) +
  geom_col(position = "dodge")
 
head(all_trips_v2)

#Popular start stasion member = member
popular_start_stations_member <- all_trips_v2 %>% 
  filter(member_casual == 'member') %>% 
  group_by(start_station_name) %>% 
  summarise(number_of_starts = n()) %>% 
  filter(start_station_name != "") %>% 
  arrange(- number_of_starts)

head(popular_start_stations_member)

#Popular start stasion member = casual
popular_start_stations_member <- all_trips_v2 %>% 
  filter(member_casual == 'casual') %>% 
  group_by(start_station_name) %>% 
  summarise(number_of_starts = n()) %>% 
  filter(start_station_name != "") %>% 
  arrange(- number_of_starts)

head(popular_start_stations_member)
```

### Analysis
**Guiding Questions**
* How should you organize the data for analysis?
* Is your data formatted correctly?
* What surprises did you find in the data?
* What trends or relationships did you find in the data?
* How can these insights help answer your business questions?

**Key Tasks**
* Aggregate your data so that it is useful and accessible.
* Organize and format your data.
* Perform calculations.
* Identify trends and relationships in the data.

**Deliverable**
* Analysis summary
* ` Summary yang didapatkan ialah terdapat 902182 pengguna casual dan 2973860 member`
* `Penggunaan yang dilakukan oleh pengguna casual lebih besar yang mencapai 3552.7502 dibanding dengan member yang sebesar 850.0662`

