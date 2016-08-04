# gettingandcleaningdataproject
Final project for Getting and Cleaning Data on Coursera.org

This Readme walks through each step within the run_analysis.R code
Before running the R script, you must ensure that it is contained within the parent folder that also contains the Samsung phone sensor data

##load the dplyr package to utilize some of it's functions
library(dplyr)

##load necessary data from the top level folder
  ##load the features table into a data.frame
  features <- read.table("features.txt")
  
  ##create a vector of the 561 feature names.  these will be used to name columns later
  feature_names <- as.character(features[[2]])
  
  ##load the activity labels.  these will be joined with the data later
  activity_labels <- read.table("activity_labels.txt", col.names = c("label_id", "label"))

##load the test and training data
  ##load the test data set
  test_set <- read.table("./test/X_test.txt")
  ##name the variables with feature names
  names(test_set) <- feature_names
  
  ##load the test labels and apply the column name 'label_id'
  test_labels <- read.table("./test/y_test.txt", col.names = c("label_id"))
  
  ##load the training data set
  train_set <- read.table("./train/X_train.txt")
  ##name the variables with feature names
  names(train_set) <- feature_names
  
  ##load the training labels and apply the column name 'label_id'
  train_labels <- read.table("./train/y_train.txt", col.names = c("label_id"))

##add the activity lable names to test_labels and also train_labels
  ##update test_labels to add the activity labels
  test_labels <- inner_join(activity_labels, test_labels, by = "label_id")
  
  ##update train_labels to add the activity labels
  train_labels <- inner_join(activity_labels, train_labels, by = "label_id")
  
  ##update test_set to add the activity labels
  test_set  <- bind_cols(test_labels, test_set)
  
  ##update train_set to add the activity labels
  train_set <- bind_cols(train_labels, train_set)
  
##add the subject id numbers to each set
  ##read the subject test id numbers into a data.frame
  subject_test <- read.table("./test/subject_test.txt", col.names = c("subject"))
  
  ##read the subject train id numbers into a data.fram
  subject_train <- read.table("./train/subject_train.txt", col.names = c("subject"))
  
  ##add the subject id numbers to the test_set
  test_set <- bind_cols(subject_test, test_set)
  
  ##add the subject id numbers to the train_set
  train_set <- bind_cols(subject_train, train_set)
  
##produced combined data set
  ##bind all rows for the test and training data together
  combined_data <- bind_rows(test_set, train_set)

##write the combined_data to a .txt file
  write.table(combined_data, "tidy_data_set.txt", row.names = FALSE)
  
