# Gazebo ROS2 Bridge Simulator

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![ROS 2 Humble](https://img.shields.io/badge/ROS2-Humble-blue.svg)](https://docs.ros.org/en/humble/)
[![Gazebo 11+](https://img.shields.io/badge/Gazebo-11+-orange.svg)](https://gazebosim.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A comprehensive simulation framework that seamlessly integrates Gazebo Classic and Gazebo Sim with ROS 2, enabling realistic testing and validation of autonomous robotics systems.

## 🚀 Features

- **Seamless Integration**: Bridge Gazebo simulation with ROS 2 nodes effortlessly
- **Multi-Robot Support**: Simulate multiple robots with independent control
- **Sensor Simulation**: LiDAR, IMU, GPS, cameras, depth sensors, contact sensors
- **Custom Plugins**: Create and load custom Gazebo plugins for ROS 2
- **Real-time Control**: Low-latency communication between simulation and ROS 2
- **World Generation**: Procedural environment generation for diverse testing scenarios
- **Scenario Testing**: Pre-built test scenarios for navigation, manipulation, and perception
- **Data Logging**: Record and playback simulation data for analysis
- **Docker Support**: Containerized deployment for reproducible simulations
- **CI/CD Integration**: Automated testing pipelines for robotics software

## 📦 Installation

### Prerequisites

- Ubuntu 20.04/22.04
- ROS 2 Humble/Iron
- Gazebo 11 or Gazebo Sim
- Python 3.8+

### Quick Installation

```bash
# Install ROS 2 dependencies
sudo apt update
sudo apt install ros-humble-gazebo-ros-pkgs

# Install package
pip install gazebo-ros2-bridge-simulator
```

### From Source

```bash
# Clone repository
git clone https://github.com/strangerwhoisharborofdoom/gazebo-ros2-bridge-simulator.git
cd gazebo-ros2-bridge-simulator

# Create ROS 2 workspace
mkdir -p ~/ros2_ws/src
cp -r . ~/ros2_ws/src/

# Build
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install

# Source workspace
source install/setup.bash
```

## 🔧 Usage

### Launch Simulation

```bash
# Launch default simulation with differential drive robot
ros2 launch gazebo_ros2_bridge_simulator default_simulation.launch.py

# Launch with specific robot
ros2 launch gazebo_ros2_bridge_simulator robot_simulation.launch.py robot:=turtlebot3

# Launch multi-robot scenario
ros2 launch gazebo_ros2_bridge_simulator multi_robot.launch.py num_robots:=3

# Launch custom world
ros2 launch gazebo_ros2_bridge_simulator custom_world.launch.py world:=warehouse
```

### Python API

```python
from gazebo_ros2_bridge import SimulationManager, RobotSpawner

# Initialize simulation
sim = SimulationManager()
sim.start()

# Spawn robot
spawner = RobotSpawner()
spawner.spawn_robot(
    name='robot_1',
    model='turtlebot3',
    x=0.0, y=0.0, z=0.0,
    yaw=0.0
)

# Load sensors
spawner.add_lidar(robot_name='robot_1')
spawner.add_camera(robot_name='robot_1')
spawner.add_imu(robot_name='robot_1')

# Run simulation
sim.run(duration=60)  # Run for 60 seconds

# Stop simulation
sim.stop()
```

### ROS 2 Nodes

```bash
# List active topics
ros2 topic list

# View sensor data
ros2 topic echo /robot_1/laser/scan
ros2 topic echo /robot_1/camera/image_raw
ros2 topic echo /robot_1/imu/data

# Send velocity commands
ros2 topic pub /robot_1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.5}, angular: {z: 0.1}}"
```

## 📁 Project Structure

```
gazebo-ros2-bridge-simulator/
├── gazebo_ros2_bridge/       # Main package
│   ├── __init__.py
│   ├── simulation_manager.py # Simulation lifecycle management
│   ├── robot_spawner.py      # Robot spawning utilities
│   ├── sensor_plugins.py     # Sensor configurations
│   ├── bridge_node.py        # ROS 2-Gazebo bridge
│   └── world_generator.py    # Procedural world generation
├── launch/                   # Launch files
│   ├── default_simulation.launch.py
│   ├── robot_simulation.launch.py
│   ├── multi_robot.launch.py
│   ├── custom_world.launch.py
│   └── test_scenario.launch.py
├── models/                   # Robot models
│   ├── turtlebot3/
│   ├── pioneer3at/
│   ├── custom_robot/
│   └── manipulator_arm/
├── worlds/                   # Simulation worlds
│   ├── empty.world
│   ├── warehouse.world
│   ├── outdoor_terrain.world
│   ├── office_environment.world
│   └── test_course.world
├── config/                   # Configuration files
│   ├── sensors.yaml
│   ├── controllers.yaml
│   └── simulation_params.yaml
├── scripts/                  # Utility scripts
│   ├── record_data.py
│   ├── playback_data.py
│   ├── benchmark.py
│   └── visualize.py
├── tests/                    # Unit and integration tests
│   ├── test_bridge.py
│   ├── test_sensors.py
│   └── test_spawn.py
├── docker/                   # Docker configuration
│   ├── Dockerfile
│   └── docker-compose.yml
├── examples/                 # Example usage
│   ├── navigation_example.py
│   ├── manipulation_example.py
│   └── perception_example.py
├── setup.py
├── requirements.txt
├── package.xml
├── README.md
└── LICENSE
```

## 🌍 Supported Worlds

### Warehouse
- Industrial warehouse environment
- Dynamic obstacles (forklifts, boxes)
- RFID tags and markers
- Ideal for logistics and navigation testing

### Office Environment
- Multi-room office layout
- Doors, corridors, furniture
- Human traffic simulation
- Perfect for service robotics

### Outdoor Terrain
- Uneven terrain with slopes
- Vegetation and obstacles
- GPS coordinates
- Suitable for autonomous vehicles

### Test Course
- Standardized navigation course
- Obstacles and checkpoints
- Performance benchmarking
- Competition-ready scenarios

## 🤖 Supported Robots

- **TurtleBot3**: Burger, Waffle, Waffle Pi
- **Pioneer3AT**: Mobile manipulation
- **Universal Robots**: UR3, UR5, UR10 arms
- **Franka Emika**: Panda manipulator
- **Custom Robots**: Import URDF/SDF models
- **Drones**: Quadrotors with cameras
- **UGVs**: Differential and Ackermann drive

## 🧪 Testing & Validation

```bash
# Run all tests
colcon test --packages-select gazebo_ros2_bridge

# Run navigation tests
ros2 launch gazebo_ros2_bridge_simulator test_navigation.launch.py

# Benchmark performance
python3 scripts/benchmark.py --duration=300

# Validate sensor data
python3 scripts/validate_sensors.py --robot=robot_1
```

## 🐳 Docker Deployment

```bash
# Build Docker image
docker build -t gazebo-ros2-bridge:latest .

# Run container with GUI support
docker run -it --rm \
  -e DISPLAY=$DISPLAY \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  --network host \
  gazebo-ros2-bridge:latest

# Using docker-compose
docker-compose up
```

## 📊 Data Logging & Analysis

```python
from gazebo_ros2_bridge import DataLogger

# Start logging
logger = DataLogger(output_dir='./simulation_data')
logger.start_recording()

# Run simulation...

# Stop logging
logger.stop_recording()

# Analyze data
logger.analyze()
logger.plot_results()
```

## 🔌 Custom Plugin Development

```python
# Example custom plugin
from gazebo_ros2_bridge import GazeboPlugin

class CustomSensorPlugin(GazeboPlugin):
    def __init__(self):
        super().__init__()
        self.publisher = self.create_publisher('custom_topic', 'std_msgs/msg/Float32')
    
    def update(self, data):
        # Process sensor data
        msg = Float32(data=data)
        self.publisher.publish(msg)

# Register plugin
register_plugin('custom_sensor', CustomSensorPlugin)
```

## 🎯 Example Use Cases

### 1. Autonomous Navigation
```bash
ros2 launch gazebo_ros2_bridge_simulator navigation_test.launch.py
ros2 run nav2_bringup navigation_bringup.py
```

### 2. Manipulation Tasks
```python
from manipulation_controller import ManipulationController

controller = ManipulationController('robot_arm')
controller.pick_object(position=[0.5, 0.0, 0.1])
controller.place_object(position=[0.5, 0.5, 0.1])
```

### 3. Perception Testing
```python
from perception_evaluator import PerceptionEvaluator

evaluator = PerceptionEvaluator()
evaluator.test_object_detection()
evaluator.test_slam()
evaluator.test_tracking()
```

## 🛠️ Troubleshooting

### Common Issues

**Gazebo fails to launch:**
```bash
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:~/ros2_ws/src/gazebo-ros2-bridge-simulator/models
```

**ROS 2 topics not publishing:**
```bash
source ~/ros2_ws/install/setup.bash
ros2 daemon start
```

**Performance issues:**
- Reduce sensor update rates in config/sensors.yaml
- Lower rendering quality in Gazebo
- Use headless mode for CI testing

## 🤝 Contributing

Contributions are welcome! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Open Robotics](https://www.openrobotics.org/) for Gazebo and ROS
- [ROS 2 Community](https://ros.org/) for tools and support
- [Gazebo Sim](https://gazebosim.org/) team
- All contributors and testers

## 📧 Contact

- **Author**: Pavan C N
- **GitHub**: [@strangerwhoisharborofdoom](https://github.com/strangerwhoisharborofdoom)
- **Issues**: [Report bugs or request features](https://github.com/strangerwhoisharborofdoom/gazebo-ros2-bridge-simulator/issues)

## 🗺️ Roadmap

### Version 0.1.0 (Current)
- ✅ Basic Gazebo-ROS 2 bridge
- ✅ Robot spawning and control
- ✅ Sensor plugins (LiDAR, camera, IMU)
- ✅ Pre-built worlds
- ✅ Docker support

### Version 0.2.0 (Planned)
- ⏳ Gazebo Sim (Ignition) support
- ⏳ Advanced sensor models
- ⏳ Multi-robot coordination
- ⏳ RL training integration
- ⏳ Web-based visualization

### Version 0.3.0 (Future)
- ⏳ Cloud-based simulation
- ⏳ Digital twin capabilities
- ⏳ Hardware-in-the-loop (HIL)
- ⏳ Automated scenario generation
- ⏳ Performance benchmarking suite
