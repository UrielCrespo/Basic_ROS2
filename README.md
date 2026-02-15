# Challenge 1 – ROS2 Signal Generator & Processor

This project implements a basic **ROS2 (Python)** signal processing pipeline with two nodes:

* **`signal_generator`**: generates a sinusoidal signal and publishes:

  * `/signal` (Float32)
  * `/time` (Float32)
* **`signal_processor` (process)`**: subscribes to `/signal`and`/time`, processes the signal (phase shift, half amplitude, positive offset), and publishes:

  * `/proc_signal` (Float32)

Includes:

* Visualization with **rqt_plot**
* Node/topic graph with **rqt_graph**
* A **launch file** to run everything with a single command

---

## 📦 Requirements

* Ubuntu 22.04 (recommended)
* ROS2 Humble (or compatible distro)
* Python 3

Install visualization tools (if not installed):

```bash
sudo apt update
sudo apt install ros-humble-rqt-plot ros-humble-rqt-graph
```

> If you use another ROS2 distro (iron, jazzy), replace `humble` with your version.

---

## 📁 Project Structure

```
challenge1ALU32/
├── src/
│   └── signal_processing/
│       ├── launch/
│       │   └── challenge1_launch.py
│       ├── signal_processing/
│       │   ├── signal_generator.py
│       │   └── signal_process.py
│       ├── package.xml
│       └── setup.py
├── build/
├── install/
└── log/
```

---

## 🚀 Installation & Run (Step by Step)

### 1) Clone or unzip the project

Place the `challenge1ALU32` folder in your home directory (or any path you prefer).

```bash
cd ~
```

---

### 2) Source ROS2

In **every terminal** you use:

```bash
source /opt/ros/humble/setup.bash
```

---

### 3) Build the workspace

```bash
cd ~/challenge1ALU32
colcon build --symlink-install
source install/setup.bash
```

Verify executables:

```bash
ros2 pkg executables | grep signal_processing
```

Expected output:

```text
signal_processing signal_generator
signal_processing process
```

---

## ▶️ Option A: Run everything with one command (recommended)

Using the **launch file**:

```bash
cd ~/challenge1ALU32
source install/setup.bash
ros2 launch signal_processing challenge1_launch.py
```

This launches:

* `signal_generator`
* `signal_processor (process)`
* `rqt_plot` (plotting `/signal/data` and `/proc_signal/data`)
* `rqt_graph` (node/topic graph)

---

## ▶️ Option B: Run nodes manually (multi-terminal)

**Terminal 1**

```bash
cd ~/challenge1ALU32
source install/setup.bash
ros2 run signal_processing signal_generator
```

**Terminal 2**

```bash
cd ~/challenge1ALU32
source install/setup.bash
ros2 run signal_processing process
```

**Terminal 3 (visualization)**

```bash
cd ~/challenge1ALU32
source install/setup.bash
ros2 run rqt_plot rqt_plot /signal/data /proc_signal/data
```

To open the graph:

```bash
rqt_graph
```

---

## 🔎 Useful ROS2 Commands

List nodes:

```bash
ros2 node list
```

List topics:

```bash
ros2 topic list
```

Echo data:

```bash
ros2 topic echo /signal
ros2 topic echo /proc_signal
```

---

## 📸 Expected Results

* In `rqt_plot`:

  * Original signal `/signal` (sine wave)
  * Processed signal `/proc_signal` (phase shift + half amplitude + positive offset)
* In `rqt_graph`:

  * Pipeline: `signal_generator` → (`/signal`, `/time`) → `signal_processor` → `/proc_signal`

---

## 🧯 Troubleshooting

**“Package not found”**

```bash
source /opt/ros/humble/setup.bash
source ~/challenge1ALU32/install/setup.bash
```

**Launch file not found**

```bash
colcon build --symlink-install
source install/setup.bash
```

**`rqt_plot` / `rqt_graph` not opening**

```bash
sudo apt install ros-humble-rqt-plot ros-humble-rqt-graph
```

---

## 🧠 Technical Notes

* ROS2 executables are defined in `setup.py` using `entry_points`.
* The `.py` filename is not the executable name.
* The launch file orchestrates nodes and tools with a single command.

---

## 📄 License

Educational project for academic purposes.
