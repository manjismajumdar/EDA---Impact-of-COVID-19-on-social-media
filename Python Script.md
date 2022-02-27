# Project Work
# An EDA on Impact of Covid on social media 

# Group Members:
# 1. Ratul Chakraborty
# 2. Kazi Jishan Zaman
# 3. Manjis Majumdar

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("Impact of COVID-19 on social media.csv")
df.head()

df.columns = ["Timestamp", "age", "gender", "education", "soc_med_used", "hrs_be_cov", "hrs_af_cov", "news_topic", "covid_news_medium", "panic_y/n/dn", "publish_y/n", "soc_med_panic", "panic_type", "fake_news_y/n", "fake_source", "most_vaccine"]
df.head()

df = df.drop("Timestamp", axis = 1)
df.head()

df['panic_type'].replace({"Both of the above":"Both","News":"other", "I feel panicked more by the tampering of test reports, or death counts, dearth of good hospitals in the districts and such things. The whole scenario is sickening. I fear not only the virus but also the circumstances I am living in." : "other"}, inplace = True)

# plot 1

plt.figure(figsize = (13, 5), dpi = 150)
sns.countplot(data = df, x= "education", hue = "panic_y/n/dn")
plt.title("Panic response vs Educational qualifiaction", fontdict={"family":"serif", "color":"darkred", "size":20})
plt.xlabel("Education ----->")
plt.ylabel("Frequency ----->")
plt.legend(title = "Panic Response", loc = "upper right")
plt.savefig("Plot 1.jpg")

# plot 2

plt.figure(figsize = (13, 5), dpi = 150)
sns.countplot(data = df, x= "age", hue = "panic_y/n/dn")
plt.title("Panic response vs Age Group", fontdict={"family":"serif", "color":"darkred", "size":20})
plt.xlabel("Age Group ----->")
plt.ylabel("Frequency ----->")
plt.legend(title = "Panic Response")
plt.savefig("Plot 2.jpg")

# plot 3

plt.figure(figsize = (13, 5), dpi = 150)
sns.countplot(data = df, x= "gender", hue = "panic_y/n/dn")
plt.title("Panic response vs Gender", fontdict={"family":"serif", "color":"darkred", "size":20})
plt.xlabel("Gender ----->")
plt.ylabel("Frequency ----->")
plt.legend(loc = "upper right", title = "Panic Response")
plt.savefig("Plot 3.jpg")

# comparison 1, 2, 3

fig, axes = plt.subplots(nrows = 1, ncols = 3, dpi = 150, figsize = (8, 6))

# 1
sns.countplot(data = df, x= "education", hue = "panic_y/n/dn", ax = axes[0])
axes[0].set_ylim([0, 95])
axes[0].set_title("Vs Education", fontsize = 8)
axes[0].set_xlabel("")
axes[0].set_ylabel("Frequency ----->")
axes[0].legend().remove()
axes[0].tick_params(labelsize = 4, rotation = 90)


#2
sns.countplot(data = df, x= "age", hue = "panic_y/n/dn", ax = axes[1])
axes[1].set_ylim([0, 95])
axes[1].set_title("Vs Age", fontsize = 8)
axes[1].set_xlabel("")
axes[1].set_ylabel("")
axes[1].legend().remove()
axes[1].tick_params(labelsize = 8, rotation = 90)


#3
sns.countplot(data = df, x= "gender", hue = "panic_y/n/dn", ax = axes[2])
axes[2].set_ylim([0, 95])
axes[2].set_title("Vs Gender", fontsize = 8)
axes[2].set_xlabel("")
axes[2].set_ylabel("")
axes[2].legend(bbox_to_anchor = [0.8, -0.15], ncol = 4, fancybox = True,fontsize = 7)
axes[2].tick_params(labelsize = 8, rotation = 90)

fig.tight_layout()
fig.suptitle("Panic Response", fontdict={"family":"serif", "color":"darkred", "size":20}, y = 1)
plt.savefig("Plot 1_2_3_combined.jpg")

