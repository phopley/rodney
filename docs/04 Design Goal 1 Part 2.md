## Face Recognition
The pan_tilt node was written in C++, this node will be written in Python. This partly because it gives us examples of ROS nodes written in two different languages but also because I'm going to write the node around some existing face recognition Python code.

The package is available in the GitHub Repository https://github.com/phopley/face_recognition. See the package documentation for details. This package not only contains the code for the ROS node but also two standalone Python scripts that are used to train system for the faces you wish to recognise.

The `data_set_generator.py` script is used to capture images of each subject. It uses the Raspberry Pi Camera to capture facial images and stores them ready for the face recognition training phase. The script should be run for each subject that you wish to recognise. Ensure when prompted you give the next unique ID value (start at 1) and increase for each subject recorded. As well as a unique ID the script prompts for the subject name and wether  it is low light conditions. Ideally the script should be run twice per subject (giving the same ID) in good light and low light conditions.

Once the `data_set_generator.py` script has been run the next stage is to run the `training.py` script, this script examines and the images in the data set and produces a training file which is load by the ROS node. Each time a new subject is added to the data set, using the `data_set_generator.py` script, the `training.py` script should be re-run.

The face_recognition node itself, when instructed examines the next frame from the Pi camera and attempts to detect a face(s). If a face(s) is detected it them attempts to identify the face(s) from those that it has been trained with. A ROS message is sent from the node containing the unique ID and names of any faces identified. If no faces are identified the data in the message will be empty. As well as this message is also publishes a messages which contains a copy of the modified image. If a face(s) was detected this modified image includes a square drawn around each face along with text showing the name of the subject and a confidence level indicating  how confident the software is that the name is correct. The package includes a confidence_level configurable parameter which is used to set a threshold level used to accept the identification. If the confidence level for a detection is below this threshold then the square on the modified image is red in colour and the message with ID and name will be empty. If the confidence level for a detection is above this threshold then the square on the modified image is green in colour and the name and ID will appear in the other message.

The message containing the ID and name is once again a user defined message. It is available in the ROS package face_recognition_msgs. This package also contains an action which is used to trigger a complete scan, which includes moving the head with the pan/tilt device. This package is available in the GitHub Repository https://github.com/phopley/face_recognition_msgs. See the package documentation for full details.

Next describe the camera node and the bridge before showing a graph of the nodes together.