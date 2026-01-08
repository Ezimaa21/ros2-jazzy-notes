1 Setting Up ROS 2 Environment

Before working with ROS 2, source the ROS 2 installation:

    source /opt/ros/jazzy/setup.bash


Verify ROS 2 is installed:

    printenv ROS_DISTRO
# Output: jazzy

# Creating a Workspace

Create a workspace and source folder:

    mkdir -p ~/Desktop/ros2_ws/src
    cd ~/Desktop/ros2_ws

3. Creating a ROS 2 Python Package

Navigate to the src folder and create a new Python package:

    cd ~/Desktop/ros2_ws/src
    ros2 pkg create --build-type ament_python first_package


This generates the following structure:

first_package/

    ├── first_package/       # Python module folder
    
    │   └── __init__.py
    ├── resource/
    │   └── first_package
    ├── setup.py
    ├── setup.cfg
    ├── package.xml
    └── test/
        ├── test_copyright.py
        ├── test_flake8.py
        └── test_pep257.py

4. Writing Publisher Node

Create publisher.py in first_package/first_package/:

    import rclpy
    from rclpy.node import Node
    from std_msgs.msg import String
    
    class PublisherNode(Node):
        def __init__(self):
            super().__init__('node_publisher')
            self.publisher_ = self.create_publisher(String, 'communication_topic', 10)
            self.timer = self.create_timer(1.0, self.callback_function)
            self.counter = 0

    def callback_function(self):
        msg = String()
        msg.data = f'Counter: {self.counter}'
        self.publisher_.publish(msg)
        self.get_logger().info(f'Publishing: "{msg.data}"')
        self.counter += 1

    def main(args=None):
        rclpy.init(args=args)
        node = PublisherNode()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()

5. Writing Subscriber Node

Create subscriber.py in first_package/first_package/:

    import rclpy
    from rclpy.node import Node
    from std_msgs.msg import String
    
    class SubscriberNode(Node):
        def __init__(self):
            super().__init__('node_subscriber')
            self.subscription = self.create_subscription(
                String,
                'communication_topic',
                self.listener_callback,
                10
            )
    
        def listener_callback(self, msg):
            self.get_logger().info(f'Received: "{msg.data}"')
    
    def main(args=None):
        rclpy.init(args=args)
        node = SubscriberNode()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()
    
6. Updating setup.py Entry Points

Make sure your setup.py contains:

    entry_points={
        'console_scripts': [
            'talker = first_package.publisher:main',
            'listener = first_package.subscriber:main',
        ],
    },


⚠️ Common error: missing comma between console scripts prevents nodes from being registered.

7. Building the Workspace

Clean build and rebuild the workspace:

    cd ~/Desktop/ros2_ws
    rm -rf build install log
    colcon build --packages-select first_package


Source the workspace:

    source install/setup.bash


Check that ROS 2 recognizes the package:

    ros2 pkg list | grep first_package
# Output: first_package

8. Running Nodes

Open two terminals. In both, source the workspace:

    source ~/Desktop/ros2_ws/install/setup.bash
    
    Run the publisher:
    ros2 run first_package talker


Output example:

    [INFO] [node_publisher]: Publisher node has started
    [INFO] [node_publisher]: Publishing: "Counter: 0"
    [INFO] [node_publisher]: Publishing: "Counter: 1"

Run the subscriber (in second terminal):

    ros2 run first_package listener
    
    
Output example:
    
    [INFO] [node_subscriber]: Received: "Counter: 0"
    [INFO] [node_subscriber]: Received: "Counter: 1"

Notes

Make sure to source both /opt/ros/jazzy/setup.bash and your workspace install/setup.bash in every new terminal before running nodes.

If ros2 run cannot find your package, check:

Workspace was built with colcon build

setup.py entry_points are correct

Workspace is sourced

## PS I had to make sure the description, maintainer, or license, always update both files so they stay consistent in setup.py and package.xml
