# EDA---Impact-of-COVID-19-on-social-media

covid <- read.csv('covid.csv')
library(ggplot2)
library(dplyr)
colnames(covid) <- c("timestamp", "age", "gender", "education", "soc_med_used", "hrs_be_cov", "hrs_af_cov", "news_topics", "covid_news_medium", "panic_y/n/dn", "publish_y/n", "soc_med_panic", "panic_type", "fake_news_y/n", "fake_news_med", "med_vaccine") 
covid
str(covid)

covid$timestamp <- NULL #deleting the timestamp column
covid

# DATA CLEANING ------------------

data <- split(covid, covid$`fake_news_y/n`)
data_Yes <- data$Yes
data_No <- data$No
data_No$fake_news_med <- 'None of the above'
covid <- rbind(data_Yes, data_No) 
covid$age <- sort(covid$age)  
covid <- covid[-c(30, 97, 121, 175, 187), ]
covid <- covid[-(28), ]
covid$fake_news_med[22] <- c("Youtube")

# DATA PROCESSING ---------------------

library(tidyverse)
covid <- covid %>% mutate_at(c(1, 2, 3, 5, 6, 9, 10, 11, 12, 13), as.factor)
str(covid)


# DATA VISUALISATION -----------------------------

## 1

# Bar Plot of the frequency of people having types of panic response w.r.t. educational qualification

covid %>% 
    group_by(`panic_y/n/dn`,education) %>% 
    summarise(count = n()) %>% 
    ggplot(aes(education, count, fill = `panic_y/n/dn`)) +
    geom_bar(stat = 'identity', position = 'dodge') +
    labs(x = "Education", y = "Frequency",
         fill = "Panic Response",
         title = "Panic Response vs Education") +
    theme(axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5))

## 2

# Bar Plot of the frequency of people having types of panic response w.r.t. Age group

df1 <- covid %>% 
    group_by(`panic_y/n/dn`, age) %>% 
    summarise(count = n())
ggplot(df1, aes(age, count, fill = `panic_y/n/dn`)) +
    geom_bar(stat = 'identity', position = 'dodge') +
    labs(x = "Age-group", y = "Frequency",
     fill = "Panic Response",
     title = "Panic Response vs Age Group") +
    theme(axis.text.x = element_text(angle = 45, hjust = 1, vjust = 1))

## 3

# Bar Plot of the frequency of people having types of panic response w.r.t. gender

covid %>% 
    filter(gender != "Others") %>% 
    group_by(`panic_y/n/dn`,gender) %>% 
    summarise(count = n()) %>% 
    ggplot(aes(gender, count, fill = `panic_y/n/dn`)) +
    geom_bar(stat = 'identity', position = 'dodge') +
    labs(x = "Gender", y = "Frequency",
         fill = "Panic Response",
         title = "Panic Response vs Gender") +
    theme(axis.text.x = element_text(hjust = 1, vjust = 0.5))

## 4

# Pie Charts having comparisons of the hours spend on social media before and after COVID

#Pie Chart 1: Before COVID
library(ggpubr)
covid$hrs_be_cov <- factor(covid$hrs_be_cov, labels = c('1-2','2-3','3-4','4-5','5-6','6-7','7-8','<1','>8'))
p1 <- covid %>% 
  group_by(hrs_be_cov, age) %>% 
  summarise(count = n()) %>% 
  ggplot(aes(hrs_be_cov,count, fill = age)) +
  geom_bar(stat = "identity") +
  coord_polar() +
  labs(x = "Hours spend (Before COVID)", y = "Frequency",
       title = "Comparisons of hours of social media before COVID")

#Pie Chart 2: After COVID
covid$hrs_af_cov <- factor(covid$hrs_af_cov, labels = c('1-2','2-3','3-4','4-5','5-6','6-7','7-8','<1','>8'))
p2 <- covid %>% 
  group_by(hrs_af_cov, age) %>% 
  summarise(count = n()) %>% 
  ggplot(aes(hrs_af_cov,count, fill = age)) +
  geom_bar(stat = "identity") +
  coord_polar() +
  labs(x = "Hours spend (After COVID)", y = "Frequency",
       title = "Comparisons of hours of social media after COVID")

