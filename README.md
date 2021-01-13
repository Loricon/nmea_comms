nmea_comms
==========

Generalized ROS interface to NMEA-speaking devices. 

Provides a serial node and socket server node, which relay CRLF-terminated strings on `rx` and `tx`
ROS topics using the [nmea_msgs/Sentence](http://docs.ros.org/latest-available/api/nmea_msgs/html/msg/Sentence.html) message type.


## 安装二进制nmea-comms

```bash
sudo apt-get install ros-melodic-nmea-comms
```
配置launch文件
```bash
<launch>
  <arg name="port" default="/dev/ttyUSB0" />
  <arg name="baud" default="115200" />

  <group ns="navsat">
    <node pkg="nmea_comms" type="serial_node" name="nmea_serial">
      <param name="port" value="$(arg port)" />
      <param name="baud" value="$(arg baud)" />
    </node>
    <node pkg="nmea_comms" type="socket_node" name="nmea_socket">
      <!-- Transmit sentences received from the serial port to clients connected
           to the socket connection. -->
      <remap from="nmea_sentence" to="socket_in" />
      <remap from="nmea_sentence_out" to="nmea_sentence" />
    </node>
  </group>
</launch>
```