hrs_be = df["hrs_be_cov"].value_counts()
abc = hrs_be.sort_index()
hrs_be_age = df[["hrs_be_cov", "age"]].value_counts()
fed = pd.DataFrame(hrs_be_age).sort_index()
fed.index = np.array(["15-24", "25-34", "35-44", "45-54", "55-64", "65-74", "15-24", "25-34", "35-44", "45-54", "65-74",
                     "15-24", "25-34", "45-54", "15-24", "25-34", "45-54", "15-24", "35-44", "15-24", "25-34", "45-54", "15-24",
                     "15-24", "25-34", "35-44", "45-54", "55-64", "65-74", "15-24"])
hrs_af = df["hrs_af_cov"].value_counts()
ghi = hrs_af.sort_index()
hrs_af_age = df[["hrs_af_cov", "age"]].value_counts()
klm = pd.DataFrame(hrs_af_age).sort_index()
klm.index = np.array(["15-24", "25-34", "35-44", "45-54", "55-64", "65-74", "15-24", "25-34", "35-44", "45-54", "55-64", "65-74",
                     "15-24", "25-34","35-44", "45-54","65-74", "15-24", "45-54", "15-24", "15-24", "25-34", "15-24","35-44", "45-54",
                     "15-24", "35-44", "45-54", "55-64", "65-74", "15-24", "25-34", "45-54"])

# plot 4

fig, axes = plt.subplots(nrows = 1, ncols = 2, dpi = 200)

# 1st subplot

# 1st layer
axes[0].pie(fed[0], labels = fed.index, radius = 1+0.3, wedgeprops={'linewidth': 1.0, 'edgecolor': 'white'},
       colors = ["#D64748", "#C74445", "#E0282A", "#E83537", "#EB4D4F", "#E07274",
                "#E62589", "#EB2A8E", "#F73B9D", "#AB0059", "#ED6BAF",
                "#CC21BB", "#FF42EC", "#CC78C4",
                "#2C1B7D", "#2C1B7D", "#4234D9",
                "#237FA1", "#237FA1", 
                "#278A6A", "#11D495",
                "#ADEDB2", 
                "#5C3E19", "#A65C00", "#F29827", "#FF8E00", "#FFBB66", "#FCDBB1",
                "#EBE65E"], textprops={'fontsize': 4})
# 2nd layer
axes[0].pie(abc , autopct = '%1.1f%%', shadow = True, wedgeprops={'linewidth': 1.0, 'edgecolor': 'white'},textprops = {"fontsize":5},
       colors = ["#B03E3F", "#AD3674", "#9D23A6", "#30298A", "#265769", "#2A6351", "#219E2A", "#966221", "#FFF700"])
# 3rd layer
circle = plt.Circle((0,0),radius = 1-0.7, color = "white")
axes[0].add_patch(circle)
axes[0].set_title("Before", y = 0.45, fontsize = 9)

# 2nd subplot
# 1st layer
axes[1].pie(klm[0],labels = klm.index, radius = 1+0.3, wedgeprops={'linewidth': 1.0, 'edgecolor': 'white'},
       colors = ["#D64748", "#C74445", "#E0282A", "#E83537", "#EB4D4F", "#E07274",
                "#E62589", "#EB2A8E", "#F73B9D", "#AB0059", "#ED6BAF","#C79BB2",
                "#CC21BB", "#FF42EC", "#CC78C4","#FF6BF1", "#590251",
                "#2C1B7D", "#2C1B7D",
                "#237FA1", 
                "#278A6A", "#11D495", "#82BDAA",
                "#ADEDB2","#0FD41D", "#B8F2BC", 
                "#5C3E19", "#A65C00", "#F29827", "#FF8E00", "#FFBB66", "#FCDBB1",
                "#EBE65E", "#7D7904", "#C7C581"], textprops={'fontsize': 4})
# 2nd layer
axes[1].pie(ghi , autopct = '%1.1f%%', shadow = True, wedgeprops={'linewidth': 1.0, 'edgecolor': 'white'}, textprops = {"fontsize":5},
       colors = ["#B03E3F", "#AD3674", "#9D23A6", "#30298A", "#265769", "#2A6351", "#219E2A", "#966221", "#FFF700"])
# 3rd layer
circle = plt.Circle((0,0),radius = 1-0.7, color = "white")
axes[1].add_patch(circle)
axes[1].set_title("After", y = 0.44)

