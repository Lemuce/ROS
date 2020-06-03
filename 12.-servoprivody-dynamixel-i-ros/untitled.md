# Подглава 12.12 Полное приложение для отслеживания головы ROS

Все три последних примера имеют три из четырех общих launch-файлов. Мы можем создать один launch-файл, который включает в себя три общих файла, а затем выбрать конкретный Vision launch-файл, который мы хотим, основываясь на аргументе командной строки.

Взгляните на файл head\_tracker\_app.launch в каталоге rbx1\_apps/launch и воспроизведенный ниже:

```text
<launch>
 <!-- The camera arg can be one of kinect, xtion, or usb -->
 <arg name="kinect" default="false" />
 <arg name="xtion" default="false" />
 <arg name="usb" default="false" />
 <!-- These arguments determine which vision node we run -->
 <arg name="face" default="false" />
 <arg name="color" default="false" />
 <arg name="keypoints" default="false" />
 <!-- Launch the appropriate camera driver -->
 <include if="$(arg kinect)" file="$(find
freenect_launch)/launch/examples/freenect-registered-xyzrgb.launch" />

 <include if="$(arg xtion)" file="$(find
openni2_launch)/launch/openni2.launch">
 <arg name="depth_registration" value="true" />
 </include>
  <include if="$(arg usb)" file="$(find rbx1_vision)/launch/usb_cam.launch" />

 <include if="$(arg face)" file="$(find
rbx1_vision)/launch/face_tracker2.launch" />
 <include if="$(arg color)" file="$(find
rbx1_vision)/launch/camshift.launch" />
 <include if="$(arg keypoints)" file="$(find
rbx1_vision)/launch/lk_tracker.launch" />

 <include file="$(find rbx1_dynamixels)/launch/dynamixels.launch" />
 <include file="$(find rbx1_dynamixels)/launch/head_tracker.launch" />

</launch>
```

Launch-файл использует ряд аргументов для управления тем, какие другие файлы запуска включены в любой конкретный запуск. Первые три аргумента: kinect, xtion или usb - указывают, какой драйвер камеры следует загрузить. Все аргументы по умолчанию имеют значение false, поэтому мы устанавливаем одно значение true в командной строке, как мы покажем ниже.

Следующие три аргумента: face\(лицо\), color \(цвет\) и keypoints \(ключевые точки\) определяют, какой узел видения мы будем запускать. Все три значения по умолчанию имеют значение false, поэтому мы должны установить одно из них в true в командной строке. Следуя определениям аргументов, мы запускаем либо freenectdriver, либо драйвер openni2, либо драйвер usb\_cam в зависимости от аргумента камеры, предоставленного в командной строке.

Одна из следующих трех строк выполняется в зависимости от того, какой аргумент vision мы устанавливаем в командной строке в значение True. Например, если пользователь установил аргумент color в True, то файл camshift.launch будет запущен.

Далее мы запускаем файл dynamixels.launch, чтобы запустить сервоприводы. И наконец, мы запускаем файл head\_tracker\_app.launch, чтобы начать отслеживание головы.

Используя наш новый launch-файл, мы теперь можем запустить все приложение отслеживания головы для отслеживания лиц, цветов или ключевых точек с помощью одной команды. Например, чтобы отслеживать лицо с помощью Kinect \(по умолчанию\), мы бы запустили:

`$ roslaunch rbx1_apps head_tracker_app.launch kinect:=true face:=true`

Чтобы отслеживать цвета вместо этого, мы бы запустили:

`$ roslaunch rbx1_apps head_tracker_app.launch color:=True`

Или использовать веб-камеру и отслеживание ключевых точек:

`$ roslaunch rbx1_apps head_tracker_app.launch  depth_camera:=False keypoints:=True`

Последнее замечание: если вы забыли выбрать один из режимов vision в командной строке, вы всегда можете запустить его файл запуска позже. Другими словами, команда:

`$ roslaunch rbx1_apps head_tracker_app.launch face:=True`

это эквивалентно запуску:

`$ roslaunch rbx1_apps head_tracker_app.launch`

с последующим:

`$ roslaunch rbx1_vision face_tracker2.launch`

