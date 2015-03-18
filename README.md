####Getting and Cleaning Data
#####1.Merges the training and the test sets to create one data set.
#####2.Extracts only the measurements on the mean and standard deviation for each measurement. 
#####3.Uses descriptive activity names to name the activities in the data set
#####4.Appropriately labels the data set with descriptive variable names. 
#####5.From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

```{r}
#Load & Merge & remove for x
x_train <- read.table("./train/X_train.txt")
x_test <- read.table("./test/X_test.txt")
x_data <- rbind(x_train, x_test)
rm (x_train)
rm (x_test)
```

```{r}
#Load & Merge & remove for  y
y_train <- read.table("train/y_train.txt")
y_test <- read.table("test/y_test.txt")
y_data <- rbind(y_train, y_test)
rm (y_train)
rm (y_test)
```

```{r}
#Load & Merge & remove for subject
subject_train <- read.table("train/subject_train.txt")
subject_test <- read.table("test/subject_test.txt")
subject_data <- rbind(subject_train, subject_test)
names(subject_data) <- "subject"
rm (subject_train)
rm (subject_test)
```

```{r}
#Load & Merge & remove for features
features <- read.table("features.txt")
sub_features <- grep("-(mean|std)\\(\\)", features[, 2])
x_data <- x_data[, sub_features]
names(x_data) <- features[sub_features, 2]
rm(features)
rm(sub_features)
```

```{r}
#Load & Merge activities
activities <- read.table("activity_labels.txt")
y_data[, 1] <- activities[y_data[, 1], 2]
names(y_data) <- "activity"
rm(activities)
```

```{r}
#Merge all data
all_data <- cbind(x_data, y_data, subject_data)
```

```{r}
#Average of each variable for each activity and each subject
avg_data <- ddply(all_data, .(subject, activity), function(x) colMeans(x[, 1:66]))
```

```{r}
#Creates a second independent tidy data set
write.table(avg_data, "tidy_data.txt", row.name=FALSE)
```
