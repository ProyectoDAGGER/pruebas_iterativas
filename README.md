Para la implementación del sistema de vuelo autónomo del dron, se utilizó el repositorio Fast-Drone-250 desarrollado por ZJU-FAST Lab (2024), disponible en GitHub. Este repositorio proporciona el código fuente y las instrucciones necesarias para configurar y ejecutar el algoritmo FAST LAB 250. La instalación del software comenzó con la configuración de una Intel NUC, que incluyó la instalación de Ubuntu 20.04 y la partición adecuada del disco. Posteriormente, se procedió con la instalación de ROS y sus dependencias, así como del driver para las cámaras Realsense y otros componentes necesarios. El uso de este repositorio fue crucial para integrar y adaptar el algoritmo de control y navegación del dron, permitiendo su correcto funcionamiento y optimización para las pruebas de vuelo autónomo.

Primero hay que garantizar los requerimientos mínimos para la instalación del software en nuestra computadora a bordo, que es una Intel NUC. Estos requisitos son principalmente una distribución Ubuntu 20.04 con la siguiente distribución del disco:
•	EFI 512M
•	swap area 16000M
•	En la computadora a tierra también se requiere una distribución de Ubuntu, recomendable la 20.04.

Se clona este repositorio y se procede a la instalación de las dependencias, empezando desde ROS hasta el algoritmo de navegación.
* Instalación de ROS
  * `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`
  * `sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654'`
  * `sudo apt update`
  * `sudo apt install ros-noetic-desktop-full`
  * `echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc`
  * <font color="#dd0000">It is recommended that students who do not know ROS first learn the ROS introductory tutorial by Guyueju in Bilibili.</font>

* Instalación Driver de Realsense
  * `sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key  F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key  F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE`
  * `sudo add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main" -u`
  * `sudo apt-get install librealsense2-dkms`
  * `sudo apt-get install librealsense2-utils`
  * `sudo apt-get install librealsense2-dev`
  * `sudo apt-get install librealsense2-dbg`
  * test：`realsense-viewer`
  * <font color="#dd0000">Note that the connected USB port must be 3.x (blue).</font>
* Instalación de MAVROS
  * `sudo apt-get install ros-noetic-mavros`
  * `cd /opt/ros/noetic/lib/mavros`
  * `sudo ./install_geographiclib_datasets.sh`
* Instalación de ceres, glog y ddyanmic-reconfigure
  * Descomprima `3rd_party.zip`
  * Abra la terminal y ponga ./glog
  * `./autogen.sh && ./configure && make && sudo make install`
  * `sudo apt-get install liblapack-dev libsuitesparse-dev libcxsparse3.1.2 libgflags-dev libgoogle-glog-dev libgtest-dev`
  * Abra la terminal y ponga ./ceres
  * `mkdir build`
  * `cd build`
  * `cmake ..`
  * `sudo make -j4`
  * `sudo make install`
  * `sudo apt-get install ros-noetic-ddynamic-reconfigure`
* Descargue el repositorio y ponga 
  * `git clone https://github.com/ProyectoDAGGER/pruebas_iterativas.git`
  * `cd Fast-Drone-250`
  * `catkin_make`
  * `source devel/setup.bash`
  * `roslaunch ego_planner single_run_in_sim.launch`
 
    Una vez descargado, empezamos con las pruebas. Primero se prueba la simulación:

    ![Screenshot from 2024-06-26 10-06-35](https://github.com/ProyectoDAGGER/pruebas_iterativas/assets/163484218/90bf4e51-25c5-4c54-a01e-2a2c38cf026a)

    Observamos los nodos de inicialización:

    
    ![Screenshot from 2024-06-26 10-07-44](https://github.com/ProyectoDAGGER/pruebas_iterativas/assets/163484218/f12292fd-5b95-4b12-a35f-afb0bda84fe2)

    Ahora de la carpeta shfiles ejecutamos rspx4.sh y observamos los datos de la imu para calibrarla.
    
![Screenshot from 2024-06-26 10-11-06](https://github.com/ProyectoDAGGER/pruebas_iterativas/assets/163484218/0f19684b-91e8-4f57-bd95-ae30b2e7d6d9)

Finalmente se moviliza el drone hasta lograr las pruebas de calibración de la IMU.

    
![WhatsApp Image 2024-06-27 at 1 50 18 PM](https://github.com/ProyectoDAGGER/pruebas_iterativas/assets/163484218/a89233d9-b607-4a28-a8b6-21c02086ea02)
