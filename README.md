# Sumo Robot: BotZilla

Hamida Firoz • Nila Mathivannan • Chloe Chow

---

# What Does the Robot Do and How Does It Compete?

make this sound a bit better: BotZilla is designed ram into opposing robots using a large black angled slope at the front 3D printed. The robot uses front and rear surface detectors to identify the arena boundary and prevent driving out of the ring.

The robot also uses an HC-SR04 ultrasonic sensor to detect opponents. When an opponent is detected, the robot advances toward it and attempts to push or flip it using the slope mechanism.

* Stay inside the arena
* Detect and track opponents
* Attack when an opponent is within range
* Recover from dangerous positions near the boundary

## Extensions Beyond the Guided Build

* Front and rear edge-detection sensors
* HC-SR04 ultrasonic distance sensor
* Custom movement logic:

  * Front sensor detects edge → reverse and turn left
  * Rear sensor detects edge → move forward
* Enhanced opponent detection and pursuit behavior

---

# Design

## What the Robot Needs to Do

The robot must:

2. Detect the arena boundary using front and rear sensors.
3. Reverse and turn when the front sensor detects the edge.
4. Move forward when the rear sensor detects the edge.
5. Drive forward, backward, left, and right reliably.
6. Detect opponents using an HC-SR04 ultrasonic sensor.

---

| Requirement                                | How We Tested It                                                                                                               | Pass Condition                                                                        |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| Detect boundary and react appropriately    | Ran the robot over a white surface with a black boundary line. Tested from front, back, and angled approaches (5 trials each). | Robot responds correctly every time and remains within the arena.                     |
| Motors operate correctly                   | Tested wheel movement, direction, and speed over 5 trials.                                                                     | Both motors spin smoothly in the correct direction and maintain consistent speed.     |
| Front sensor avoidance                     | Placed a black boundary line directly in front of the robot and ran 5 trials.                                                  | Robot reverses and turns left within 1 second in every trial.                         |
| Rear sensor avoidance                      | Backed the robot toward a black boundary line and ran 5 trials.                                                                | Robot immediately moves forward every time the rear sensor detects the edge.          |
| Front wedge can effectively push opponents | Placed a stationary object of similar weight to a competition robot in front of BotZilla and performed 5 pushing trials.       | Robot successfully pushes the object at least 30 cm in all 5 trials without stalling. |
| Ultrasonic distance sensing                | Placed objects at 10 cm, 20 cm, 30 cm, and 50 cm from the sensor. Recorded 5 readings at each distance.                        | Distance readings remain within ±2 cm of the actual distance in all trials.           |


---

## System Architecture

### State Machine Diagram

[State Machine Diagram]<img width="2126" height="1664" alt="StateMachineDiagram drawio" src="https://github.com/user-attachments/assets/591e5438-8fe2-4db1-a296-08c8d9996d5c" />


**Caption:** State machine showing robot behaviors and sensor-triggered transitions.

---

## Circuit Design

![Circuit Schematic](images/circuit-schematic.png)

**Caption:** Complete circuit schematic showing GPIO assignments, power rails, resistor values, motors, sensors, and servo connections.

---

## Physical Layout

![Physical Layout](images/physical-layout.png)

**Caption:** Top-down chassis layout showing component placement and cable routing paths.

### Design Decisions

* Sensors positioned at front and rear for reliable edge detection.
* Battery placement optimized to keep the center of mass low.
* Cable routing minimized to reduce interference and tangling.

---

# Build

## Chassis

We began by creating approximately five different robot concepts before selecting the final design.

### Design Evolution

#### Prototype 1: Vertical Claw
<img width="432" height="348" alt="image" src="https://github.com/user-attachments/assets/b240e7d3-1bf5-4000-8cfc-66f75be0f4f6" />


Our first concept used a claw that moved vertically to push opponents out of the arena. While functional in theory, the mechanism was overly complicated.

#### Prototype 2: Bulldozer Design
<img width="428" height="180" alt="image" src="https://github.com/user-attachments/assets/a81b8953-1e24-4e37-acd1-cf800796cee2" />

