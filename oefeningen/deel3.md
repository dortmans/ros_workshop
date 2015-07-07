# Workshop "ROS voor Engineers" - deel3

auteur: Eric Dortmans (e.dortmans@fontys.nl)

## Voorbereiding

Update de ros_examples stack die je in de eerste sessie hebt geinstalleerd:

    cd ~/catkin_ws/src/ros_examples
    git pull
    cd ~/catkin_ws
    catkin_make

Installeer benodigde binaire ROS packages

    sudo apt-get install ros-indigo-urdf 
    sudo apt-get install ros-indigo-urdf-tutorial
    sudo apt-get install liburdfdom-tools
    sudo apt-get install ros-indigo-turtlebot ros-indigo-turtlebot-arm
    sudo apt-get install ros-indigo-gazebo-ros-pkgs ros-indigo-gazebo-ros-control
    sudo apt-get install ros-indigo-moveit-full
    
Installeer Gazebo demo package:

    cd ~/catkin_ws/src/
    git clone https://github.com/ros-simulation/gazebo_ros_demos.git
    cd ..
    catkin_make

## URDF files

Alle URDF files die we gebruiken kun je vinden in het package *urdf_examples*.
Je kunt naar betreffende map met URDF files gaan via het volgende commando:

    roscd urdf_examples/urdf

## URDF test

Bekijk de file *test_robot.urdf* in je editor:

    gedit test_robot.urdf
   
Maak een PDF file met een grafische representatie van het model:

    urdf_to_graphiz test_robot.urdf
    evince test_robot.pdf

Visualizeer dit model nu in RViz:

    roslaunch urdf_tutorial display.launch model:=test_robot.urdf
    
Kies *link1* als *Fixed Frame*.
  
Wat zie je en wat niet?

Open een nieuwe terminal en Bekijk de ROS Graph om te zien hoe wat de visualisatie launch file heeft opgestart:

    rqt_graph

## Een Link positioneren

We gaan nu een robot met wat meer vorm bekijken. Open het model *block.urdf* in een editor:

    gedit block.urdf

Visualiseer in een \ndere terminal dit model in RViz:

    roslaunch urdf_tutorial display.launch model:=block.urdf

Selecteer *world* als *Fixed Frame*.

Zoals je ziet bevat het model 1 blok.

Zet het vinkje bij *RobotModel* uit als je alleen de TF output wilt zien of het vinkje bij *TF* als je alleen het *RobotModel* wilt zien.

Als je goed kijkt zie je dat dit blok niet goed op de grond (*world*) staat. Het is als het ware half verzonken. Waar komt daar door?

Pas de URDF file aan zodat het blok goed op de grond komt te staan en start de visualisatie opnieuw.

## Links verbinden in URDF en URDF+XACRO model

Bekijk nu het model *blocks.urdf* in een editor:

    gedit blocks.urdf

Maak een PDF file met een grafische representatie van het model and bekijk die:

    urdf_to_graphiz blocks.urdf
    evince blocks.pdf

Visualiseer dit model in RViZ:

    roslaunch urdf_tutorial display.launch model:=blocks.urdf

Selecteer *world* als *Fixed Frame*.

Je ziet nu 3 blokken: een rode, een groene en een blauwe.

Zet het vinkje bij *RobotModel* uit. En bekijk de TF output.

Pas de URDF file aan om de blokken op elkaar te stapelen: het groene blok op het rode en het blauwe op het groene.
de stapel moet natuurlijk netjes op de grond komen te staan. Visualiseer (telkens) opnieuw als je (gedeeltelijk) klaar bent met aanpassen.

Als dit gelukt is open het model *blocks.urdf.xacro*:

    gedit blocks.urdf.xacro

Pas ook dit URDF+XACRO model aan om de blokken netjes op elkaar te stapelen. Je zult merken dat dit veel minder werk is.

Als dit gelukt is, herstart dan de visualizatie met als extra de *gui:=true* optie:

    roslaunch urdf_tutorial xacrodisplay.launch model:=blocks.urdf.xacro gui:=true

Bedien de *schuifjes* en de *random* en *center* buttons in de GUI en zie wat er gebeurt.

## De robot_description parameter

Je kunt ook de parameter server vragen om een kopie van het huidige robot model:
Laat de visualisatie draaien en open een nieuwe terminal. Geef dan het volgende commando:

    rosparam get -p /robot_description | tail -n +2 |cut -c 3- > robot_description.urdf

Je vraagt dus gewoon de huidige waarde van de 'robot_description' parameter op.

Bekijk de file die je hebt gemaakt:

    gedit robot_description.urdf

Open ook nog eens de blocks.urdf file

    gedit blocks.urdf
    
Zie je verschil?

## Een mobiele robot modelleren

We gaan een mobiele robot bouwen, althans modelleren.

Bekijk het model *mobile_robot.urdf.xacro* in een editor:

    gedit mobile_robot.urdf.xacro

Visualiseer dit model:

    roslaunch urdf_tutorial xacrodisplay.launch model:=mobile_robot.urdf.xacro

Het model klopt niet. De wielen zitten niet goed aan de base. Maak dat in orde.

