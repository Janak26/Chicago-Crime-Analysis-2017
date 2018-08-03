import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

data = pd.read_csv("D:/ML_Projects/Chicago_crime_analysis/Crimes_-_2017.csv")

# Separate categorical to int
data = pd.get_dummies(data, columns=["Arrest", "Domestic"])
data.drop(['Arrest_False', 'Domestic_False'], axis=1, inplace=True)

# Convert date into a 24 hour format
data['Date'] = pd.to_datetime(data['Date'], format='%m/%d/%Y %I:%M:%S %p')

# Separate Date and Time
data['Date'] = data['Date'].astype(str)
data[['Date', 'Time']] = data['Date'].str.split(" ", expand=True)

# Create day of week from corresponding date
data['Date'] = pd.to_datetime(data['Date'])
data['Dayofweek'] = data['Date'].dt.weekday_name

# Separate Security code and block
data[['Security Code', 'Block']] = data['Block'].str.split(" ", 1, expand=True)
# print(data['Security Code'])
# print(data['Block'])

# Fill blankspace in Location Description with NaN
data['Location Description'] = data['Location Description'].fillna('UNAVAILABLE')
# print(data['Location Description'])
# print(data.info())

# Fill blankspace in X Coordinate and Y Coordinate with NaN
data[['X Coordinate', 'Y Coordinate']] = data[['X Coordinate', 'Y Coordinate']].fillna("NaN")
# print(data.info())

data2 = data.copy()
data2['Time'] = data['Time'].astype(str)
data2[['Hours', 'Minutes', 'Seconds']] = data2['Time'].str.split(':', expand=True)
# print(data2.columns)
# print(data2['Hours'])


# Plot counts of time in hours
sns.countplot(x='Hours', data=data2)
plt.title('Most crimes with respect to time of the day')
plt.xlabel('Time of the day')
plt.ylabel('No. of crimes')
plt.show()

# Indicats the crimes occures at 12 pm
hourly_crime = data2.loc[data2['Hours'] == '12', 'Primary Type']
sns.countplot(y=hourly_crime, data=data2)
plt.show()

# Plot counts of Dayofweek
sns.countplot(x='Dayofweek', data=data)
plt.title('Number of crimes according to day of the week')
plt.xlabel('Day of week')
plt.ylabel('No. of crimes')
plt.show()


# Plot Primary Type vs count
ax00 = sns.countplot(y='Primary Type', data=data, order=data['Primary Type'].value_counts().iloc[:30].index)
ax00.figure.subplots_adjust(left=0.25)
plt.title('Identifying Types of Crimes')
plt.xlabel('No. of crimes')
plt.ylabel('Type of Crime')
plt.show()

# Plot bolck with max number of crimes
ax01 = sns.countplot(y='Block', data=data, order=data.Block.value_counts().iloc[:25].index)
ax01.figure.subplots_adjust(left=0.25)
plt.title('Block with most number of crimes')
plt.xlabel('No. of crimes')
plt.ylabel('Block name')
plt.show()



# Plot Descriptions vs count
sns.countplot(y='Description', data=data, order=data.Description.value_counts().iloc[:40].index)
plt.show()

# Count the primary type
Primary_type_count = data['Primary Type'].value_counts()
print('Typesof Primary Crimes and their count')
print(Primary_type_count)

# Count max no of description
description_count = data['Description'].value_counts().iloc[:20]
print('Description of crimes')
print(description_count)


# EXPLORING THEFT

# Plot description of theft and its count
theft_description = data.loc[data['Primary Type'] == 'THEFT', 'Description']
ax0 = sns.countplot(y=theft_description, data=data, color='red')
ax0.figure.subplots_adjust(left=0.22)
plt.title('Description of thefts')
plt.xlabel('No. of thefts')
plt.ylabel('Description')
plt.show()

# Plot Thefts wrt location
theft_location = data.loc[data['Primary Type'] == 'THEFT', 'Location Description']
ax1 = sns.countplot(y=theft_location, data=data, color='red', order=theft_location.value_counts().iloc[:30].index)
ax1.figure.subplots_adjust(left=0.22)
plt.title('Location with most theft cases')
plt.xlabel('No. of theft cases')
plt.ylabel('Location of thefts')
plt.show()

