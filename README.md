# 1. Всупление
В репозитории записаны действия, связанные с запуском и работой робота Newsha

# 2. Сборка программ для запуска лидара и построения облаков точек

## 2.1 Ubuntu and ROS
Предпочтительна Ubuntu20.04 и выше с ROS. 

ROS noetic:
- <http://wiki.ros.org/noetic/Installation/Ubuntu>

Дополнительные пакеты, необходимые для запуска (вероятно они уже установлены):
```
sudo apt-get install ros-xxx-pcl-conversions
```
P.s.: вместо xxx вставить вашу версию ROS

## 2.2 Eigen
Установка Eigen:
```
sudo apt-get install libeigen3-dev
```

## 2.3 unilidar_sdk

Мы используем лидар 'L1', чтобы уставновить и собрать программу для его запуска необходимо выполнить следующие шаги:

```
git clone https://github.com/unitreerobotics/unilidar_sdk.git

cd unilidar_sdk/unitree_lidar_ros

catkin_make
```

## 2.4 Построение catkin_point_unilidar для ROS

Выполните следующие шаги:

```
mkdir -p catkin_point_lio_unilidar/src

cd catkin_point_lio_unilidar/src

git clone https://github.com/unitreerobotics/point_lio_unilidar.git

cd ..

catkin_make
```

# 3. Запуск программ

## 3.1 Запуск с подключенным лидаром

Для правильного запуска лидара желательно, чтобы он находился в стационарном состоянии.
Если в /catkin_point_lio_unilidar/src/point_lio_unilidar/ нет папки PCD, необходимо ее создать:
```
mkdir catkin_point_lio_unilidar/src/point_lio_unilidar/PCD
```
Запуск `unilidar`, проводить его из папки unilidar_sdk/unitree_lidar_ros:
```
cd unilidar_sdk/unitree_lidar_ros
```
Один раз за сеанс в этой папке нужно сделать (+можно начать с повтора этой команды, если что-то идет не так):
```
source devel/setup.bash
```
Запуск лидара без подключения rviz:
```
roslaunch unitree_lidar_ros run_without_rviz.launch
```

При возникновении ошибки, связанной с нахождением лидара можно попробовать:
```
sudo chmod 666 /dev/ttyUSB0
```

Запуск `Point-LIO` (вариант для последней сборки программ), делать из другого терминала, из папки unilidar_sdk/unitree_lidar_ros/catkin_unilidar_point_lio:
```
cd catkin_unilidar_point_lio
```
Один раз за сеанс в этой папке нужно сделать (+можно начать с повтора этой команды, если что-то идет не так):
```
source devel/setup.bash
```
Запуск rviz и программы по построению облака точек:
```
roslaunch point_lio_unilidar mapping_unilidar_l1.launch 
```
Если есть какие-то проблеммы с launch-файлом, то возможно была загружена старая версия репозитория, можно попробовать запустить:
```
roslaunch point_lio_unilidar mapping_unilidar.launch 
```

## 3.2 Запуск с rosbag

Если нет возможности работать с подключенным лидаром, можно работать с записями rosbag.
Пример такой записи:
- [unilidar-2023-09-22-12-42-04.bag - Download](https://oss-global-cdn.unitree.com/static/unilidar-2023-09-22-12-42-04.zip)

Запуск `Point-LIO` (вариант для последней сборки программ), делать из другого терминала, из папки unilidar_sdk/unitree_lidar_ros/catkin_unilidar_point_lio:
```
cd catkin_unilidar_point_lio
```
Один раз за сеанс в этой папке нужно сделать (+можно начать с повтора этой команды, если что-то идет не так):
```
source devel/setup.bash
```
Запуск rviz и программы по построению облака точек:
```
roslaunch point_lio_unilidar mapping_unilidar_l1.launch 
```
Если есть какие-то проблеммы с launch-файлом, то возможно была загружена старая версия репозитория, можно попробовать запустить:
```
roslaunch point_lio_unilidar mapping_unilidar.launch 
```

Запуск загруженного датасета (либо любого другого):
```
rosbag play unilidar-2023-09-22-12-42-04.bag 
```

## 3.3 Сохранение pcd-файла
При завершении работы с лидаром, желательно сначала закрыть rviz, затем завершить процесс работы лидара. При корректном завершении появится файл с облаком точек:
```
catkin_point_lio_unilidar/src/point_lio_unilidar/PCD/scans.pcd
```

Для его просмотра можно использовать `pcl_viewer`:
```
pcl_viewer scans.pcd 
```

# 4. Передача данных с лидара через малинку
На малинке стоит ROS2, для нее нет point_lio, поэтому нужно передавать результат работы малинки на компютер с установленным на нем ROS2 и ROS. ROS2 и ROS связаны через bridg.

## 4.1 Запуск лидара на малинке
Из дирректории Desktop/projects/myenv/unilidar_sdk/unitree_lidar_ros2:
```
source install/setup.bash
```
```
ros2 launch unitree_lidar_ros2 launch.py
```
## 4.2 Запуск программы на компьютере 

# 5.

```
ros2 run raspberry_camera raspberry_camera
```

