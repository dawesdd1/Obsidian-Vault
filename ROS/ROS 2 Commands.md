Tags: [[ROS2]]

##### ROS topics 
```
ros2 topic echo <Topic_Name>       # list specific topic
ros2 topic lsit                    # list all 
```

##### Visualizing node connections 
`Rqt` pulls up a graph of your node communications
```
rqt 

# graph visualizer should pop up
```

##### Listing Class Attributes of Include Imports
use this bash command to see the class attributes of a certain msg/data type
```
ros2 interface show vision_msgs/msg/Detection2DArray

```

Output e.g. 
```
# A list of 2D detections, for a multi-object 2D detector.

std_msgs/Header header
        builtin_interfaces/Time stamp
                int32 sec
                uint32 nanosec
        string frame_id

# A list of the detected proposals. A multi-proposal detector might generate
#   this list with many candidate detections generated from a single input.
Detection2D[] detections
        #
        std_msgs/Header header
                builtin_interfaces/Time stamp
                        int32 sec
                        uint32 nanosec
                string frame_id
        ObjectHypothesisWithPose[] results
                ObjectHypothesis hypothesis
                        string class_id
                        float64 score
                geometry_msgs/PoseWithCovariance pose
                        Pose pose
                                Point position
                                        float64 x
                                        float64 y
                                        float64 z
                                Quaternion orientation
                                        float64 x 0
                                        float64 y 0
                                        float64 z 0
                                        float64 w 1
                        float64[36] covariance
        BoundingBox2D bbox
                vision_msgs/Pose2D center
                        vision_msgs/Point2D position
                                float64 x
                                float64 y
                        float64 theta
                float64 size_x
                float64 size_y
        string id
```