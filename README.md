# UberAssignment 🚙 
## Overview
This project involves the analysis and visualization of Uber trip data using R.
The analysis aims to provide insights into trip patterns by hour, day, month, and base of operation. 
The project culminates in a Shiny app that displays various charts and heat maps to interactively explore trip data.

## Data Preprocessing 
### Library used :
```
install.packages(c("readr", "dplyr", "lubridate", "ggplot2", "shiny", "leaflet"))
```

### Combine all datasets into one data frame 
```
all_trips <- bind_rows(uber_raw_data_apr14, uber_raw_data_may14, uber_raw_data_jun14, 
                       uber_raw_data_jul14, uber_raw_data_aug14, uber_raw_data_sep14)
```
### Change hour colum to 24 hr format.
Makes it easier to manipulate date and time data. 
It also extracts the hour from the Date/Time column.
```
all_trips <- all_trips %>%
  mutate(`Date/Time` = mdy_hms(`Date/Time`), 
         hour = hour(`Date/Time`))
```
### Further Date-Time Transformations and Extractions
```
all_trips$month <- month(all_trips$`Date/Time`, label = TRUE)
all_trips$day <- day(all_trips$`Date/Time`)
all_trips$weekday <- wday(all_trips$`Date/Time`, label = TRUE)
```
### Visualization and Analysis 📊 
Pivot Table: Trips by Hour
```
library(dplyr)
trips_by_hour <- all_trips %>%
  group_by(hour) %>%
  summarise(Trips = n())
```
Graph: Trips Every Hour and Month
```
library(ggplot2)
ggplot(all_trips, aes(x = hour, fill = month)) +
  geom_histogram(stat = "count", position = "dodge", binwidth = 1) +
  labs(x = "Hour of the Day", y = "Number of Trips", title = "Trips by Hour and Month")
```
(Similar formats were used for other graphs)

Heat Map: Hour and Day
```
ggplot(all_trips, aes(x = month, y = weekday, fill = Trips)) +
  geom_tile()

# Bar Chart for Trips by Base and Month (within Shiny app)
output$base_month_chart <- renderPlot({
  ggplot(filtered_data(), aes(x = Base, fill = month)) +
  geom_bar(position = "dodge")
})
```
(Similar formats were used for other Heat Maps)

