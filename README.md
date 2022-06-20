# Name: Satyam mishra

# Task :5 Exploratory Data Analysis - Sports

# GRIP@TheSparkFoundation- Data Science and Buisness Analytics- june2022

# Objective

● Perform ‘Exploratory Data Analysis’ on dataset 'Indian Premier League' 

● As a sport analysis 'find out the most successful teams' players and factors contributing win or loss of a team.

●Suggest teams or players a company should endors for its product.  

# Import all required libraries

import numpy as np 
import pandas as pd 
import matplotlib.pyplot as plt
import seaborn as sns

# Reading the Data

deliveries=pd.read_csv("C:/csvfile/deliveries.csv")
matches=pd.read_csv("C:/csvfile/matches.csv")
print("Data Import Successfuly")

deliveries.head(5)

matches.head(5)

# Check null values of both data set

deliveries.isnull().sum()

matches.isnull().sum()

# Check the length of both dataset

print(len(deliveries))
print(len(matches))

# Preprocessing the data

deliveries=deliveries.drop(["player_dismissed","dismissal_kind","fielder"],axis=1)
deliveries.isnull().sum()

match=matches.drop(['umpire3'],axis=1)
matches=match.dropna(subset=['winner','city','umpire1','umpire2'])
matches.isnull().sum()

# Now we check most number of win teams

winners=pd.DataFrame({"Winner" : matches['winner']})
win=winners.value_counts()
labels=[x[0] for x in win.keys()]
bar,ax=plt.subplots(figsize=(20,12))
ax=plt.pie(x=win,autopct='%.1f%%',labels=labels)
plt.title('Most number of Wins',fontsize=20,color="blue")
plt.show()

plt.figure(figsize =(35,15))
plt.bar(labels,win)
plt.show()

# Toss Results

teams=matches['toss_winner'].unique()
decision_making=pd.DataFrame([],columns=['Toss Winner','Decision','Times'])
for id,element in enumerate(teams):
    bat=matches[(matches['toss_winner']==element) & (matches['toss_decision']=='bat')]
    field=matches[(matches['toss_winner']==element) & (matches['toss_decision']=='field')]

    decision_making=decision_making.append({'Toss Winner':element,'Decision':'bat','Times':bat['toss_winner'].count()},
                                           ignore_index=True)
    decision_making=decision_making.append({'Toss Winner':element,'Decision':'field','Times':bat['toss_winner'].count()},
                                           ignore_index=True)
decision_making

sns.catplot(x='Toss Winner',y='Times',hue='Decision',data=decision_making,kind='bar',height=5,aspect=2)
plt.xticks(rotation=60)
plt.title('Toss Decision by different teams',fontsize=20,color="green",)
plt.xlabel('Teams',fontsize=15,color="blue")
plt.ylabel('Toss Decision',fontsize=15,color="blue")
plt.show()

# Top 10 Successful Players in IPL

sns.barplot(x=matches['player_of_match'].value_counts().head(10).values,
            y=matches['player_of_match'].value_counts().head(10).index,data=matches)
plt.title('Top 10 Players',fontsize=20,color="green")
plt.xlabel('No of Main Of the Match Awards',fontsize=15,color="red")
plt.ylabel('Players',fontsize=15,color="red")
plt.show()

# Top 10 Famous Venues

sns.barplot(x=matches['venue'].value_counts().head(10).values,y=matches['venue'].value_counts().head(10).index,data=matches)
plt.title('Famous Venues',fontsize=20,color="green")
plt.xlabel('Venue Count',fontsize=15,color="red")
plt.ylabel('Venue',fontsize=15,color="red")
plt.show()

# Top Five 1st Umpires

sns.barplot(x=matches['umpire1'].value_counts().head().index,y=matches['umpire1'].value_counts().head().values,data=matches)
plt.xticks(rotation=90)
plt.title('Top 5 First Umpires',fontsize=20,color="green")
plt.xlabel('Umpires',fontsize=15,color="red")
plt.ylabel('Match Count',fontsize=15,color="red")
plt.show()

# Top Five 2nd Umpires

sns.barplot(x=matches['umpire2'].value_counts().head().index,y=matches['umpire2'].value_counts().head().values,data=matches)
plt.xticks(rotation=90)
plt.title('Top 5 First Umpires',fontsize=20,color="green")
plt.xlabel('Umpires',fontsize=15,color="red")
plt.ylabel('Match Count',fontsize=15,color="red")
plt.show()

# Best 10 Players for Companies 'Endorsement'

matches['player_of_match'].value_counts().head(10).index

##### Result: 1.CH Gayle, 2.AB de Villiers,3. RG Sharma, 4.DA Warner, 5.MS Dhoni,6.YK Pathan, 7.SR Watson, 8.SK Raina, 9.G Gambhir, 10.MEK Hussey

# Best 3 Teams for companies 'Endorsement'

matches['winner'].value_counts().head(3).index

#### Result: 1.Mumbai Indians, 2.Chennai Super Kings, 3.Kolkata Knight Riders.

# Now we check no-ball and wide-ball runs

test=deliveries['bowling_team'].unique()
extra_runs=pd.DataFrame([],columns=['Bowling Team','noball_runs','wide_runs'])
for id,element in enumerate(test):
    noball_runs=deliveries[(deliveries['bowling_team']==element) & (deliveries['noball_runs']>0)]
    wide_runs=deliveries[(deliveries['bowling_team']==element) & (deliveries['wide_runs']>0)]
    extra_runs=extra_runs.append({'Bowling Team':element,'noball_runs':noball_runs['bowling_team'].count(),'wide_runs':
    wide_runs['bowling_team'].count()},ignore_index=True)
extra_runs

# Plot the Graph for no-ball runs by different Teams

plt.bar(extra_runs['Bowling Team'],extra_runs['noball_runs'])
plt.xticks(rotation=70)
plt.title('Runs conceded through No-Balls by different teams',color="blue",fontsize=20)
plt.xlabel('Teams',color="red",fontsize=16)
plt.ylabel('Runs',color="red",fontsize=16)
plt.show()

# Plot the Graph for wide-ball runs by different Teams

plt.bar(extra_runs['Bowling Team'],extra_runs['wide_runs'])
plt.xticks(rotation=70)
plt.title('Runs conceded through wide-Balls by different teams',color="blue",fontsize=20)
plt.xlabel('Teams',color="red",fontsize=16)
plt.ylabel('Runs',color="red",fontsize=16)
plt.show()

# Thankyou
