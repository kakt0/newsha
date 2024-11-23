# newsha

## 1. Всупление
В репозитории записаны действия, связанные с запуском и работой робота Newsha

## 2. Сборка программ для запуска лидара и построения облаков точек

### 2.1 Ubuntu and ROS
Предпочтительна Ubuntu20.04 и выше с ROS. 

ROS noetic:
- <http://wiki.ros.org/noetic/Installation/Ubuntu>

Дополнительные пакеты, необходимые для запуска (вероятно они уже установлены):
```
sudo apt-get install ros-xxx-pcl-conversions
```
P.s.: вместо xxx вставить вашу версию ROS

### 2.2 Eigen
Following the official [Eigen installation](eigen.tuxfamily.org/index.php?title=Main_Page), or directly install Eigen by:
```
sudo apt-get install libeigen3-dev
```

### 2.3 unilidar_sdk

Мы используем лидар 'L1', чтобы уставновить и собрать программу для его запуска необходимо выполнить следующие шаги:

```
git clone https://github.com/unitreerobotics/unilidar_sdk.git

cd unilidar_sdk/unitree_lidar_ros

catkin_make
```

### 2.4 Построение catkin_point_unilidar для ROS

Выполните следующие шаги:

```
mkdir -p catkin_point_lio_unilidar/src

cd catkin_point_lio_unilidar/src

git clone https://github.com/unitreerobotics/point_lio_unilidar.git

cd ..

catkin_make
```

## 3. Запуск программ

### 3.1 Запуск с подключенным лидаром

Для правильного запуска лидара желательно, чтобы он находился в стационарном состоянии.
Если в /catkin_point_lio_unilidar/src/point_lio_unilidar/ нет папки PCD, необходимо ее создать:
```
mkdir catkin_point_lio_unilidar/src/point_lio_unilidar/PCD
```

Run `unilidar`:
```
cd unilidar_sdk/unitree_lidar_ros

source devel/setup.bash

roslaunch unitree_lidar_ros run_without_rviz.launch
```

Run `Point-LIO` (вариант для последней сборки программ):
```
cd catkin_unilidar_point_lio

source devel/setup.bash

roslaunch point_lio_unilidar mapping_unilidar_l1.launch 
```

Если есть какие-то проблеммы с launch-файлом, попробовать запустить:
```
cd catkin_unilidar_point_lio

source devel/setup.bash

roslaunch point_lio_unilidar mapping_unilidar.launch 
```

### 3.2 Запуск с rosbag

Если нет возможности работать с подключенным лидаром, можно работать с записями rosbag.
Пример такой записи:
- [unilidar-2023-09-22-12-42-04.bag - Download](https://oss-global-cdn.unitree.com/static/unilidar-2023-09-22-12-42-04.zip)


Run `Point-LIO`:
```
cd catkin_point_lio_unilidar

source devel/setup.bash

roslaunch point_lio_unilidar mapping_unilidar_l1.launch 
```

Либо:
```
cd catkin_point_lio_unilidar

source devel/setup.bash

roslaunch point_lio_unilidar mapping_unilidar.launch 
```

Запуск загруженного датасета (либо любого другого):
```
rosbag play unilidar-2023-09-22-12-42-04.bag 
```

### 3.3 Сохранение pcd-файла
При завершении работы с лидаром, желательно сначала закрыть rviz, затем завершить процесс работы лидара. При корректном завершении появится файл с облаком точек:
```
catkin_point_lio_unilidar/src/point_lio_unilidar/PCD/scans.pcd
```

Для его просмотра можно использовать `pcl_viewer`:
```
pcl_viewer scans.pcd 
```
