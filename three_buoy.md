# For calibrating the parameters in the task_buoy

1. for thresholding the parameters first make the threshold parameter and don't change the flag value otherwise you will be in trouble 
2. if you have got your values then chage the threshold param to false then you can change to other colors. 
3. `threshold` means to store the value of the changed parameters in the respective matrices which will then be used to save it. if it is not true then any change made to the parameters in rqt_reconfigure will not be saved
4. `done` parameter is for making the save parameter false 
5. `save` parameter is for dumping all the parameters in a yaml file
6. `flag` parameter is for switching to different buoys variabels
7.  the save parameter is always true because it is true at the time of saving the parameters and so it's always gets saved as true and if we reload the values of parameters then it is always true 
8. due to unknown working of the parameter server done parameter is of no use for now.

## So here are the steps for calibrating without getting into any trouble:

1. Change the `flag` parameter depending on which color you want to threshold
2. Change the `threhold` parameter to true then do the calibration, during this don't change the flag value
3. Make `threshold` to false again
4. When done save it by changing the `save` parameter to true and then making it false again  
5. Then you can repeat the above steps to calibrate other colors
