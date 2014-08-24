Getting and Cleaning Data project submittal
=================

This README file was created to document and discuss the code created in order to complete the programming project assignment for the **Getting and Cleaning Data** course through Coursera.

This section of the code sets the working directory and points to where the unzipped raw data files are located.

```
#sets proper working directory
setwd("~/Coursera/Data Science/3-Getting and Cleaning Data/Project/getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset")
```

This section of the code brings all the separate downloaded .txt files from the original unzipped file into the Global Environment and stores them with logical names. This is done in order to manipulate the files later with additional code.

```
#this section pulls the necessary information from the files
#reads the column names and stores it to 'cols'
cols <- read.table("features.txt")
#reads the test data and stores it to 'test' with the proper column names
test <- read.table("./test/X_test.txt", col.names=cols[,2])
#reads the train data and stores it to 'train' with the proper column names
train <- read.table("./train/X_train.txt", col.names=cols[,2])
#read the test activity codes and stores it to 'test_act'
test_act <- read.table("./test/y_test.txt", col.names="activity_ID")
#read the train activity codes and stores it to 'test_act'
train_act <- read.table("./train/y_train.txt", col.names="activity_ID")
#read the test subject codes and stores it to 'subject_test'
subject_test <- read.table("./test/subject_test.txt", col.names="subject_ID")
#read the train subject codes and stores it to 'subject_train'
subject_train <- read.table("./train/subject_train.txt", col.names="subject_ID")
#read the activity label data and stores it to 'labels'
labels <- read.table("activity_labels.txt")
```

```{r}
##PROJECT STEP 1##
##Merges the training and the test sets to create one data set##
#combines the test and train data frames
table <- rbind(test,train)
#combines the test and train activity vectors
activities <- rbind(test_act,train_act)
#combines the test and train subject vectors
subjects <- rbind(subject_test,subject_train)
#combines the activity labels and the data
full_table <- cbind(subjects,activities,table)
```