# Blocks with most theft cases
theft_block = data.loc[data['Primary Type'] == 'THEFT', 'Block']
ax2 = sns.countplot(y=theft_block, data=data, color='red', order=theft_block.value_counts().iloc[:30].index)
ax2.figure.subplots_adjust(left=0.2)
plt.title('Blocks with most theft cases')
plt.xlabel('No. of theft cases')
plt.ylabel('Blocks')
plt.show()


# Theft arrests
theft_arrest = data.loc[data['Primary Type'] == 'THEFT', 'Arrest_True']
ax3 = sns.countplot(x=theft_arrest, data=data, color='red')
plt.title('Arrested for theft')
plt.xlabel('Arrested')
plt.ylabel('No. of Arrests')
plt.show()


# Explore Criminal Damage

# Plot description of Criminal Damage and its count
criminal_damage_description = data.loc[data['Primary Type'] == 'CRIMINAL DAMAGE', 'Description']
ax4= sns.countplot(y=criminal_damage_description, color='blue', data=data)
ax4.figure.subplots_adjust(left=0.2)
plt.title('Criminal Damage caused to:')
plt.xlabel('No. of Damages')
plt.ylabel('Damage caused to')
plt.show()

# Plot criminal damage location
criminal_damage_location = data.loc[data['Primary Type'] == 'CRIMINAL DAMAGE', 'Location Description']
ax5 = sns.countplot(y=criminal_damage_location, data=data, color='blue', order=criminal_damage_location.value_counts().iloc[:30].index)
ax5.figure.subplots_adjust(left=0.25)
plt.title('Criminal Damage Location')
plt.xlabel('No.of Criminal Damages')
plt.ylabel('Location')
plt.show()

# Blocks wit most criminal damages
criminal_damage_block = data.loc[data['Primary Type'] == 'CRIMINAL DAMAGE', 'Block']
ax6 = sns.countplot(y=criminal_damage_block, data=data, color='blue', order=criminal_damage_block.value_counts().iloc[:30].index)
ax6.figure.subplots_adjust(left=0.2)
plt.title('Blocks with most Criminal Damages')
plt.xlabel('No. of Criminal Damages')
plt.ylabel('Blocks')
plt.show()

# Criminal damage arrests
criminal_damage_arrest = data.loc[data['Primary Type'] == 'CRIMINAL DAMAGE', 'Arrest_True']
ax7 = sns.countplot(x=criminal_damage_arrest, data=data)
plt.title('Arrests for Criminal Damage')
plt.xlabel('Arrested')
plt.ylabel('No. of Arrests')
plt.show()


# DECEPTIVE PRACTICE

# Types of deceptive practices
deceptive_pract_description = data.loc[data['Primary Type'] == 'DECEPTIVE PRACTICE', 'Description']
ax81 = sns.countplot(y=deceptive_pract_description, data=data, color='yellow', order=deceptive_pract_description.value_counts().iloc[:].index)
ax81.figure.subplots_adjust(left=0.27)
plt.title('Types for Deceptive practice')
plt.ylabel('Types of deceptive practice ')
plt.xlabel('No. of Deceptive Practices')
plt.show()

# Explore no of people arrested for deceptive practice
deceptive_pract_arrest = data.loc[data['Primary Type'] == 'DECEPTIVE PRACTICE', 'Arrest_True']
ax8 = sns.countplot(x=deceptive_pract_arrest, data=data, color='yellow')
plt.title('Arrests for Deceptive practice')
plt.xlabel('Arrested')
plt.ylabel('No. of Arrests')
plt.show()


# Homicide

# # Homicide description
# homicide_description = data.loc[data['Primary Type'] == 'HOMICIDE', 'Description']
# ax9 = sns.countplot(x=homicide_description, data=data)
# plt.show()


# Location of homicide
homicide_location = data.loc[data['Primary Type'] == 'HOMICIDE', 'Location Description']
ax10 = sns.countplot(y=homicide_location, data=data, color='maroon', order=homicide_location.value_counts().iloc[:30].index)
ax10.figure.subplots_adjust(left=0.2)
plt.title('Location of Homicides')
plt.xlabel('No. of Homicides')
plt.ylabel('Location of Homicides')
plt.show()

# Homicide wrt time of day
homicide_time = data2.loc[data2['Primary Type'] == 'HOMICIDE', 'Hours']
ax11 = sns.countplot(x=homicide_time, data=data2, color='maroon')
plt.title('Homicides wrt time of the day')
plt.xlabel('Hours of the day')
plt.ylabel('No of Homicides')
plt.show()


