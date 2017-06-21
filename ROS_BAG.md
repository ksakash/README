# HOW TO RECORD AND REPLAY BAG FILES
 
1. Make a directory for storing the bag files ```mkdir ~/bagfiles```
2. Move into the directory ```cd ~/bagfiles```

Now there are two options first to record all the topics and second is to specific topics only

3. For recording all the topics ```rosbag record -a```

A bag file will be created with a name that begins with the year, date, and time and the suffix .bag.

4. We can get info about the bag file by the command ```rosbag ingo <bag file>```, about the toopics recorded and the number of messages recorded.
5. Inorder to record the specific topics run ```rosbag record -O subset /topic1 /topic2```
This will record all the data published on the ```topic1``` and ```topic2``` and save it in ```subset.bag```
6. Now for playing back the rosbag file run ```rosbag play <your bagfile>```

All the data recorded will be published on the specified topic.