fig.tight_layout()

# add legend
colors = {'1-2':'#B03E3F', "2-3":"#AD3674", "3-4":"#9D23A6", '4-5':'#30298A', "5-6":'#265769', "6-7":"#2A6351", "7-8":"#219E2A",
         "<1":"#966221", ">8":"#FFF700"}         
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, title = "Hours on Social Media", fontsize = 7,
          loc = "upper center", bbox_to_anchor = [-0.1, -0.1], ncol = 9, fancybox = True)

fig.suptitle("Hours on Social Media Vs Age", y = 0.9, fontdict={"family":"serif", "color":"darkred", "size":20})
#plt.savefig("hrs_age.jpg")
plt.savefig("Plot 4 proper.jpg")

df4 = df.groupby(['panic_type', 'education']).size().reset_index().pivot(columns='panic_type', index='education', values=0)
df4 = df4.fillna(0)
df4
# plot 5 (renewd)
plt.figure(figsize = (13, 4), dpi = 150)

plt.bar(df4.index, df4.Physical, color = "red")
plt.bar(df4.index, df4.Psychological, bottom = df4.Physical, color = "blue")
plt.bar(df4.index, df4.Both, bottom = df4.Psychological + df4.Physical, color = "orange")
plt.bar(df4.index, df4["None"], bottom = df4.Both + df4.Psychological + df4.Physical, color = "green")
plt.bar(df4.index, df4.other, bottom = df4["None"] + df4.Both + df4.Psychological + df4.Physical, color = "purple")

plt.xlabel("Education ------>")
plt.ylabel("Frequency ------>")
plt.title("Panic Type vs Education", fontdict={"family":"serif", "color":"darkred", "size":20})

colors = {'Physical':'red', "Psychological":"blue", "Both":"orange", 'None':'green', "other":'purple'}         
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, title = "Panic type")
plt.savefig("Plot 5.jpg")

df5 = df.groupby(['panic_type', 'age']).size().reset_index().pivot(columns='panic_type', index='age', values=0)
df5 = df5.fillna(0)
df5

# plot 6 (renewd)

plt.figure(figsize = (13, 5), dpi = 150)

plt.bar(df5.index, df5.Physical, color = "red")
plt.bar(df5.index, df5.Psychological, bottom = df5.Physical, color = "blue")
plt.bar(df5.index, df5.Both, bottom = df5.Psychological + df5.Physical, color = "orange")
plt.bar(df5.index, df5["None"], bottom = df5.Both + df5.Psychological + df5.Physical, color = "green")
plt.bar(df5.index, df5.other, bottom = df5["None"] + df5.Both + df5.Psychological + df5.Physical, color = "purple")

plt.xlabel("Age Group ------>")
plt.ylabel("Frequency ------>")
plt.ylim((0, 175))
plt.title("Panic Type vs Age Group", fontdict={"family":"serif", "color":"darkred", "size":20})

colors = {'Physical':'red', "Psychological":"blue", "Both":"orange", 'None':'green', "other":'purple'}         
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, title = "Panic type")
plt.savefig("Plot 6.jpg")

df6 = df.groupby(['panic_type', 'gender']).size().reset_index().pivot(columns='panic_type', index='gender', values=0)
df6 = df6.fillna(0)
df6

# plot 7 (renewd)

plt.figure(figsize = (13, 5), dpi = 150)

plt.bar(df6.index, df6.Physical, color = "red")
plt.bar(df6.index, df6.Psychological, bottom = df6.Physical, color = "blue")
plt.bar(df6.index, df6.Both, bottom = df6.Psychological + df6.Physical, color = "orange")
plt.bar(df6.index, df6["None"], bottom = df6.Both + df6.Psychological + df6.Physical, color = "green")
plt.bar(df6.index, df6.other, bottom = df6["None"] + df6.Both + df6.Psychological + df6.Physical, color = "purple")

plt.xlabel("Gender ------>")
plt.ylabel("Frequency ------>")
plt.ylim((0, 175))
plt.title("Panic Type vs Gender", fontdict={"family":"serif", "color":"darkred", "size":20})