## 5

# Combining the two pie charts:

ggpubr::ggarrange(p1, p2, ncol = 2, legend = "top", common.legend = T)

## 6

# A stacked bar plot of the no of people having diff types of panic w.r.t education

df2 <- covid %>% 
  group_by(education, panic_type) %>% 
  summarise(count = n()) 
df2 <- df2[-14,]
p1 <- df2 %>% 
  ggplot(aes(education,count,fill = panic_type)) +
  geom_bar(stat = 'identity') +
  labs(x = "education", fill = "Panic Type", title = "Panic type categories by education") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5))
p1

## 7

# A stacked bar plot of the no of people having diff types of panic w.r.t age group

df2 <- covid %>% 
  group_by(age, panic_type) %>% 
  summarise(count = n()) 
df2 <- df2[-3,]
p2 <- df2 %>% 
  ggplot(aes(age,count,fill = panic_type)) +
  geom_bar(stat = 'identity') +
  labs(x = "age group", fill = "Panic Type", title = "Panic type categories by age group") 
p2

## 8

# A stacked bar plot of the no of people having diff types of panic w.r.t gender

df2 <- covid %>% 
  group_by(gender, panic_type) %>% 
  summarise(count = n()) 
df2 <- df2[-3,]
p3 <- df2 %>% 
  ggplot(aes(gender,count,fill = panic_type)) +
  geom_bar(stat = 'identity') + 
  labs(x = "Gender", fill = "Panic Type", title = "Panic type categories by gender") 
p3

## 9

# Combining the 3 bar plots:

ggpubr::ggarrange(p1,p2,p3, common.legend = T, nrow = 1, legend = 'top')

## 10

# A grouped bar plot showing the no of people having panic through social media w.r.t education

p1 <- covid %>% 
  group_by(soc_med_panic,education) %>% 
  summarise(count = n()) %>% 
  ggplot(aes(education, count, fill = soc_med_panic)) +
  geom_bar(stat = 'identity', position = 'dodge') +
  labs(x = "Education", y = "Frequency",
       fill = "Social Media",
       title = "Panic through social media vs Education") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5))

## 11

# A grouped bar plot showing the no of people having panic through social media w.r.t age group

p2 <- covid %>% 
  group_by(soc_med_panic,age) %>% 
  summarise(count = n()) %>% 
  ggplot(aes(age, count, fill = soc_med_panic)) +
  geom_bar(stat = 'identity', position = 'dodge') +
  labs(x = "Age Group", y = "Frequency",
       fill = "Social Media",
       title = "Panic through social media vs Age group")

## 12

# A grouped bar plot showing the no of people having panic through social media w.r.t gender

p3 <- covid %>% 
  group_by(soc_med_panic,gender) %>% 
  summarise(count = n()) %>% 
  ggplot(aes(gender, count, fill = soc_med_panic)) +
  geom_bar(stat = 'identity', position = 'dodge') +
  labs(x = "Gender", y = "Frequency",
       fill = "Social Media",
       title = "Panic through social media vs Gender")
p1
p2
p3

## 13

# Combining the three graphs:

ggpubr::ggarrange(p1,p2,p3, common.legend = T, nrow = 1)

## 14

# A donut pie chart showing percentages of people having social media panic through percentages in education

df3 <- covid %>% 
  group_by(soc_med_panic,education) %>% 
  summarise(count = n())
library(webr)
df3
PieDonut(df3,aes(soc_med_panic, education, count = count),
         showPieName = F, title = "Social Media panic by education")

# Load the data

age_sc_vac <- read.csv("age_sc_vaccine.csv")
covid2 <- covid
age_sc_vac <- age_sc_vac[-c(30, 97, 121, 175, 187),]

## 15 

# A stacked bar plot showing the count of people who got news related to vaccine through social media w.r.t age group

