DATA PROCESSING 1.DOWNLOADING AND UNZIPPING DATASET:

if(!file.exists("./data")){dir.create("./data")} fileUrl &lt;-
"<https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip>"
download.file(fileUrl,destfile="./data/Dataset.zip")
unzip(zipfile="./data/Dataset.zip",exdir="./data")

2.MERGING THE TRAINING AND THE TEST SETS TO CREATE ONE SET:

x\_train &lt;- read.table("./data/UCI HAR Dataset/train/X\_train.txt")
y\_train &lt;- read.table("./data/UCI HAR Dataset/train/y\_train.txt")
subject\_train &lt;- read.table("./data/UCI HAR
Dataset/train/subject\_train.txt") x\_test &lt;- read.table("./data/UCI
HAR Dataset/test/X\_test.txt") y\_test &lt;- read.table("./data/UCI HAR
Dataset/test/y\_test.txt") subject\_test &lt;- read.table("./data/UCI
HAR Dataset/test/subject\_test.txt") features &lt;-
read.table('./data/UCI HAR Dataset/features.txt') activityLabels =
read.table('./data/UCI HAR Dataset/activity\_labels.txt')
colnames(x\_train) &lt;- features\[,2\] colnames(y\_train)
&lt;-"activityId" colnames(subject\_train) &lt;- "subjectId"
colnames(x\_test) &lt;- features\[,2\] colnames(y\_test) &lt;-
"activityId" colnames(subject\_test) &lt;- "subjectId"
colnames(activityLabels) &lt;- c('activityId','activityType') mrg\_train
&lt;- cbind(y\_train, subject\_train, x\_train) mrg\_test &lt;-
cbind(y\_test, subject\_test, x\_test) setAllInOne &lt;-
rbind(mrg\_train, mrg\_test)

1.  EXTRACTING ONLY THE MEASUREMENTS ON THE MEAN AND STANDARD DEVIATION

colNames &lt;- colnames(setAllInOne) mean\_and\_std &lt;-
(grepl("activityId" , colNames) | grepl("subjectId" , colNames) |
grepl("mean.." , colNames) | grepl("std.." , colNames) )
setForMeanAndStd &lt;- setAllInOne\[ , mean\_and\_std == TRUE\]

1.  USING DESCRIPTIRVE ACTIVITY NAMES TO NAME THE ACTIVITIES:

setWithActivityNames &lt;- merge(setForMeanAndStd, activityLabels,
by='activityId', all.x=TRUE)

1.  CREATE THE NEW TIDY SET

secTidySet &lt;- aggregate(. ~subjectId + activityId,
setWithActivityNames, mean) secTidySet &lt;-
secTidySet\[order(secTidySet*s**u**b**j**e**c**t**I**d*, *s**e**c**T**i**d**y**S**e**t*activityId),\]
write.table(secTidySet, "secTidySet.txt", row.name=FALSE)
