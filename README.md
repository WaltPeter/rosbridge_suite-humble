rosbridge_suite (mod)
===============

[![ROS Humble version](https://img.shields.io/ros/v/humble/rosbridge_suite)](https://index.ros.org/p/rosbridge_suite/github-RobotWebTools-rosbridge_suite/#humble)


### Building and Installing This Custom Version

This section explains how to uninstall existing rosbridge installations and build your custom version with QoS support from this local repository.

#### Uninstalling Existing Installations

**1. Uninstall APT Packages:**
```bash
sudo apt remove ros-humble-rosbridge-library ros-humble-rosbridge-msgs ros-humble-rosbridge-server ros-humble-rosbridge-suite ros-humble-rosapi ros-humble-rosapi-msgs
```

**2. Uninstall Previous Source Builds:**
If you previously built rosbridge from source in `/opt/ros/humble/src/`:
```bash
cd /opt/ros/humble/src/
rm -rf rosbridge_suite
```

**3. Uninstall Pip Installations:**
If you installed rosbridge via pip:
```bash
pip3 uninstall rosbridge-suite
```

#### Building from Local Repository

**Custom repository location:** `~/rosbridge_suite`

**1. Build with Colcon:**
```bash
cd ~/rosbridge_suite-humble
rosdep install -i --from-path . --rosdistro humble -y
colcon build --symlink-install
```

**2. Source** Installation:

Add to your `~/.bashrc` for persistence:
```bash
echo "source ~/rosbridge_suite-humble/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Or source temporarily in your current terminal:
```bash
source ~/rosbridge_suite-humble/install/setup.bash
```

**3. Verify Installation:**
```bash
python3 -c "import rosbridge_library; print(rosbridge_library.__file__)"
```

Expected output: `/home/YOUR_USERNAME/rosbridge_suite-humble/install/...`

#### Rebuilding After Code Changes

After making changes to the source code:
```bash
cd ~/rosbridge_suite-humble
colcon build --symlink-install
source install/setup.bash
```

The `--symlink-install` flag creates symbolic links, allowing you to edit source files directly without rebuilding the entire package.

#### Updating from Upstream

To pull latest changes from the upstream humble branch:
```bash
cd ~/rosbridge_suite-humble
git fetch origin humble
git pull origin humble
colcon build --symlink-install
source install/setup.bash
```

**Note:** If you have uncommitted changes, use `git stash` before pulling.

#### Custom Features

This build includes custom QoS (Quality of Service) parameter support for rosbridge subscriptions, allowing clients to specify:
- Reliability: `reliable` or `best_effort`
- Durability: `volatile` or `transient_local`
- Depth: Queue size (integer)

Example usage in rosbridge JSON:
```json
{
  "op": "subscribe",
  "topic": "/camera/image_raw",
  "qos": {
    "reliability": "best_effort",
    "durability": "volatile",
    "depth": 5
  }
}
```

---
Original Readme
---

#### Server Implementations of the rosbridge v2 Protocol

rosbridge provides a JSON interface to ROS, allowing any client to send JSON to publish or subscribe to ROS topics, call ROS services, and more. rosbridge supports a variety of transport layers, including WebSockets and TCP. For information on the protocol itself, see the [rosbridge protocol specification](ROSBRIDGE_PROTOCOL.md).

For full documentation, see [the ROS wiki](http://ros.org/wiki/rosbridge_suite).

This project is released as part of the [Robot Web Tools](https://robotwebtools.github.io/) effort.

### Packages

 * [rosbridge_suite](rosbridge_suite) is a [ROS meta-package](http://www.ros.org/wiki/catkin/conceptual_overview#Metapackages_and_the_Elimination_of_Stacks) including all the rosbridge packages.

 * [rosbridge_library](rosbridge_library) contains the Python API that receives JSON-formatted strings as input and controls ROS publishers/subscribers/service calls according to the content of the JSON strings.

 * [rosbridge_server](rosbridge_server) contains a WebSocket server implementation that exposes the rosbridge_library.

 * [rosapi](rosapi) provides service calls for getting meta-information related to ROS like topic lists as well as interacting with the Parameter Server.

### Clients

A rosbridge client is a program that communicates with rosbridge using its JSON API. rosbridge clients include:

 * [roslibjs](https://github.com/RobotWebTools/roslibjs) - A JavaScript API, which communicates with rosbridge over WebSockets.
 * [jrosbridge](https://github.com/WPI-RAIL/jrosbridge) - A Java API, which communicates with rosbridge over WebSockets.
 * [roslibpy](https://github.com/gramaziokohler/roslibpy) - A Python API, which communicates with rosbridge over WebSockets.
 * [roslibrust](https://github.com/Carter12s/roslibrust) - A Rust API, which communicates with rosbridge over WebSockets.

### License
rosbridge_suite is released with a BSD license. For full terms and conditions, see the [LICENSE](LICENSE) file.

### Authors
See the [AUTHORS](AUTHORS.md) file for a full list of contributors.

### Quality Declaration
This package claims to be in the **Quality Level 3** category, see the [Quality Declaration](/QUALITY_DECLARATION.md) for more details.

### Troubleshooting

See the [TROUBLESHOOTING](TROUBLESHOOTING.md) doc for common problems and solutions.

### Release process

Releasing requires push access to [RobotWebTools/rosbridge_suite](https://github.com/RobotWebTools/rosbridge_suite) as well as [ros2-gbp/rosbridge_suite-release](https://github.com/ros2-gbp/rosbridge_suite-release). For more details on how the release process works, see the [bloom tutorial](https://docs.ros.org/en/galactic/How-To-Guides/Releasing-a-ROS-2-package-with-bloom.html).

1. Run `catkin_generate_changelog` to update CHANGELOG.rst files.
2. Manually edit and clean up the changelogs. Commit the changes.
3. Run `catkin_prepare_release --bump [major/minor/patch]` to bump versions in package.xml and push changes to origin.
4. Run bloom-release commands to create PRs to update rosdistro:
    - `bloom-release --rosdistro foxy --track foxy rosbridge_suite`
    - `bloom-release --rosdistro galactic --track galactic rosbridge_suite`
    - `bloom-release --rosdistro rolling --track rolling rosbridge_suite`

Once the PRs are merged, packages will be available for each distro after the next sync. Build/sync status can be viewed at: [foxy](http://repo.ros2.org/status_page/ros_foxy_default.html), [galactic](http://repo.ros2.org/status_page/ros_galactic_default.html), [rolling](http://repo.ros2.org/status_page/ros_rolling_default.html).