Heb je de wielen goed zitten? 
Voeg dan de volgende regels aan je modelfile toe om een Kinect support steun en een Kinect aan het mobiele platform toe te voegen:

  <joint name="kinect_support_joint" type="fixed">
    <parent link="base_link" />
    <child link="kinect_support" />
    <origin xyz="-0.118 0 ${base_height/2}" rpy="0 0 0" />
  </joint>

  <link name="kinect_support">
    <visual>
      <origin xyz="0 0 ${kinect_support_height/2}" rpy="0 0 0" />
      <geometry>
        <box size="0.05 0.05 ${kinect_support_height}" />
      </geometry>
      <material name="grey_blue">
        <color rgba="0.4 0.4 1.0 1"/>
      </material>
    </visual>
  </link>

  <xacro:include filename="$(find turtlebot_description)/urdf/sensors/kinect.urdf.xacro"/>
  <sensor_kinect parent="base_link"/>

Tenslotte start de visualisatie (opnieuw):

   roslaunch urdf_tutorial xacrodisplay.launch model:=mobile_robot.urdf.xacro

Zet het vinkje bij TF uit om het model goed te zien. Als het goed is zit alles netjes aan elkaar. 

## Stage simulatie

We hebben nu een leuk robot platform gemodelleerd maar kan het ook meebewegen met de echte of gesimuleerde robot?

Laat de visulatiesatie draaien en start in een nieuwe terminal een gesimuleerde wereld in Stage:

    roslaunch stage_worlds kinect_world.launch

Start in weer een andere terminal de *arbotix gui* om met je muis de robot te kunnen besturen:

    arbotix_gui

Als je de gesimuleerde robot laat bewegen zul je zien dat je robotmodel meebeweegt.

## De mobiele robot krijgt een arm

Om onze mobile robot af te maken zetten we er nog een arm op. Voege het volgende toe aan de model file:

  <include filename="$(find turtlebot_arm_description)/urdf/arm.xacro" />
  <turtlebot_arm parent="base_link" color="white" gripper_color="green"
    joints_vlimit="${M_PI/2}" pan_llimit="-${M_PI/2}" pan_ulimit="${M_PI/2}">
    <origin xyz="0.12 0 0.02"/>
  </turtlebot_arm>

Kunnen we de arm ook bewegen?

Herstart visualisatie met de *gui:=true* optie: 

    roslaunch urdf_tutorial xacrodisplay.launch model:=mobile_robot.urdf.xacro gui:=true

## Gazebo simulatie

Om met Gazebo te experimenteren gebruiken we de RRBot (Revolute-Revolute Manipulator Robot). RRBot is a simpele robot arm met 3 links en 2 joints.Het is in essentie een *double inverted pendulum*.

Bekijk het model

    gedit `rospack find rrbot_description`/urdf/rrbot.xacro

Start RRBot in Rviz om te kijken of het model werkt

    roslaunch rrbot_description rrbot_rviz.launch

Bedien de joints. Zoals je ziet is er in RViz geen zwaartekracht en alle bewegingen gaan oneindig snel.

Start nu de RRBot in Gazebo:

    roslaunch rrbot_gazebo rrbot_world.launch

De motoren van de joints zijn nog onbekrachtigd dus de zwaartekracht heeft vrij spel. Wacht af en kijk wat de zwaartekracht doet.

Laad en start nu the joint position controllers:

    rosservice call /rrbot/controller_manager/load_controller "name: 'joint1_position_controller'"
    rosservice call /rrbot/controller_manager/load_controller "name: 'joint2_position_controller'"
    rosservice call /rrbot/controller_manager/switch_controller "{start_controllers: ['joint1_position_controller','joint2_position_controller'], stop_controllers: [], strictness: 2}"

Gebruik bijvoorbeeld de volgende rostopic pub commando's om joint1 en/of joint 2 aan te sturen:

    rostopic pub -1 /rrbot/joint1_position_controller/command std_msgs/Float64 "data: 1.5"
    rostopic pub -1 /rrbot/joint2_position_controller/command std_msgs/Float64 "data: 1.0"
    
## MoveIt!

We gaan motionplanning doen met de de arm van onze mobiele robot.

Start MoveIt in RViz:

    roslaunch turtlebot_arm_moveit_config turtlebot_arm_moveit.launch sim:=true --screen

Ga naar de tab *Planning*. Trek aan de interactieve markers om een doelpose (*Goal State*) te kiezen en druk dan op *Plan* om een traject te laten uitrekenen. Je kunt ook *<random>* een doelpose kiezen kiezen.

Als je hier bent heb je hopelijk geleerd hoe je zonder te programmeren toch veel met ROS kunt doen.
 
## Referenties

- [ROS Tutorials](http://wiki.ros.org/ROS/Tutorials)
- [ROS-Industrial(Indigo) Training Exercises](http://aeswiki.datasys.swri.edu/rositraining/indigo/Exercises)
- [XML Robot Description Format (URDF)](http://wiki.ros.org/urdf/XML/model)
- [URDF Tutorials](http://wiki.ros.org/urdf/Tutorials)
- [Create a URDF for an Industrial Robot](http://wiki.ros.org/Industrial/Tutorials/Create%20a%20URDF%20for%20an%20Industrial%20Robot)
- [List of moments of inertia](https://en.wikipedia.org/wiki/List_of_moments_of_inertia)
- [Specification for TurtleBot Compatible Platforms](http://www.ros.org/reps/rep-0119.html)
- [Tutorial: Using a URDF in Gazebo](http://gazebosim.org/tutorials?tut=ros_urdf)
- [Tutorial: ROS Control](http://gazebosim.org/tutorials/?tut=ros_control)
