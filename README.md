# Sas-projects

#SAS Healthcare Analysis Project
The analysis uses the Indian Pima diabetics dataset('diabetics.csv') which contains information about diabetics patients.

/* importing dataset*/

proc import datafile='/home/u63005412/diabetics/diabetes.csv' dbms=csv out=diabetics replace;
getnames=yes;
run;

/*Data exploration*/


/*summary statistics of numerical variables*/

proc means data=diabetics;
var Glucose BloodPressure SkinThickness Insulin BMI DiabetesPedigreeFunction Age;
run;
/*Here minimum cannot be zero for some parameters like glucose,bmi,etc so there will be missing values*/


/*Frequency disribution of categorical variable 'Outcome'*/

proc freq data=diabetics;
tables Outcome;
run;

/*Handling missing values*/



/* Replace missing values with median*/

proc stdize data=diabetics out=diabetics_clean missing=median;
   var  Glucose BloodPressure SkinThickness Insulin BMI DiabetesPedigreeFunction Age; 
run;

checking whether the missing values are replaced with median value.

proc means data=diabetics_clean;
var Glucose BloodPressure SkinThickness Insulin BMI DiabetesPedigreeFunction Age;
run;

/*model training*/

proc corr data=diabetics_clean;
var  Glucose BMI;
run;

 Proc logistic data=diabetics_clean;
 model Outcome(event='1')=Glucose BMI BloodPressure Age;
 run;
 
 /*Here target variable is 'Outcome'*/

/*Model Evaluation*/
Proc logistic data=diabetics_clean;
 model Outcome(event='1')=Glucose BMI BloodPressure Age/outroc=roc;
 run;
