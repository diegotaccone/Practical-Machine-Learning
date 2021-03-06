# Practical-Machine-Learning
Course project

# LOADING PACKAGES
#library(dplyr)

#library(caret)

#library(rattle)



# LOADING THE DATA
#SET WORKING DIRECTORY

#setwd("~/Coursera/Practical Machine Learning (ML en R)/Proyecto Final")

#database = read.csv(file = "pml-training.csv")



# PREPARING THE DATA
#Counting missing data

#sapply(database, function(x) sum(is.na(x)))

#Drop variables with +5% of missing data, filling this variables would change the model's outcome

#database = subset(database, select = -c(X, max_roll_belt, user_name, cvtd_timestamp, max_picth_belt, new_window, min_roll_belt, min_pitch_belt, amplitude_roll_belt, amplitude_pitch_belt,
                                        var_total_accel_belt, avg_roll_belt, stddev_roll_belt, var_roll_belt, avg_pitch_belt, stddev_pitch_belt,
                                        var_pitch_belt, avg_yaw_belt, stddev_yaw_belt, var_yaw_belt, var_accel_arm, avg_roll_arm, stddev_roll_arm,
                                        var_roll_arm, avg_pitch_arm, stddev_pitch_arm, var_pitch_arm, avg_yaw_arm, stddev_yaw_arm, var_yaw_arm,
                                        max_roll_arm, max_picth_arm, max_yaw_arm, min_roll_arm, min_pitch_arm, min_yaw_arm, amplitude_roll_arm,
                                        amplitude_pitch_arm, amplitude_yaw_arm, max_roll_dumbbell, max_picth_dumbbell, min_roll_dumbbell, min_pitch_dumbbell,
                                        amplitude_roll_dumbbell, amplitude_pitch_dumbbell, var_accel_dumbbell, avg_roll_dumbbell, stddev_roll_dumbbell,
                                        var_roll_dumbbell, avg_pitch_dumbbell, stddev_pitch_dumbbell, var_pitch_dumbbell, avg_yaw_dumbbell,
                                        stddev_yaw_dumbbell,  max_roll_forearm, max_picth_forearm, min_roll_forearm, min_pitch_forearm,
                                        amplitude_roll_forearm, amplitude_roll_forearm, amplitude_pitch_forearm, var_accel_forearm, avg_roll_forearm,
                                        stddev_roll_forearm, var_roll_forearm, avg_pitch_forearm, stddev_pitch_forearm, var_pitch_forearm,
                                        avg_yaw_forearm, stddev_yaw_forearm, var_yaw_forearm, kurtosis_roll_belt, kurtosis_picth_belt, kurtosis_yaw_belt, 
                                        skewness_roll_belt, skewness_roll_belt.1, skewness_yaw_belt, kurtosis_roll_arm, kurtosis_picth_arm, 
                                        kurtosis_yaw_arm, skewness_roll_arm, skewness_pitch_arm, skewness_yaw_arm, kurtosis_roll_dumbbell, 
                                        kurtosis_picth_dumbbell, kurtosis_yaw_dumbbell, skewness_roll_dumbbell, skewness_pitch_dumbbell,
                                        skewness_yaw_dumbbell, kurtosis_roll_forearm, kurtosis_picth_forearm, kurtosis_yaw_forearm, skewness_roll_forearm,
                                        skewness_pitch_forearm, skewness_yaw_forearm, max_yaw_belt, min_yaw_belt, amplitude_yaw_belt, max_yaw_dumbbell,
                                        min_yaw_dumbbell, amplitude_yaw_dumbbell, max_yaw_forearm, min_yaw_forearm, amplitude_yaw_forearm,
                                        var_yaw_dumbbell))


# APLY PCA TO REDUCE DIMENSIONALITY AND KEEP MOST PART OF THE INFORMATION OF THE VARIABLES (COMPONENTS MUST HAVE 90+% OF THE DATA)
#database_correlation = subset(database, select = -c(classe))

#correlation = cor(database_correlation)

#VARIABLES WITH ABSOLUTE VALUE OF CORRELATION EQUAL OR BIGGER TO 0.8

#roll_belt, yaw_belt, total_accel_belt, accel_belt_y, accel_belt_z
#v1 = prcomp(database[c(4, 6, 7, 12, 13)]) 
#summary(v1) #Component 1-90%
#v1[2]
#database[,57] = database[,4]*0.42079160 + database[,6]*0.59154028 + database[,7]*0.05024761 + database[,12]*0.16805308 + database[,13]*-0.66501720