# Homicide wrt Domestic
homicide_domestic = data.loc[data['Primary Type'] == 'HOMICIDE', 'Domestic_True']
ax12 = sns.countplot(x=homicide_domestic, data=data,color='maroon')
plt.title('No. of Domestic Homicides')
plt.xlabel('Domestic vs Non Domestic')
plt.ylabel('No of Homicides')
plt.show()


# Explore narcotics

# Narcotics Description
narcotics_description = data.loc[data['Primary Type'] == 'NARCOTICS', 'Description']
ax13 = sns.countplot(y=narcotics_description, data=data, color='orange', order=narcotics_description.value_counts().iloc[:30].index)
ax13.figure.subplots_adjust(left=0.25)
plt.title('Types of Narcotics cases')
plt.xlabel('No. of Narcotics cases')
plt.ylabel('Description of Narcotics case')
plt.show()

# Narcotics Day of week
narcotics_dayofweek = data.loc[data2['Primary Type'] == 'NARCOTICS', 'Dayofweek']
ax14 = sns.countplot(y=narcotics_dayofweek, data=data2)
ax14.figure.subplots_adjust(left=0.1)
plt.title('Narcotics cases wrt day of the week')
plt.xlabel('No. of Narcotics cases')
plt.ylabel('Day of the week')
plt.show()

# Narcotics Time of the day
narcotics_timeofday = data2.loc[data2['Primary Type'] == 'NARCOTICS', 'Hours']
ax15 = sns.countplot(y=narcotics_timeofday, color='orange', data=data2)
ax15.figure.subplots_adjust(left=0.1)
plt.title('Narcotics cases wrt Time of the Day')
plt.xlabel('No. of Narcotics cases')
plt.ylabel('Time of the day in Hours')
plt.show()

# Narcotics location
narcotics_location = data.loc[data['Primary Type'] == 'NARCOTICS', 'Location Description']
ax16 = sns.countplot(y=narcotics_location, data=data, color='orange', order=narcotics_location.value_counts().iloc[:30].index)
ax16.figure.subplots_adjust(left=0.25)
plt.title('Location with most cases of Narcotics')
plt.xlabel('No of Narcotics cases')
plt.ylabel('Location')
plt.show()

# Narcotics block
narcotics_block = data.loc[data['Primary Type'] == 'NARCOTICS', 'Block']
ax17 = sns.countplot(y=narcotics_block, data=data, color='orange', order=narcotics_block.value_counts().iloc[:30].index)
ax17.figure.subplots_adjust(left=0.2)
plt.title('Block with most cases of Narcotics')
plt.xlabel('No. of Narcotics cases')
plt.ylabel('Block Name')
plt.show()


# Prostitution

# blocks with most prostitution
prostitution_block = data.loc[data['Primary Type'] == 'PROSTITUTION', 'Block']
ax18 = sns.countplot(y=prostitution_block, data=data, color='red',  order=prostitution_block.value_counts().iloc[:30].index)
ax18.figure.subplots_adjust(left=0.2)
plt.title('Blocks with most prostitution cases')
plt.xlabel('No. of cases')
plt.ylabel('Block Name')
plt.show()

# location with most prostution
prostitution_location = data.loc[data['Primary Type'] == 'PROSTITUTION', 'Location Description']
ax181 = sns.countplot(y=prostitution_location, data=data, color='red',  order=prostitution_location.value_counts().iloc[:30].index)
ax181.figure.subplots_adjust(left=0.2)
plt.title('Location with most prostitution cases')
plt.xlabel('No. of cases')
plt.ylabel('Location')
plt.show()

# Prostitution arrest
prostitution_arrest = data.loc[data['Primary Type'] == 'PROSTITUTION', 'Arrest_True']
ax182 = sns.countplot(x=prostitution_arrest, data=data, color='red')
plt.title('Arrests for Criminal Damage')
plt.xlabel('Arrested')
plt.ylabel('No. of Arrests')
plt.show()


# Other offence

# Other offence description
otheroffense_description = data.loc[data['Primary Type'] == 'OTHER OFFENSE', 'Description']
ax19 = sns.countplot(y=otheroffense_description, data=data, color='red', order=otheroffense_description.value_counts().iloc[:20].index)
ax19.set_yticklabels(ax19.get_yticklabels())
ax19.figure.subplots_adjust(left=0.3)
plt.title('Counts of Types of Other Offenses')
plt.xlabel('Counts')
plt.ylabel('Description of Other Offences')
plt.show()