colors = {'Physical':'red', "Psychological":"blue", "Both":"orange", 'None':'green', "other":'purple'}         
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, title = "Panic type")
plt.savefig("Plot 7.jpg")

# plot 8

fig, axes = plt.subplots(nrows = 1, ncols = 3, dpi = 150)

# 1st
axes[0].bar(df4.index, df4.Physical, color = "red")
axes[0].bar(df4.index, df4.Psychological, bottom = df4.Physical, color = "blue")
axes[0].bar(df4.index, df4.Both, bottom = df4.Psychological + df4.Physical, color = "orange")
axes[0].bar(df4.index, df4["None"], bottom = df4.Both + df4.Psychological + df4.Physical, color = "green")
axes[0].bar(df4.index, df4.other, bottom = df4["None"] + df4.Both + df4.Psychological + df4.Physical, color = "purple")

axes[0].set_ylim((0,175))
axes[0].set_xlabel("Education ------>")
axes[0].set_ylabel("Frequency ------>")
axes[0].set_title("Vs Education", fontsize = 8)
axes[0].tick_params(labelsize = 4, rotation = 90)

# 2nd
axes[1].bar(df5.index, df5.Physical, color = "red")
axes[1].bar(df5.index, df5.Psychological, bottom = df5.Physical, color = "blue")
axes[1].bar(df5.index, df5.Both, bottom = df5.Psychological + df5.Physical, color = "orange")
axes[1].bar(df5.index, df5["None"], bottom = df5.Both + df5.Psychological + df5.Physical, color = "green")
axes[1].bar(df5.index, df5.other, bottom = df5["None"] + df5.Both + df5.Psychological + df5.Physical, color = "purple")

axes[1].set_ylim((0,175))
axes[1].set_xlabel("Age Group ------>")
axes[1].set_ylabel("")
axes[1].set_title("Vs Age Group", fontsize = 8)
axes[1].tick_params(labelsize = 4, rotation = 90)

# 3rd
axes[2].bar(df6.index, df6.Physical, color = "red")
axes[2].bar(df6.index, df6.Psychological, bottom = df6.Physical, color = "blue")
axes[2].bar(df6.index, df6.Both, bottom = df6.Psychological + df6.Physical, color = "orange")
axes[2].bar(df6.index, df6["None"], bottom = df6.Both + df6.Psychological + df6.Physical, color = "green")
axes[2].bar(df6.index, df6.other, bottom = df6["None"] + df6.Both + df6.Psychological + df6.Physical, color = "purple")

axes[2].set_ylim((0,175))
axes[2].set_xlabel("Gender ------>")
axes[2].set_ylabel("")
axes[2].set_title("Vs Gender", fontsize = 8)
axes[2].tick_params(labelsize = 4, rotation = 90)

colors = {'Physical':'red', "Psychological":"blue", "Both":"orange", 'None':'green', "other":'purple'}
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, fontsize = 5, title = "Panic Type",loc = "upper center", bbox_to_anchor = [0, -0.2], ncol = 5, fancybox = True)

fig.suptitle("Comparison Through Panic Type", fontdict={"family":"serif", "color":"darkred", "size":20})
plt.savefig("Plot 8.jpg")

# plot 9

plt.figure(figsize = (13, 5), dpi = 150)
sns.countplot(data = df, x= "education", hue = "soc_med_panic")
plt.title("Panic through social media vs Education", fontdict={"family":"serif", "color":"darkred", "size":20})
plt.xlabel("Education ----->")
plt.ylabel("Frequency ----->")
plt.legend(bbox_to_anchor = [0.9,1], title = "Social Media")
plt.savefig("Plot 9.jpg")

# plot 10 

plt.figure(figsize = (13, 5), dpi = 150)
sns.countplot(data = df, x= "age", hue = "soc_med_panic")
plt.title("Panic through social media vs Age group", fontdict={"family":"serif", "color":"darkred", "size":20})
plt.xlabel("Age Group ----->")
plt.ylabel("Frequency ----->")
plt.legend(bbox_to_anchor = [0.9, 1], title = "Social Media")
plt.savefig("Plot 10.jpg")

# plot 11

