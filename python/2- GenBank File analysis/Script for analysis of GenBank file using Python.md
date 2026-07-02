**# To import libraries**



import glob

from Bio import SeqIO

from collections import Counter

import pandas as pd

import os

&#x20;    

**# create path to access downloaded GenBank files in system**



file\_dir='/home/vbox\_indu/Desktop'



**# To find and extract the GenBank files**



gfiles=glob.glob('%s/\*.gb'%file\_dir)

gfiles



**# To read GenBank files and get the features in each GenBank file**



def read\_file(gfile):

&#x20;   genbank\_obj=SeqIO.read(gfile,'gb')

&#x20;   features=genbank\_obj.features

&#x20;   feature\_types=\[feature.type for feature in features]

&#x20;   return feature\_types



**# To separately keep count of the no of types of features found in each GenBank file** 



def count\_features(feature\_types):

&#x20;   feature\_count=Counter(feature\_types)

&#x20;   print('features have been counted')

&#x20;   return feature\_count



**# to identify features in GenBank files, set it to remove duplicate info and create a list of the features in each** 



def scan\_all\_features(files):

&#x20;   allfeatures=\[]

&#x20;   for gfile in gfiles:

&#x20;       feature\_types=read\_file(gfile)

&#x20;       allfeatures.extend(feature\_types)

&#x20;   allfeatures=set(allfeatures)

&#x20;   allfeatures=list(allfeatures)

&#x20;   print('all features have been identified')

&#x20;   return allfeatures



**# Get all features are stored under name allfeatures**



allfeatures=scan\_all\_features(gfiles)

all features have been identified

print(allfeatures)



**# Create a empty list to store the counts of feature**



allfeature\_count=\[]



**# Loop to iterate over files and count the features**

&#x20;

for gfile in gfiles:

&#x20;   dir,filename=os.path.split(gfile)

&#x20;   filename=filename.strip('.gb')

&#x20;   feature\_types=read\_file(gfile)

&#x20;   feature\_count=count\_features(feature\_types)

&#x20;   temp\_count=\[]

&#x20;   temp\_count.append(filename)

&#x20;   for feature in allfeatures:

&#x20;       if feature in feature\_count.keys():

&#x20;          temp\_count.append(feature\_count\[feature])

&#x20;       else:

&#x20;          temp\_count.append(0)

&#x20;   allfeature\_count.append(temp\_count)



**#Function to find the length/ no. of different entries ( since 5 files are scanned length will be 5)**



len(allfeature\_count) 



**# Function to report the features of 1st file in GenBank files**



allfeature\_count\[0]

\['AR465', 59, 2934, 5, 1, 4, 13, 3, 19, 1, 2852]



**#Function to get all type of Features**



>>> print(allfeatures)

\['tRNA', 'gene', 'misc\_feature', 'source', 'misc\_binding', 'regulatory', 'ncRNA', 'rRNA', 'tmRNA', 'CDS']



**# Create columns for data frame (created to arrange the extracted info in tabulated form)**



columns=\[]

columns.append('File')

columns.extend(allfeatures)

columns



**# Create Data frame using Pandas**



dataframe=pd.DataFrame(allfeature\_count,columns=columns)

print(dataframe)



**# Set index to File ( a structured table with columns with features and row with GenBank file name formed)**



finaldata=dataframe.set\_index('File')

print(finaldata)



**# Function to delete a particular column from the table**



columns\_to\_del=\['source']

finaldata.drop(columns=columns\_to\_del,inplace=True)

finaldata



**# Function to use place particular query eg- to find particular info about V521 file**



finaldata.loc\['V521',:]

finaldata.loc\['V521','gene']



**#save the data frame in an output file and define output file name**



outputfile='/home/vbox\_indu/Desktop/featurecount.csv'

>>> finaldata.to\_csv(outputfile,index=True)



**# final output file named feature\_count.csv saved in the repository**

