# 3rd_imdc_lncc_d-fense_lncc_ARp26_chikungunya
LNCC ARp26 chikungunya prediction model


Team and Contributors

D-FENSE

Americo Cunha Jr (team leader) (LNCC - UERJ)

Paulo Esquef (LNCC)

Emanuelle Paixão (LNCC)


2. Repository Structure

Main
|_>  Aggredated Data

     (Data used to train the model)
     
|_>  ARp26_CG  (model source and results)

     |_> validation1    (tranining for validation 1) 
     
         |_> matlab    (matlab scritps to generate the results)
         
         |_> spreadsheets  (CSV files with the results; one for each state)
         
         |_> plots   (PDF files with plots of the results; one for each state)
         
     |_> validation2  (see validation 1)
     
         |_> matlab
         
         |_> spreadsheets
         
         |_> plots
         
     |_> validation3 (see validation 1)
     
         |_> matlab
         
         |_> spreadsheets
         
         |_> plots
         
     |_> validation4  (see validation 1)
     
         |_> matlab
         
         |_> spreadsheets
         
         |_> plots
         
         
3. Libraries and Dependencies

Proprietary functions of Matlab toolboxes:
armcov.m   % matlab function for AR(p) model estimation
filter.m  % matlab function for linear convolution
randn.m  % matlab function for random number generation from a Normal Distribution
prctile.m % matlab function for calculating the percentiles of a set of numbers
table.m  % matlab function for managing table creation
writetable.m % matlab function for creating a CSV file from a table

Extra function:
ssa_modPE.m  % Singular Spectrum Analysis method used for lowpass filtering the data (included in folder matlab)

4. Data and Variables
For each state, the input time series aggregates the sum of all dengue cases over a epiweek, in a given state. 
Data is organized in 27 CSV files, one per state, stored in folder 'Aggregated Data'.
Only the time series of dengue cases has been used. No climate data has been used.
We only used chikungunya cases from EW 01 of 2021 to EW 25 (cutoff year, depending on the validation target). This choice was made because,
for several states, the number of cases before year 2021 is too low (absence of a substantial surge), which impairs the model training.
  
6. Model Training

Model training only invoved regular estimation of an AR(p) parameters, based on the zero-mean log2 transformation of the original time series of dengue cases.  
For validation 1: from EW 1 of 2021 to EW 25 of 2022 
For validation 2: from EW 1 of 2021 to EW 25 of 2023 
For validation 3: from EW 1 of 2021 to EW 25 of 2024 
For validation 4: from EW 1 of 2021 to EW 25 of 2025 

The model order varied according to the validation target, i.e.,  

order p = floor( half of data length) -1;

CODE (example for validation 1) : 
cc=kcc(1:ind_v1);  % observed cases from EW 1 of 2010 to EW 25 of 2022
cclog=log2(cc); % apply a log2 function to the observed raw data 
mcc=mean(cclog); % calculate and save the mean of cclog
sig=cclog-mcc;  % zero-mean log2 observed data
Lsig=length(sig);   % length of the available data for training

% Represents oscillations in sig via p-order AR(p) model 
p=floor(Lsig/2)-1; % model order
a=armcov(sig,p);  % AR model estimation via the modified covariance method

5. Data Usage Restriction

After estimating the model parameters, we run the model, i.e., excite the model with appropriate randonly generated signals with length which is long enough to provide forecasts
from EW 26 of year 202X up to EW 52 of year 202(X+1). We then lowpass filter the attained reaults and, finally, crop the forecasts to comply with the specified range from EW 41 
of year 202X up to EW 40 of year 202(X+1). 

6. Predictive Uncertainty How are your prediction intervals computed?
We have carriedout a Monte Carlo simulation with 10000 runs, for each state. Out of the 10000 forecast realizations, for a given time instant, we calculate the specified percentiles (via prctile.m),
 produce the specified prediction intervals. 


8. References
  None. 
