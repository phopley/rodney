# rodney_control
This Arduino sketch, rodney_control, runs using rosserial and is a ROS node which controls upto four RC Servo's.

It is based on the rosserial Servo Control Example. This version controls upto four RC Servos. The node subscribes to the servo topic which consists of a servo_msgs::servo_array message. This message contains two elements, index and angle. The index addresses the servo 0-3 and the angle gives the servo required angle 0-180.