plt.figure(figsize = (13, 5), dpi = 150)
sns.countplot(data = df, x= "gender", hue = "soc_med_panic")
plt.title("Panic through social media vs Gender", fontdict={"family":"serif", "color":"darkred", "size":20})
plt.xlabel("Gender ----->")
plt.ylabel("Frequency ----->")
plt.legend(bbox_to_anchor = [0.85, 1], title = "Social Media")
plt.savefig("Plot 11.jpg")

# plot 12

fig, axes = plt.subplots(nrows = 1, ncols = 3, dpi = 150, figsize = (10, 8))

# 1st
sns.countplot(data = df, x= "education", hue = "soc_med_panic", ax = axes[0])
axes[0].set_xlabel("Education ---->")
axes[0].set_ylabel("Frequency ----->")
axes[0].set_title("Vs Education", fontsize = 8)
axes[0].get_legend().remove()
axes[0].tick_params(labelsize = 4, rotation = 90)
# 2nd
sns.countplot(data = df, x= "age", hue = "soc_med_panic", ax = axes[1])
axes[1].set_xlabel("Age Group ---->")
axes[1].set_ylabel("")
axes[1].set_title("Vs Age", fontsize = 8)
axes[1].get_legend().remove()
axes[1].tick_params(labelsize = 6, rotation = 90)
# 3rd
sns.countplot(data = df, x= "gender", hue = "soc_med_panic", ax = axes[2])
axes[2].set_xlabel("Gender ---->")
axes[2].set_ylabel("")
axes[2].set_title("Vs Gender", fontsize = 8)
axes[2].get_legend().remove()
axes[2].tick_params(labelsize = 6, rotation = 90)

colors = {"Facebook":"blue", "Whatsapp":"orange", "Don't know":'red', 'YouTube':'green', "Instagram":"violet", "Twitter":"brown"}         
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, fontsize = 8,loc = "upper center",
           bbox_to_anchor = [0, -0.1], ncol = 6, fancybox = True)

fig.suptitle("Panic through Social Media", fontdict={"family":"serif", "color":"darkred", "size":20}, y = 0.92)
#fig.tight_layout()
plt.savefig("Plot 12.jpg")

med_pan = df["soc_med_panic"].value_counts(); med_pan
pqr = med_pan.sort_index()
med_pan_ed = df[["soc_med_panic", "education"]].value_counts(); med_pan_ed
mno = pd.DataFrame(med_pan_ed).sort_index()
mno.index = ["Diploma", "Graduate", "10+2", "PhD", "PG", "Graduate", "10+2", "PhD", "PG", "Secondary", "UG", 
             "Graduate", "10+2", "PG", "Graduate", "Graduate", "10+2", "PhD", "PG", "UG", "Graduate", "10+2", "PG"]

# plot 13

plt.figure(figsize = (10, 6), dpi = 150)

# 1st layer
plt.pie(mno[0], labels = mno.index, radius = 1+0.3, wedgeprops={'linewidth': 1.0, 'edgecolor': 'white'},
       colors = ["#D64748", "#C74445", "#E0282A", "#E83537", "#EB4D4F",
                "#E62589", "#EB2A8E", "#F73B9D", "#AB0059", "#ED6BAF", "#e05ca1",
                "#CC21BB", "#FF42EC", "#65126b",
                "#2C1B7D",
                "#237FA1", "#237FA1", "#97d8f0", "#cee7f0", "#6e8891",
                "#278A6A", "#11D495", "#82BDAA"], textprops={'fontsize': 5}, autopct = "%1.1f%%", pctdistance = 0.93)
# 2nd layer
plt.pie(pqr , autopct = '%1.1f%%', shadow = True, wedgeprops={'linewidth': 1.0, 'edgecolor': 'white'}, pctdistance = 0.8,
       colors = ["#B03E3F", "#AD3674", "#9D23A6", "#30298A", "#265769", "#2A6351"], textprops = {"fontsize":6})
