Необхідно створити один R-скрипт, який називається run_analysis.R, який виконує наступні дії.

```r
install.packages("dplyr") 
library(dplyr)
```
```r
features_ <- read.table('/Users/m.oberemok/Downloads/UCI HAR Dataset/features.txt', colClasses = "character")[,2]
```

```r
#train
x_train <- read.table('/Users/m.oberemok/Downloads/UCI HAR Dataset/train/X_train.txt', col.names = features_, check.names = FALSE)
y_train <- read.table('/Users/m.oberemok/Downloads/UCI HAR Dataset/train/y_train.txt', col.names = c('Activity'))
subject_train <- read.table('/Users/m.oberemok/Downloads/UCI HAR Dataset/train/subject_train.txt', col.names = c('Subject'))
#test
x_test <- read.table('/Users/m.oberemok/Downloads/UCI HAR Dataset/test/X_test.txt', col.names = features_, check.names = FALSE)
y_test <- read.table('/Users/m.oberemok/Downloads/UCI HAR Dataset/test/y_test.txt', col.names = c('Activity'))
subject_test <- read.table('/Users/m.oberemok/Downloads/UCI HAR Dataset/test/subject_test.txt', col.names = c('Subject'))
```

```r
activity_labels <- read.table('/Users/m.oberemok/Downloads/UCI HAR Dataset/activity_labels.txt', col.names = c('n','text'))
```

1. Об’єднує навчальний та тестовий набори, щоб створити один набір даних.
```r
x <- rbind(x_train,x_test)
y <- rbind(y_train,y_test)
subj <- rbind(subject_test,subject_train)

data_all <- cbind(x,y,subj)
```

2. Витягує лише вимірювання середнього значення та стандартного відхилення (mean and standard deviation) для кожного вимірювання.
```r
no_dup <- data_all[, !duplicated(colnames(data_all))]

mean_and_std <- select(no_dup, matches("mean\\(\\)|std\\(\\)|Subject|Activity"))
```


3-4.Використовує описові назви діяльностей (activity) для найменування діяльностей у наборі даних.
Відповідно присвоює змінним у наборі даних описові імена.
```r
activ_def<-within(mean_and_std , Activity <- factor(Activity, labels = activity_labels[,2]))
```
5. З набору даних з кроку 4 створити другий незалежний акуратний набір даних (tidy dataset) із середнім значенням для кожної змінної для кожної діяльності та кожного суб’єкту (subject).
```r
final <- aggregate(x = activ_def[, -c(67,68)], by = list(activ_def[,'Subject'], activ_def[, 'Activity']), FUN = mean)

final<-rename(final,Subject=Group.1, Position.Measurement=Group.2)
write.csv(final , "tidy_dataset.csv", row.names=F)
```