df1 <- age_sc_vac %>% 
  gather(media,value,2:9) %>% 
  group_by(age,media) %>% summarise(count = sum(value))
ggplot(df1, aes(age,count,fill = media)) + geom_bar(stat = 'identity')+
  labs(title = "Vaccine news through social media w.r.t. age group")
                                                

## 16

# A stacked bar plot showing the count of people who got news related to vaccine through social media w.r.t gender

df2 <- cbind(gender = covid2$gender,age_sc_vac[,-1]) %>% 
  gather(media,value,2:9) %>% 
  group_by(gender,media) %>% summarise(count = sum(value))
ggplot(df2, aes(gender,count,fill = media)) + geom_bar(stat = 'identity')+
  labs(title = "Vaccine news through social media w.r.t. gender")

## 17

# A stacked bar plot showing the count of people who got news related to vaccine through social media w.r.t eduaction

df3 <- cbind(education = covid2$education,age_sc_vac[,-1]) %>% 
  gather(media,value,2:9) %>% 
  group_by(education,media) %>% summarise(count = sum(value))
ggplot(df3, aes(education,count,fill = media)) + geom_bar(stat = 'identity')+
  labs(title = "Vaccine news through social media w.r.t. education")


age_news <- read.csv("age_news.csv")
age_news <- age_news[-c(30, 97, 121, 175, 187),]

## 18

# Grouped Bar plot showing the no of people getting diff types of news from social media w.r.t. age group

df4 <- age_news %>% 
  gather(news,value,2:9) %>% 
  group_by(age,news) %>% summarise(count = sum(value))
p18 <- ggplot(df4, aes(age,count,fill = news)) + geom_bar(stat = 'identity',position = 'dodge')+
  labs(title = "News from social media vs Age")
p18

## 19

# Grouped Bar plot showing the no of people getting diff types of news from social media w.r.t. education

df5 <- cbind(education = covid$education[-277],age_news[,-1]) %>% 
  gather(news,value,2:9) %>% 
  group_by(education,news) %>% summarise(count = sum(value))
p19 <- ggplot(df5, aes(education,count,fill = news)) + geom_bar(stat = 'identity',position = 'dodge')+
  labs(title = "News from social media vs Education")+
  theme(axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5))

p19


age_covid_news <- read.csv("age_covid_news.csv")

## 20

# Grouped Bar plot showing the no of people who got covid related news from social media w.r.t. age group

df6 <- age_covid_news %>% 
  gather(covid_news_med,value,2:9) %>% 
  group_by(age,covid_news_med) %>% summarise(count = sum(value))
p20 <- ggplot(df6, aes(age,count,fill = covid_news_med)) + geom_bar(stat = 'identity',position = 'dodge')+
  labs(title = "Covid News from Social Media vs Age")
p20


## 21

# Grouped Bar plot showing the no of people who got covid related news from social media w.r.t. education

df7 <- cbind(education = covid$education,age_covid_news[-c(30, 97, 121, 175, 187, 150),-1]) %>% 
  gather(covid_news_med,value,2:9) %>% 
  group_by(education,covid_news_med) %>% summarise(count = sum(value))
p21 <- ggplot(df7, aes(education,count,fill = covid_news_med)) + geom_bar(stat = 'identity',position = 'dodge')+
  labs(title = "Covid News from Social Media vs Education")+
  theme(axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5))
p21

## 22

# Combination of the grouped bar plots b/w news from social media and covid news from social media w.r.t. age

ggarrange(p18,p20)

## 23

# Combination of the grouped bar plots b/w news from social media and covid news from social media w.r.t. education

ggarrange(p19,p21)


##24

#Pie Chart of the count of people having panic types grouped by social media panic

df8 <- covid %>% group_by(panic_type,soc_med_panic) %>% summarise(count = n())
df8 <- df8[-7,]
ggplot(df8, aes(panic_type,count,fill=soc_med_panic)) + geom_bar(stat= "identity") + 
  coord_polar() + labs(title = "Panic Type and Social Media Panic")
