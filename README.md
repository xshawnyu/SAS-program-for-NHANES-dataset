# SAS-program-for-NHANES-dataset
  Author: Xiang Yu  Date: March 25, 2017 stepwise investigation of organic exposure to health status
  Description: This program reads in person level measurement and Questionnaire data from the National Health and Nutrition                Examination (NHANES) Survey provided by the Center for Disease Control and Prevention (CDC) to analyze association between                thyroid disorder and selected synthetic agents. The program identifies important demographic covariates and synthetic agents associated with thyroid disease. Relationships between synthetic agents and thyroid disease are estimated individually in a series of logistic models,                controlling for the influence of demographic covariates.               
  PROGRAM STEPS 
  1. Set macro variables that define input and output libnames and file names               
  2. Read in NHANES SAS transport files                
  3  Recode and clean data; merge data; define study sample               
  4. Obtain descriptive statistics               
  5. Identify demographic control variables (based on Step 4)               
  6. Select synthetic agents of interest    (based on Step 4)               
  7. Build base model (control variables)               
  8. Estimate parameters for base model + one independent variable for each agent of interest (with thyroid disorder                  as the dependent variable)
  
  ********************************************
  
 STEP 2: Read in SAS transport files with macro %IMPORT *******************************************************************  macro %IMPORT      1. read in NHANES SAS transport data sat      2. create temporary SAS data set      3. sort by respondent ID (SEQN) to merge all data sets    INPUT   path     = location of SAS transport data   name     = name of SAS transport data   new_name = output temporary SAS data set ********************************************************************/
1
%macro IMPORT(path=, name=, new_name=);    
libname in xport "&PATH/&NAME..xpt";    
data &new_name.;  set in.&name.;    
run;    
proc sort data=&new_name.;  by SEQN;    run; 
%mend import; %import(path=&INPUT_DIR., name=DEMO_H,   new_name=demo); 
%import(path=&INPUT_DIR., name=MCQ_H,    new_name=outcomes); 
%import(path=&INPUT_DIR., name=SSPFAS_H, new_name=isomer); 
%import(path=&INPUT_DIR., name=PHTHTE_H, new_name=urine); 
%import(path=&INPUT_DIR., name=BMX_H,    new_name=BMI); 
  