# 3rd layer
circle = plt.Circle((0,0),radius = 1-0.5, color = "white")
plt.gca().add_patch(circle)
# remove legend
plt.legend("")
# add legend
colors = {"Don't know":'#B03E3F', "Facebook":"#AD3674", "Instagram":"#9D23A6", 'Twitter':'#30298A',
          "Whatsapp":'#265769', "Youtube":"#2A6351"}
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, title = "Social Media", fontsize = 7, bbox_to_anchor = [0.95, 0.2])

plt.title("Social Media\nspreading Panic\nVs\nEducation", y = 0.42, fontdict={"family":"serif", "color":"darkred", "size":12})
plt.savefig("Plot 13.jpg")

df1 = df["most_vaccine"].str.get_dummies(";")
df1["age"] = df["age"]
df1["gender"] = df["gender"]
df1["education"] = df["education"]
df1.columns = ["fa", "in", "li", "sn", "te", "tw", "wh", "yt", "age", "gender", "education"]
df1

# plot 14

temp1 = df1.groupby("age").sum()
temp1
plt.figure(figsize = (13, 4), dpi = 150)

plt.bar(temp1.index, temp1.fa, color = "red")
plt.bar(temp1.index, temp1["in"] , bottom = temp1.fa, color = "blue")
plt.bar(temp1.index, temp1.li, bottom = temp1["in"] + temp1.fa, color = "green")
plt.bar(temp1.index, temp1.sn,bottom = temp1.li + temp1["in"] + temp1.fa, color = "yellow")
plt.bar(temp1.index, temp1.te, bottom = temp1.sn + temp1.li + temp1["in"] + temp1.fa, color = "pink")
plt.bar(temp1.index, temp1.tw, bottom = temp1.te + temp1.sn + temp1.li + temp1["in"] + temp1.fa, color = "violet")
plt.bar(temp1.index, temp1.wh, bottom = temp1.tw + temp1.te + temp1.sn + temp1.li + temp1["in"] + temp1.fa, color = "orange")
plt.bar(temp1.index, temp1.yt, bottom = temp1.wh + temp1.tw + temp1.te + temp1.sn + temp1.li + temp1["in"] + temp1.fa, color = "seagreen")

plt.xlabel("Age ------>")
plt.ylabel("Frequency ------>")
plt.title("Vaccine News through Social Media vs Age", fontdict={"family":"serif", "color":"darkred", "size":20})

colors = {'Facebook':'red', "Instagram":"blue", "Whatsapp":"orange", 'Linkdin':'green', "snapchat":"yellow",  "Twitter":'violet', "Youtube":"seagreen"}         
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, title = "Social Media")
plt.savefig("Plot 14.jpg")

# plot 15

temp2 =  df1.groupby("gender").sum()
temp2
plt.figure(figsize = (13, 5), dpi = 150)
plt.bar(temp2.index, temp2.fa, color = "red")
plt.bar(temp2.index, temp2["in"] , bottom = temp2.fa, color = "blue")
plt.bar(temp2.index, temp2.li, bottom = temp2["in"] + temp2.fa, color = "green")
plt.bar(temp2.index, temp2.sn, bottom = temp2.li + temp2["in"] + temp2.fa, color = "yellow")
plt.bar(temp2.index, temp2.te, bottom = temp2.sn + temp2.li + temp2["in"] + temp2.fa, color = "pink")
plt.bar(temp2.index, temp2.tw, bottom = temp2.te + temp2.sn + temp2.li + temp2["in"] + temp2.fa, color = "violet")
plt.bar(temp2.index, temp2.wh, bottom = temp2.tw + temp2.te + temp2.sn + temp2.li + temp2["in"] + temp2.fa, color = "orange")
plt.bar(temp2.index, temp2.yt, bottom = temp2.wh + temp2.tw + temp2.te + temp2.sn + temp2.li + temp2["in"] + temp2.fa, color = "seagreen")

plt.xlabel("Gender ------>")
plt.ylabel("Frequency ------>")
plt.title("Vaccine News through Social Media vs Gender", fontdict={"family":"serif", "color":"darkred", "size":20})

colors = {'Facebook':'red', "Instagram":"blue", "Whatsapp":"orange", 'Linkdin':'green', "snapchat":"yellow",  "Twitter":'violet', "Youtube":"seagreen"}         
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, title = "Social Media")
plt.savefig("Plot 15.jpg")