Our second design used a bulldozer-style front attachment to push opponents. Although simpler and effective, we felt it did not fully utilize the capabilities of a three-person team.

#### Final Design: Flipping Arm
<img width="527" height="343" alt="image" src="https://github.com/user-attachments/assets/c7eae92f-9b0a-4086-8237-304b789ff3cb" />

The final design combines aggressive pushing with a servo-powered flipping arm. We originally considered a pulley system but switched to a servo motor because it:

* Reduced weight
* Simplified construction
* Provided sufficient pushing force
* Improved reliability

### Build Photos

![Early Chassis]
**Caption:** Orginally, we used the chasis provided, but soon realised that we would have to go bigger to fit 4 motors.

![Final Chasis]
**Caption:** We extended the base to 7.5 inches by 8.5 inches to include two photoresistor (front/back), and two motors on each side in order to have enough "ramming" power.

![Servo Mount](images/servo-mount.jpg)

**Caption:** Servo motor mounted using zipties only. The mount is fixed because there are holes within the chasis to secure the motors and zipties as one.

### Changes During Construction

Several changes were made during assembly:

* Wheels were moved further back to better accommodate both photo resistors.
* White motors originally selected for the design failed despite soldering and repair attempts (yellow ones used).
* All motors were replaced with yellow geared motors for improved reliability.

---

## Wiring

### Wiring Strategy

Wires were cut tosize and stripped to make them flush to the breadboard.

Key considerations:

* Separation of motor power wiring from GPIO pin wiring using a 4 AA battery holder pack
* Minimizing loose connections by cutting wires to size
* Preventing cables from interfering with moving parts by ziptying hanging cables to the breadboard when necessary

### Wiring Photos

![Early Wiring]
<img width="525" height="448" alt="image" src="https://github.com/user-attachments/assets/4b55f1c2-0b9b-457f-bab4-13947f2b58ba" />

**Caption:** Early wiring stage showing partially completed electronics and sensor integration.

![Final Wiring]
<img width="486" height="196" alt="image" src="https://github.com/user-attachments/assets/05a6ccb4-a30b-4767-aa5c-732e4aa0d4b5" />

**Caption:** Final wiring layout showing secured cables and completed electrical connections.

---

## Decisions Made During the Build

### Decision 1: Added Additional Motors

Testing revealed that increased drive power improved pushing force and maneuverability.

### Decision 2: Added Ultrasonic Sensor

Opponent detection significantly improved autonomous behavior and reduced time spent searching.

### Decision 2: Added Photoresistor Sensor on the back

If pushed by another robot, could easily find its way away from the black line, due to photoresitors on both the back and front.

---

# Calibration

Edge detection thresholds were calibrated using measurements collected directly from the competition arena.

## RC Timing Measurements

| Surface | Mean RC Time | Range / Std Dev | Samples |
|----------|----------|----------|----------|
| Arena Surface (White) | 1150 | ±120 | 20 |
| Boundary Line (Black) | 3850 | ±250 | 20 |
| Comparison Surface (Wood Table) | 2250 | ±180 | 20 |

### Selected Threshold

**Chosen threshold:** `FRONT_THRESHOLD = 2500`
`BACK_THRESHOLD = 2500`

The white arena surface consistently produced readings around 1150, while the black boundary line produced readings around 3850. A threshold of 2500 was selected because it lies approximately halfway between the two averages and provides a sufficient margin to distinguish the boundary from the arena surface. The comparison surface produced values between the two extremes, confirming that the chosen threshold reliably separates black and white surfaces under normal lighting conditions.

### Calibration Photos

![Arena Calibration](images/calibration-arena.jpg)

**Caption:** Robot positioned on the competition ring during calibration.

![Calibration Output](images/calibration-terminal.png)

**Caption:** Terminal output showing RC timing measurements used to determine the threshold.

---

## Project Structure

```text
src/
├── main.py     — Controls sensors, motors, and robot behaviour
```

---

## Edge Detection

