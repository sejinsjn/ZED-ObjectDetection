
# Installation der ZED-Kamera und Objekterkennung

## Voraussetzungen

```bash
  Nvidia GPU mit einer Compute Compability > 3.0
  ubuntu 22.04 Jammy (Systemsprache Deutsch)
  ~ 50GB Speicherplatz
```
## Installation


Nvidia Treiber und CUDA (oder [Nvidia](https://www.nvidia.de/Download/index.aspx?lang=de/) besuchen und downloaden)


  - Drücke die Supertaste (Windows- oder auch Befehltaste)
  - Suche nach der Aktualisierungsverwaltung
  - Gegebenenfalls aufklappen und Einstellungen öffnen
  - Reiter "Zusätzliche Treiber" auswählen
  - Empfohlenen Treiber auswählen 
  - Wenn aufgefordert neustarten
  - Mit nvidia-smi testen ob Treiber und CUDA installiert sind

### ROS2 Humble und RVIZ2

1. Locale setzen

```bash
  locale  # check for UTF-8
  
  sudo apt update && sudo apt install locales
  sudo locale-gen en_US en_US.UTF-8
  sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
  export LANG=en_US.UTF-8
  
  locale  # verify settings
```

2. Quellen einrichten

```bash
  sudo apt install software-properties-common
  sudo add-apt-repository universe

  sudo apt update && sudo apt install curl -y
  sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

3. ROS2 Pakete installieren

```bash
  sudo apt update
  sudo apt upgrade
  sudo apt install ros-humble-desktop
```

4. Umgebungseinrichtung

```bash
  # Replace ".bash" with your shell if you're not using bash
  # Possible values are: setup.bash, setup.sh, setup.zsh
  source /opt/ros/humble/setup.bash
```

5. Verifizierung der Installation

In einem Terminal wird dieses Skript ausgeführt
```bash
  source /opt/ros/humble/setup.bash
  ros2 run demo_nodes_cpp talker
```

und in einem anderen Terminal dann dieses
```bash
  source /opt/ros/humble/setup.bash
  ros2 run demo_nodes_py listener
```
Außerdem sollte einmal geprüft werden ob RVIZ2 installiert ist und dies kann man einmal tun indem man einmal rviz2 ins Terminal eingibt.


### ZED SDK

[Hier](https://www.stereolabs.com/developers/release/) muss erstmal die neuste Version der ZED SDK runtergeladen werden.

1. Öffne den Download Ordner wo die ZED SDK liegt
```bash
  cd path/to/download/folder
```

2. Falls nicht installiert, installiere zstd

```bash
  sudo apt install zstd
```

3. Fügen Sie dem Installationsprogramm mit dem Befehl chmod +x die Ausführungsberechtigung hinzu. Version kann abweichen und sollte angepasst werden.

```bash
  chmod +x ZED_SDK_Ubuntu22_cuda11.7_v3.8.2.zstd.run
```

4. Starte die Installation

```bash
  ./ZED_SDK_Ubuntu22_cuda11.7_v3.8.2.zstd.run
```

Alles mit y oder Enter beantworten bis auf die Optimierung der AI-Modelle welches zu viel Zeit beansprucht. Wenn die Zeit besteht, wäre es auch sinnvoll die Optimierung durchzuführen.

Nach der Beendigung der Installation folgende Schritte ausführen
```bash
  /usr/local/bin
  ./ZED_Diagnostic #Vorher die ZED-Kamera anschließen
```

Danach auch ZED_Explorer und ZED_Sensor_Viewer auf die gleiche Weise mit ./ testen.

### ZED-ROS2

Damit alles reibungslos verläuft, müssen wir erstmal colcon, rosdep und git installieren.

```bash
  sudo apt install python3-colcon-common-extensions
  sudo apt install python3-rosdep2
  sudo apt install git
```

Um die ZED-ROS2-Examples zu installieren führen wir folgendes aus.

```bash
  source /opt/ros/humble/setup.bash
  mkdir -p ~/ros2_ws/src
  cd ~/ros2_ws/src/
  git clone https://github.com/stereolabs/zed-ros2-examples.git
  cd ../
  rosdep install --from-paths src --ignore-src -r -y
  colcon build --symlink-install --cmake-args=-DCMAKE_BUILD_TYPE=Release
  source ~/.bashrc
```

Als nächstes muss der ZED-Wrapper installiert werden

```bash
  cd ~/ros2_ws/src/ #use your current ros2 workspace folder
  git clone  --recursive https://github.com/stereolabs/zed-ros2-wrapper.git
  cd ..
  rosdep install --from-paths src --ignore-src -r -y
  colcon build --symlink-install --cmake-args=-DCMAKE_BUILD_TYPE=Release --parallel-workers $(nproc)
  echo source $(pwd)/install/local_setup.bash >> ~/.bashrc
  source ~/.bashrc
```

Falls es zu Problemen kommt, könnte das folgende helfen

```bash
  pip install setuptools==58.2.0.
```

und colcon build --symlink-install --cmake-args=-DCMAKE_BUILD_TYPE=Release --parallel-workers $(nproc)
durch colcon build --symlink-install --cmake-args=-DCMAKE_BUILD_TYPE=Release --parallel-workers 2 ersetzen.

Führe den Befehl 
```bash
  ros2 launch zed_display_rviz2 display_zed2.launch.py
```

aus.
## Documentation

[Documentation](https://linktodocumentation)


