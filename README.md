  # terabee_sensor_contorl

- teraranger github

[Terabee/teraranger](https://github.com/Terabee/teraranger)

- teraranger_array github (use this!!)

[Terabee/teraranger_array](https://github.com/Terabee/teraranger_array)

- Sample Code

[Terabee/sample_codes](https://github.com/Terabee/sample_codes#with-teraranger-evo-family)

---

## 0. Building and running the package from source

- If you have ssh key setup for your github account:

```markdown
cd ~/ros_ws/src
git clone git@github.com:Terabee/teraranger_array.git
cd ~/ros_ws
catkin_make
source devel/setup.bash
```

- If you prefer to use https use this set of commands:

```markdown
cd ~/ros_ws/src
git clone https://github.com/Terabee/teraranger_array.git
cd ~/ros_ws
catkin_make
source devel/setup.bash
```

- Dependencies

This package depends on this serial library. To get it, execute the following command:

```markdown
sudo apt-get install ros-<your_distro>-serial
```

---

## <TeraRanger Multiflex>

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f75e3076-fc5d-485e-b91c-c3d595a168c8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f75e3076-fc5d-485e-b91c-c3d595a168c8/Untitled.png)

### 1. Product

[Sensing Kit | 8 Sensor Modules | Custom Configurations](https://www.terabee.com/shop/lidar-tof-multi-directional-arrays/teraranger-multiflex/)

### 2. Info

[TeraRanger Multiflex > Terabee Sensor | 네스켓](https://www.netsket.kr/bbs/board.php?bo_table=tbsensor&wr_id=20&page=2)

### 3. Datasteet

[teraranger-multiflex-rangefinder-datasheet.pdf](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e08370a6-5c5e-4dd4-935b-c61442326115/teraranger-multiflex-rangefinder-datasheet.pdf)

### 4. Specifications

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a548ca2-40f1-4367-a369-16e6db7fa6fe/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a548ca2-40f1-4367-a369-16e6db7fa6fe/Untitled.png)

### 5. Running teraranger_multiflex node

```markdown
rosrun teraranger_array teraranger_multiflex _portname:=/dev/ttyACM0
```

### 6. Sensor test

- 한 번 센서가 죽으면 센서 연결부터 roscore까지 처음부터 진행해야함
적은 충격에서 센서가 잘 끊김
- 센서 정확도
→ 거리: 0.05m ~ 2.0m 확인
→ 센서 놓였을때 지면 기준 약 0.1m부터 0.05이라고 인식함
 → FOV: 20° 확인
- 투명 아크릴 감지 확인

---

## <TeraRanger Evo Mini ToF Array Kit>

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/362738da-98cc-442e-9ef4-35473e4de76e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/362738da-98cc-442e-9ef4-35473e4de76e/Untitled.png)

### 1. product

[TeraRanger Evo Mini Array Kits | Short-range LiDAR ToF distance sensors](https://www.terabee.com/shop/lidar-tof-multi-directional-arrays/teraranger-evo-mini-array/)

### 2. Info

[TeraRanger Evo Mini ToF Array Kit > Terabee Sensor | 네스켓](https://www.netsket.kr/bbs/board.php?bo_table=tbsensor&wr_id=17&page=2)

### 3. Datasheet

[evoMini-array-kits-specifications-1.pdf](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a228219a-fbba-4d11-bad5-43ad567c5608/evoMini-array-kits-specifications-1.pdf)

### 4. Specifications

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c897056-fab6-4fd7-b776-969db7f80dae/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c897056-fab6-4fd7-b776-969db7f80dae/Untitled.png)

### 5. Running TeraRanger Hub Evo & Tower Evo

```python
# using UART Daughterboard:
rosrun teraranger_array teraranger_evo _portname:=/dev/ttyACM0 _baudrate:=921600

# Changing Sensor Configuration
rosrun rqt_reconfigure rqt_reconfigure
```

### 6. Sensor test

- 0,1,6,7번 센서 사용
- 센서 거리 정확도
→ 거리 값: 0.03m ~ 3.3 m 확인
→ 약 0.15m 이하부터는 값이 0.02~0.03정도 오프셋 발생 / 그 이상부터는 정확하게 값 측정
     ex) 13cm일 경우 값이 0.10~0.11 값으로 뜸
→ FOV: 27° 확인
- 유리 감지 확인
- 모듈 사용에 있어서 4개 or 8개의 센서 값만 받아들여짐
- multisensor보다 성능이 좋은 것으로 추정(천장까지 높이 측정)
- 파라미터 launch 파일 코드

```html
<launch>
  <!-- Create namesapce to avoid naming collisions if launching several drivers -->
  <group ns="hub_1">
      <node pkg="teraranger_array" type="teraranger_evo" name="hub_parser" output="screen">
          <!-- Set the serial port name -->
          <param name="portname" value="/dev/ttyACM0" />
					
					# Rate parm: default=ASAP, 5Hz. 다른 값으로 변경해도 그만큼의 높은 주기로 값을 받아오지는 않음
          <!-- Set the output frequency 0=ASAP, 1=50Hz, 2=100Hz, 3=250Hz--> 
          <param name="Rate" value="0" />

					# Sequence mode parm: crosstalk 모드에서는 16Hz, anti_crosstalk에서는 4Hz
          <!-- Set the sequence mode 0=Crosstalk, 1=Anti_crosstalk-->
          <param name="Sequence_mode" value="1" /> 
          <!-- Set the IMU mode 0=OFF, 1=QUAT, 2=EULER, 3=QUAT+LIN-->
          <param name="IMU_mode" value="0" />

					# hub_evo_mini는 3번 parm
          <!-- Set the sensor type of each port 0=EVO_600HZ, 1=EVO_60M-->
          <param name="Sensor_type_port_0" value="3" />
          <param name="Sensor_type_port_1" value="3" />
          <param name="Sensor_type_port_2" value="0" />
          <param name="Sensor_type_port_3" value="0" />
          <param name="Sensor_type_port_4" value="0" />
          <param name="Sensor_type_port_5" value="0" />
          <param name="Sensor_type_port_6" value="3" />
          <param name="Sensor_type_port_7" value="3" />

          <!-- Remapping topic when using converter nodes-->
          <remap from="ranges" to="ranges_raw" />
      </node>
  </group>
</launch>
```

### 7. CAD with FOV

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b607133b-e80e-4e8d-9f9a-d3cd68c8b0d4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b607133b-e80e-4e8d-9f9a-d3cd68c8b0d4/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4bf1f8ad-40cb-4e77-89fe-848ea0826ba2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4bf1f8ad-40cb-4e77-89fe-848ea0826ba2/Untitled.png)

- 연장 센서 선 1m → 센서 허브 위치 고정이 안됐기 때문에 아직 정확한 계산 미정

    - 센서 로봇 최소 높이 약 0.07m 이상
    차 앞 범퍼 높이 0.2~0.3m 사이 → 약 0.35~0.4정도가 적당할 것으로 추정