#pitch_belt, accel_belt_x, magnet_belt_x
#v2 = prcomp(database[c(5, 11, 14)]) 
#summary(v2) #Component 1-72%, 2-14%
#v2[2]
#database[,58] = database[,5]*-0.2851349 + database[,11]*0.3816465 + database[,14]*0.8792292
#database[,59] = database[,5]*-0.5242966 + database[,11]*0.7058034 + database[,14]*-0.4763975

#gyros_arm_x, gyros_arm_y
#v3 = prcomp(database[c(21, 22)])
#summary(v3) #Component 1-97%
#v3[2]
#database[,60] = database[,21]*0.9278154 + database[,22]*-0.3730396

#accel_arm_x, magnet_arm_x
#v4 = prcomp(database[c(24, 27)])
#summary(v4) #Component 1-95%
#v4[2]
#database[,61] = database[,24]*0.3320298 + database[,27]*0.9432689

#magnet_arm_y, magnet_arm_z
#v5 = prcomp(database[c(28, 29)])
#summary(v5) #Component 1-92%
#v5[2]
#database[,62] = database[,28]*-0.488373 + database[,29]*-0.872635

#yaw_dumbbell, accel_dumbbell_z
#v6 = prcomp(database[c(32, 39)])
#summary(v6) #Component 1-93%
#v6[2]
#database[,63] = database[,32]*0.5832650 + database[,39]*0.8122819

#gyros_dumbbell_x, gyros_dumbbell_z, gyros_forearm_z
#v7 = prcomp(database[c(34, 36, 49)])
#summary(v7) #Component 1-96%
#v7[2]
#database[,64] = database[,34]*0.4633751 + database[,36]*-0.7105826 + database[,49]*-0.5294864

#Drop variables that were transformed via PCA

#database = subset(database, select = -c(roll_belt, yaw_belt, total_accel_belt, accel_belt_y, accel_belt_z, pitch_belt, accel_belt_x, magnet_belt_x,
                                        gyros_arm_x, gyros_arm_y, accel_arm_x, magnet_arm_x, magnet_arm_y, magnet_arm_z, yaw_dumbbell, accel_dumbbell_z,
                                        gyros_dumbbell_x, gyros_dumbbell_z, gyros_forearm_z))



# 1ST MODEL: DECISION TREE (WE USE STANDARIZED DATA TO AVOID THE UNITS SIZE IMPACT)
#DIVIDE DATASET INTO TRAIN AND TEST SETS

#partition = createDataPartition(y=database[,1], p=0.75, list=FALSE)

#train = database[partition,]

#test = database[-partition,]


#TRAINING DECISION TREE WITH STANDARIZED DATA
#RTREE = train(classe ~., data=train, preProcess=c("center","scale"), method="rpart")

#PREDICTING WITH THE DECISION TREE MODEL
#test[,46] = predict(RTREE, test)

#OBTAINING MODELS ACCURACY (ACCURACY IS A GOOD METRIC BECAUSE THE CLASSE VARIABLE FROM THE DATASET IS BALANCED)
#countp=0

#for(i in 1:4904){
  if(test[i,37] == test[i,46]){
    countp = countp+1
  }
}

#accurcy = countp/4904

#EXPECTED OUTCOME
#DUE TO THE DATA PARTITION, AND THE MODEL BEHAVIOUR WITH DATA THAT THE MODEL HASN'T SEEN BEFORE WE EXPECT THE MODEL TO HAVE AN ACCURACY OF 0.76



# 2ND MODEL: RANDOM FOREST
#TRAINING RANDOM FOREST WITH STANDARIZED DATA
#RFOREST = train(classe ~., data=train, preProcess=c("center","scale"), method="rf")

#PREDICTING WITH THE DECISION TREE MODEL
#test[,47] = predict(RFOREST, test)

#OBTAINING MODELS ACCURACY (ACCURACY IS A GOOD METRIC BECAUSE THE CLASSE VARIABLE FROM THE DATASET IS BALANCED)
#countp=0

#for(i in 1:4904){
  if(test[i,37] == test[i,47]){
    countp = countp+1
  }
}

#accurcy = countp/4904

#EXPECTED OUTCOME
#DUE TO THE DATA PARTITION, AND THE MODEL BEHAVIOUR WITH DATA THAT THE MODEL HASN'T SEEN BEFORE WE EXPECT THE MODEL TO HAVE AN ACCURACY OF 0.99
