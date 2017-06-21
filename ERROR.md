# Error related to segmentation fault

* In computing, a segmentation fault (often shortened to segfault) or access violation is a fault,
or failure condition, raised by hardware with
memory protection, notifying an
operating system (OS) the software has attempted to access a restricted area of memory (a memory access violation). 

* But how the hell do we know where the hell we are accessing the restricted area.

* It is a very difficult error to solve, different from its cousin syntax fault which will will indicate where is the problem.

* One way is to print something after every code block where there is a possibility of error.

* Then one can know after which code block there is problem.

* One precaution that one should take is that do not do anything with the subscribed image from the topic with the command ```newframe = cv_bridge::toCvShare(msg, "bgr8")->image```.
Here one should do anything with the newframe because anything changed to it will also change the raw ros image which we are getting from the topic ,which results into segmentation fault.

* That was the reason of 5 hours of irritation and anxiety.
