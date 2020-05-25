# 11.4.3 Запуск приложения Follower на TurtleBot

Если у вас есть TurtleBot, вы можете сравнить наш узел Python follower с версией C++ от Tony Pratkanis, которая находится в пакете turtlebot\_follower. В любом случае, запускайте своего робота в середине большой комнаты, как можно дальше от стен, мебели или других людей.

Убедитесь, что TurtleBot включен, а затем запустите launch-файл:

`$ roslaunch rbx1_bringup turtlebot_minimal_create.launch`

Затем включите камеру глубины: 

Для Microsoft Kinect:

$ roslaunch freenect\_launch freenect-registered-xyzrgb.launch