# plot 16

temp3 = df1.groupby("education").sum()
temp3
plt.figure(figsize = (13, 5), dpi = 150)

plt.bar(temp3.index, temp3.fa, color = "red")
plt.bar(temp3.index, temp3["in"] , bottom = temp3.fa, color = "blue")
plt.bar(temp3.index, temp3.li, bottom = temp3["in"] + temp3.fa, color = "green")
plt.bar(temp3.index, temp3.sn, bottom = temp3.li + temp3["in"] + temp3.fa, color = "yellow")
plt.bar(temp3.index, temp3.te, bottom = temp3.sn + temp3.li + temp3["in"] + temp3.fa, color = "pink")
plt.bar(temp3.index, temp3.tw, bottom = temp3.te + temp3.sn + temp3.li + temp3["in"] + temp3.fa, color = "violet")
plt.bar(temp3.index, temp3.wh, bottom = temp3.tw + temp3.te + temp3.sn + temp3.li + temp3["in"] + temp3.fa, color = "orange")
plt.bar(temp3.index, temp3.yt, bottom = temp3.wh + temp3.tw + temp3.te + temp3.sn + temp3.li + temp3["in"] + temp3.fa, color = "seagreen")

plt.xlabel("Education ------>")
plt.ylabel("Frequency ------>")
plt.title("Vaccine News through Social Media vs Education", fontdict={"family":"serif", "color":"darkred", "size":20})

colors = {'Facebook':'red', "Instagram":"blue", "Whatsapp":"orange", 'Linkdin':'green', "snapchat":"yellow",  "Twitter":'violet', "Youtube":"seagreen"}         
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, title = "Social Media")
plt.savefig("Plot 16.jpg")

df2 = df["news_topic"].str.get_dummies(";")
# df2.columns = ["cu", "ec", "he", "mi", "po", "so", "sp", "te"]
df2["age"] = df["age"]
df2["education"] = df["education"]
df2.head()

# plot 17

temp4 = df2.groupby("age").sum()
temp4
f = plt.figure(figsize = (20, 5), dpi = 150)

# f.set_figwidth(40)
# f.set_figheight(6)

temp4.plot(kind = "bar")

plt.xlabel("Age ----->")
plt.ylabel("Frequency ---->")
plt.title("News from Social Media vs Age")

plt.legend(bbox_to_anchor = [1.05, 0.8], title = "News")
plt.savefig("Plot 17.jpg")

# plot 18

temp5 = df2.groupby("education").sum()
temp5
f = plt.figure(figsize = (20, 5), dpi = 150)


temp5.plot(kind = "bar")

plt.xlabel("Education ----->")
plt.ylabel("Frequency ---->")
plt.title("News from Social Media vs Education")

plt.legend(bbox_to_anchor = [1.1, 0.8], title = "News")
plt.savefig("Plot 18.jpg")

df3 = df["covid_news_medium"].str.get_dummies(";")
#df3.columns = ["fa", "in", "li", "sn", "te", "tw", "wh", "yt"]
df3["age"] = df["age"]
df3["education"] = df["education"]
df3

# plot 19

temp6 = df3.groupby("age").sum()
temp6
f = plt.figure(figsize = (20, 5), dpi = 150)

temp6.plot(kind = "bar")

plt.xlabel("Age ----->")
plt.ylabel("Frequency ---->")
plt.title("Covid News from Social Media vs Age")

plt.legend(bbox_to_anchor = [1.02, 0.8], title = "Social Media")
plt.savefig("Plot 19.jpg")

# plot 20

temp7 = df3.groupby("education").sum()
temp7
f = plt.figure(figsize = (20, 5), dpi = 150)

temp7.plot(kind = "bar")

plt.xlabel("Education ----->")
plt.ylabel("Frequency ---->")
plt.title("Covid News from Social Media vs Education")

plt.legend(bbox_to_anchor = [1.02, 0.8], title = "Social Media")
plt.savefig("Plot 20.jpg")

# plot 21

fig, axes = plt.subplots(nrows = 1, ncols = 2, dpi = 150)

