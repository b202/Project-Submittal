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

This section of the code creates one master data set call 'full_table' by combining the test and train data, activity IDs and the subject IDs.
```
##PROJECT STEP 1
##Merges the training and the test sets to create one data set
#combines the test and train data frames
table <- rbind(test,train)
#combines the test and train activity vectors
activities <- rbind(test_act,train_act)
#combines the test and train subject vectors
subjects <- rbind(subject_test,subject_train)
#combines the activity labels and the data
full_table <- cbind(subjects,activities,table)
```

This section of the code creates the final data set of only the mean and standard deviation variables by searching for the terms 'mean()' and 'std()' in all of the variable names and creating a vector called 'positions'. The 'positions' vector is used to indicate the positions in the full table where the mean and standard deviation variable are. A smaller data frame called 'sub_table' is created of just the mean and standard deviation variables.
```
##PROJECT STEP 2
##Extracts only the measurements on the mean and standard deviation for each
## measurement
#this section creates a sorted vector of mean and std locations
#creates vector of means locations from 2nd column of cols data.frame
means_pos <- grep("mean()", cols[,2], fixed=TRUE)
#creates vector of std locations from 2nd column of cols data.frame
std_pos <- grep("std()", cols[,2], fixed=TRUE)
#combines the means_pos and std_pos vectors and sorts the result
positions <- sort(as.vector(rbind(means_pos, std_pos)))

#this section creates a smaller table of just the mean and std records
sub_table <- full_table[,positions]
```

This section
```
##PROJECT STEP 3
##Uses descriptive activity names to name the activities in the data set
#creates a vector of the activities based on the values in the activity_ID
# column of the sub_table data frame and converts them to a descriptive
# variable per the labels data frame
desc_activity <- as.vector(labels[as.numeric(sub_table$activity_ID),2])
#reassigns the descriptive variable to the activity_ID column
sub_table$activity_ID <- desc_activity
```

This section
```
##PROJECT STEP 4
##Appropriately labels the data set with descriptive variable names
#The data frames have descriptive variable names already
```

This section
```
##PROJECT STEP 5
##Creates a second, independent tidy data set with the average of each variable
## for each activity and each subject
#creates a new data frame of tidy data based on the sub_table data that contains
# the mean or average of the set of values for each variable according to the
# subject ID and the activity typw
agg_table <- aggregate(. ~ subject_ID + activity_ID, data=sub_table, FUN=mean)
#creates a text file called 'final.txt' from agg_table
write.table(agg_table, file="final.txt", row.names=FALSE)
```
