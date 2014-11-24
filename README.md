practica-clean


setwd("C:/r")
#load activity labels and features  
activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt") [,2]
activity <- read.table("./UCI HAR Dataset/activity_labels.txt", stringsAsFactors = FALSE)
features <- read.table("./UCI HAR Dataset/features.txt")

# Load and process X_test & y_test data.
xtest <- read.table("./UCI HAR Dataset/test/X_test.txt")
ytest <- read.table("./UCI HAR Dataset/test/y_test.txt")
subjecttest <- read.table("./UCI HAR Dataset/test/subject_test.txt")

# Load and process X_train & y_train data.
xtrain <- read.table("./UCI HAR Dataset/train/X_train.txt")
ytrain <- read.table("./UCI HAR Dataset/train/y_train.txt")
subjecttrain <- read.table("./UCI HAR Dataset/train/subject_train.txt")
#Give names
names(Xtest) = features
names(ytest) = c("Activity_ID")
names(subject_test) = "subject"

names(Xtrain) = features
names(ytrain) = c("Activity_ID")
names(subjecttrain) = "subject"

names(activity) <- c("activity_id", "activity_label")

#merge

test <- cbind(xtest, ytest, subjecttest) test <- cbind(subjecttest, xtest, ytest)
test <- cbind(as.data.table(subjecttest), ytest, Xtest)
train <- cbind(xtrain, ytrain,subjecttrain) train <- cbind(subjecttrain, xtrain, ytrain)
train<- cbind(as.data.table(subjecttrain), ytrain, Xtrain)

todo <- cbind(test,train)

samis <- merge(sami, activity, by = "activity_id")

#extract

extract_features <- grepl("mean|std", features)
Xtest = Xtest[,extract_features]
Xtrain = Xtrain[,extract_features]

#mean and std
?

meanstdcols <- matchcols(todo, with = c("mean\\(\\)|std\\(\\)"))
sami <- todo[, c("subject_id", "activity_id", meanstdcols)]


sami

#Colum names in labels

labels <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("Id", "Label"))



# creating the tidy data

dataList[[length(dataList) + 1L]] <- data.frame(subject_id = summaryDS[i, "subject_id"],
                                                activity = summaryDS[i, "activity"],
                                                domain =  ifelse(variables[[1]][2] == "time", "Time", "Frequency"),
                                                acceleration = ifelse(variables[[1]][4] == "body", "Body", "Gravity"),
                                                device = ifelse(variables[[1]][6] == "acc", "Accelerometer", "Gyroscope"),
                                                jerk = ifelse(variables[[1]][8] == "", "No", "Yes"),
                                                magnitude = ifelse(variables[[1]][8] == "", "No", "Yes"),
                                                statistic = ifelse(variables[[1]][12] == "mean", "Mean", "Standard Deviation"),
                                                axis = toupper(variables[[1]][14]),
                                                average = summaryDS[i, "average"])
}


#write tidy data

write.table(tidyData, "tidydata.txt", row.names = FALSE)

==============