temp4.plot(kind = "bar", ax = axes[0])
axes[0].set_xlabel("Age ---->")
axes[0].set_ylabel("Frequency ----->")
axes[0].set_title("News from Social Media vs Age", fontsize = 10)
axes[0].legend(fontsize = 5, title = "News")
axes[0].tick_params(labelsize = 6)


temp6.plot(kind = "bar", ax = axes [1])
axes[1].set_xlabel("Age ----->")
axes[1].set_title("Covid News spreading Social Media vs Age", fontsize = 8)
axes[1].legend(fontsize = 5, title = "Social Media")
axes[1].tick_params(labelsize = 6)


fig.suptitle("Comparison between prefrence of news and News medium about age", fontdict={"family":"serif", "color":"darkred", "size":10})
fig.tight_layout()
plt.savefig("Plot 21.jpg")

# plot 22

fig, axes = plt.subplots(nrows = 1, ncols = 2, dpi = 150)

temp5.plot(kind = "bar", ax = axes[0])
axes[0].set_xlabel("Education ---->")
axes[0].set_ylabel("Frequency ----->")
axes[0].set_title("News from Social Media vs Education", fontsize = 9)
axes[0].legend(fontsize = 5, title = "News", bbox_to_anchor = [1.1, 1])
axes[0].tick_params(labelsize = 4)


temp7.plot(kind = "bar", ax = axes [1])
axes[1].set_xlabel("Education ----->")
axes[1].set_title("Covid News spreading Social Media vs Education", fontsize = 9)
axes[1].legend(fontsize = 5, title = "Social Media", bbox_to_anchor = [1.1, 1])
axes[1].tick_params(labelsize = 4)

fig.suptitle("Comparison between prefrence of news and News medium about education", fontdict={"family":"serif", "color":"darkred", "size":10})
fig.tight_layout()
plt.savefig("Plot 22.jpg")

med_pan = df["soc_med_panic"].value_counts()
pqr = med_pan.sort_index()
pqr
med_pan_type = df[["soc_med_panic", "panic_type"]].value_counts()
uvw = pd.DataFrame(med_pan_type).sort_index()
uvw.index = ["Both", "None", "Physical", "Psychological", "Both", "None", "Physical", "Psychologial", 
             "Both", "None", "Psychologial", "Psychologial", "Both", "None", "Physical", "Psychological", "other",
            "Both", "None", "Physical", "Psychological"]

# plot 23

plt.figure(figsize = (10, 6), dpi = 150)
# 1st layer
plt.pie(uvw[0], labels = uvw.index, radius = 1+0.3, wedgeprops={'linewidth': 1.0, 'edgecolor': 'white'}, autopct = "%1.1f%%",
       colors = ["#D64748", "#C74445", "#E0282A", "#E83537",
                "#E62589", "#EB2A8E", "#F73B9D", "#AB0059",
                "#CC21BB", "#FF42EC", "#65126b",
                "#2C1B7D",
                "#237FA1", "#237FA1", "#97d8f0", "#cee7f0", "#6e8891",
                "#278A6A", "#11D495", "#82BDAA", "#094532"], textprops={'fontsize': 5}, pctdistance = 0.9)
# 2nd layer
plt.pie(pqr , autopct = '%1.1f%%', shadow = True, wedgeprops={'linewidth': 1.0, 'edgecolor': 'white'}, pctdistance = 0.8,
       colors = ["#B03E3F", "#AD3674", "#9D23A6", "#30298A", "#265769", "#2A6351"], textprops = {"fontsize":8})
# 3rd layer
circle = plt.Circle((0,0),radius = 1-0.5, color = "white")
plt.gca().add_patch(circle)

# add legend
colors = {"Don't know":'#B03E3F', "Facebook":"#AD3674", "Instagram":"#9D23A6", 'Twitter':'#30298A',
          "Whatsapp":'#265769', "Youtube":"#2A6351"}
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]
plt.legend(handles, labels, title = "Social Media", fontsize = 7, bbox_to_anchor = [0.95, 0.2])

plt.title("Social Media\nVs\nPanic Type", fontdict={"family":"serif", "color":"darkred", "size":14}, y = 0.43)
plt.savefig("Plot 23.jpg")

# Finished ╰(*°▽°*)╯
