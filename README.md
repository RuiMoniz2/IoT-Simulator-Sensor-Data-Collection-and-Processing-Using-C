# IoT-Simulator-Sensor-Data-Collection-and-Processing-Using-C

The IoT Simulator project aims to simulate a simplified Internet of Things (IoT) environment, where multiple sensors send readings to a centralized system. The system stores and processes the data, generates statistics, and triggers alerts based on certain conditions. This README provides an overview of the project, its components, functionalities, and communication mechanisms.

## System Description
### The system consists of various processes and threads, which are described below:
#### Sensor: A process that periodically generates data and sends it via a named pipe to the System Manager. Multiple instances of this process can exist.
#### User Console: A process that interacts with the user and communicates with the System Manager via a named pipe to send commands. It performs system management operations, retrieves statistics and other information, and receives real-time alerts. Information is received through the Message Queue. Multiple instances of this process can exist.
#### System Manager: A process responsible for system initialization, reading the configuration file, creating Worker processes, and the Alerts Watcher process.
#### Worker: A process that handles requests received from the Sensor and User Console processes. Multiple instances of this process can exist.
#### Alerts Watcher: A process that checks if sensor values are outside the defined limits and sends an alert to the respective User Console processes via the Message Queue (only sends alerts to those who have set them).
#### Console Reader: A thread that reads commands from the User Consoles sent via the CONSOLE_PIPE named pipe.
#### Sensor Reader: A thread that reads data from the Sensor processes sent via the SENSOR_PIPE named pipe.
#### Dispatcher: A thread that retrieves requests stored in the INTERNAL_QUEUE and sends them via unnamed pipes to an available Worker.

### The system utilizes various Inter-Process Communication (IPC) mechanisms:

#### Named Pipe SENSOR_PIPE: Allows Sensor processes to send data to the System Manager process.
#### Named Pipe CONSOLE_PIPE: Used to send commands from the User Console processes.
#### Shared Memory (SHM): Shared memory region accessed by the System Manager, Worker, and Alerts Watcher processes.
#### Unnamed pipes: Enable communication between the System Manager process and each Worker process.
#### Message Queue: Facilitates the sending of alert messages from the Alerts Watcher to the User Consoles and the sending of responses to User Console requests.
#### The System Manager process also contains a fixed-size data structure called INTERNAL_QUEUE, which stores data or commands to be processed by the Workers.

### Additionally, a log file will be created to store all information for later analysis. All information written to the log should also be displayed on the screen.