```python
def avoid_line(s1, s2):
    set_backward()
    drive(BACKUP_SPEED, BACKUP_SPEED)
    time.sleep(0.5)

    if s1 > LINE_THRESHOLD and s2 <= LINE_THRESHOLD:
        set_turn_right()
    elif s2 > LINE_THRESHOLD and s1 <= LINE_THRESHOLD:
        set_turn_left()

    drive(50, 50)
    time.sleep(0.5)
```

**Explanation:**
This function prevents the robot from driving out of the ring. When one of the line sensors detects the black boundary, the robot backs up and turns away from the edge. We chose to back up and turn rather than just stop because it gets the robot back into the arena faster and lets it continue searching for opponents.

---

## Opponent Detection

```python
def charge():
    set_forward()
    drive(SPEED_FULL, SPEED_FULL)

    while True:
        s1, s2 = read_line_sensors()

        if s1 > LINE_THRESHOLD or s2 > LINE_THRESHOLD:
            avoid_line(s1, s2)
            return

        dist = ultrasonic_distance_cm()

        if dist > CHARGE_DISTANCE_CM * 1.5:
            stop_motors()
            return
```

**Explanation:**
The ultrasonic sensor is used to find opponents. If an object is detected within the set distance, the robot charges forward at full speed. The line sensors are still checked while charging so the robot doesn't accidentally drive out of the arena while attacking.

---

## Main Loop

```python
while True:
    dist = ultrasonic_distance_cm()

    if dist < CHARGE_DISTANCE_CM:
        charge()
        continue

    s1, s2 = read_line_sensors()

    if s1 > LINE_THRESHOLD or s2 > LINE_THRESHOLD:
        avoid_line(s1, s2)
        continue

    wander_step(wiggle_right)
```

**Explanation:**
The main loop controls the robot's behaviour. First it checks for an opponent. If one is detected, it charges. If no opponent is found, it checks the line sensors. If the boundary is detected, it performs an avoidance manoeuvre. Otherwise, it continues wandering around the arena looking for opponents.

---

## GPIO Cleanup

```python
finally:
    stop_motors()

    for pwm in [pwm_a, pwm_b, pwm_c, pwm_d]:
        pwm.stop()

    GPIO.cleanup()
```

**Explanation:**
This code runs when the program ends. It stops all motors and resets the GPIO pins. Without cleanup, the pins could stay active and cause problems the next time the program is run. The `finally` block is used because it runs even if the program crashes or is interrupted unexpectedly.

---

# Competition & Reflection

## Results

| Round   | Placement | Bonus Points | Round Total |
| ------- | --------- | ------------ | ----------- |
| Round 1 | 2         | 0            | 2           |
| Round 2 | 3         | 2            | 5           |
| Round 3 | 2         | 2            | 4           |
| Round 4 | 0         | 0            | 0           |
| Round 5 | 1         | 0            | 1           |
| Round 6 | 2         | 0            | 2           |

### Competition Photo

![Competition Round](images/competition.jpg)

**Caption:** Robot was pushing a competitor out of the ring.

---

## What Worked

The start button functioned reliably throughout competition and never caused issues during operation.

Additional successful features included:

* Stable driving performance
* Effective pushing action
* Correct wandering pattern followed

---

## What Failed

### Edge Detection Threshold

A last-minute physical changes, such as removing the front 3D printed piece, changed sensor readings making the robots go ully out of the ring in one round.

### Power Limitations

The robot occasionally lacked sufficient power when attempting to push heavier opponents or when getting stuck on the edge of the ring.

### Development Timeline

The final code did not have enough time to be tested and optimised due to hardware problems/readings with the photoresistors.

---

## Next Iteration

### Improvement 1: More Development Time

Complete software earlier to allow extensive testing and debugging.

### Improvement 2: Improved Power System

Use a higher-current battery and optimize motor power delivery.

### Improvement 3: Verify Dimensions Earlier

Check size requirements throughout construction to prevent compliance issues.

### Improvement 4: Recalibrate After Hardware Changes

Any sensor or component change should be followed by a full recalibration procedure.

### Improvement 5: Improve Rear Sensor Reliability

Redesign sensor mounting and perform additional testing to improve consistency.
