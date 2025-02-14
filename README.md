# Automated-Plant-Watering-System
## Abstract
This project presents the development of an Automated Plant Monitoring System designed to optimize energy usage and ensure effective irrigation. The system harnesses renewable solar energy, employing a 12V solar panel as the primary power source. A current sensor monitors the charging current for a lead-acid battery, ensuring safe and efficient operation. An MPPT (Maximum Power Point Tracking) controller regulates the solar panel's output, dynamically adjusting a connected buck converter's PWM input to implement the three-stage charging process critical for lead-acid batteries.
A secondary buck converter steps down the battery voltage to 5V to power the Arduino-based MPPT controller. This controller integrates three soil moisture sensors and three relay-controlled water pumps to automate irrigation based on soil conditions. The system efficiently manages energy distribution and enhances plant care, combining renewable energy utilization with precise moisture monitoring and irrigation control. This solution is suitable for sustainable agricultural applications, reducing manual intervention while optimizing energy and water resources

## Introduction
The need for sustainable and efficient agricultural systems has never been more critical as the global demand for food continues to grow. Automated plant monitoring and irrigation systems offer a solution by optimizing resource utilization and reducing manual labor. This project introduces an Automated Plant Monitoring System that combines renewable energy, smart sensors, and automated control to monitor and maintain soil moisture levels effectively
The system harnesses solar energy as its primary power source, utilizing a 12V solar panel and a lead-acid battery for energy storage. An integrated Maximum Power Point Tracking (MPPT) controller ensures maximum energy extraction from the solar panel under varying environmental conditions. The MPPT controller also manages the three-stage charging process of the battery, ensuring its longevity and safe operation
Connected to the controller are three soil moisture sensors and three relay-controlled water pumps. The sensors continuously monitor soil moisture levels, providing real-time feedback to the system. When the soil moisture falls below a specified threshold, the corresponding pump is activated via a relay module to irrigate the plants.
This project demonstrates the integration of renewable energy, smart monitoring, and automated irrigation in a cost-effective and scalable system. It is designed for small-scale farming, gardens, and other agricultural applications, emphasizing sustainability and energy efficiency.

## Block Diagram

![Block Diagram](8.png)

## Components Used
### BOM

![BOM](5.png)

## Circuit Design
### Schematic

![Schematic](1.png)

### Step-By-Step Circuit Analysis
#### Solar Panel Input

![solar input](9.png)

- Two Wires connects the +12V Solar Panel to the Designed System.

 #### Current Sensor Design

 ![current sensor](10.png)

 - A shunt resistor method is used to measure the current from the solar panel.
 - A maximum current of 5A can be extracted from the solar panel when charging.
 - A 0.01 ohm resistor is used.
 - Since arduino can only read voltage:
 - - v = i x r ==> v = 5 x 0.01 = 0.05v.
   - The arduino can't read this small amount, so we have to amplify it.
   - A Non-Inverting Amplifier (LM358) with Gain 100 is designed with Rf = 99kohm and Ri = 1kohm.
- The power dissipated: P = I x I x R = 25 x 0.01  = 25W
- To eliminate external noise from the circuit input, a passive low-pass filter is designed:
- - fc = 1 / (2pi x Rshunt x Cin) = 1 / (2pi x 0.01 x 10uF) = 1.6 KHz.
- A 0.01uF capacitor is connected to the output of the sensor to filter out noise from the Amplifier. 

#### Buck Converter Design for Charging the Battery

![Buck](11.png)

##### Charging Modes for Lead-Acid Battery:
Charging a lead-acid battery involves three distinct phases:
1. Bulk Charge (Constant Current Mode):
- - In this phase, the battery is charged at a constant current until the voltage reaches the absorption voltage level (usually around 14.4V to 14.8V for a 12V battery).
  - This is the fastest charging phase and replenishes most of the battery's capacity.
  - Current is the primary focus during this stage, while the voltage gradually rises.
2. Absorption Charge (Constant Voltage Mode):
- - Once the absorption voltage is reached, the charger maintains this voltage.
  - During this phase, the current gradually decreases as the battery becomes more fully charged.
  - This stage ensures the battery is charged safely without overcharging and allows the internal chemical reactions to stabilize.
3. Float Charge (Maintenance Mode):
- - After the battery is fully charged, the voltage is reduced to a lower level, typically around 13.2V to 13.8V, to prevent overcharging.
  - This phase compensates for the self-discharge of the battery and keeps it ready for use without damaging it.
  - It is ideal for batteries that remain connected to a charger for long periods.
##### Buck Converter (Charger):
1. Arduino PWM and BC557 Transistor Control:
- - The Arduino generates a PWM signal to control the buck converter.
  - A 100-ohm resistor is connected between the PWM output pin of the Arduino and the base of the BC557 (a PNP transistor).
  - The BC557 acts as a switch to regulate the power flow based on the PWM signal.
  - When the PWM signal is LOW, the BC557 transistor turns ON, allowing current to flow from the Panel+ terminal through the circuit.
2. MOSFET (IRFZ44NPBF) and Zener Diode:
- - The IRFZ44N MOSFET is the main switching component of the buck converter.
  - A 12V Zener diode connected to the MOSFET's gate ensures that the gate voltage stays within safe limits, protecting the MOSFET from excessive voltage.
  - The MOSFET switches ON and OFF rapidly, controlled by the Arduino's PWM signal passed through the BC557.
3. Schottky Diode and Inductor:
- - A Schottky diode is connected to the source terminal of the MOSFET to prevent backflow of current and ensure unidirectional energy transfer to the load.
  - The 33uH inductor is placed in series with the circuit to smooth the current flow and store energy during the OFF cycle of the MOSFET.
  - During the OFF phase, the inductor releases stored energy, ensuring a continuous flow of current to the battery.
4. Output Filtering;
- - A 470uF capacitor is connected after the inductor to filter out voltage ripples and stabilize the DC output voltage.
  - A second Schottky diode prevents reverse current from flowing back into the inductor or MOSFET, ensuring the battery is safely charged.
5. Battery Connection:
- - The output of the buck converter is directly connected to the terminals of the lead-acid battery.
  - The buck converter adjusts its output voltage and current to suit the battery's charging phase based on the Arduino's MPPT algorithm.

#### Panel Voltage Monitoring

![Panel monitor](12.png)

- A voltage divider ise configured to measure the voltage from the solar panel.
- Vsensed = Vsolar (R2 / R1 + R2).
- Vsensed = 12V (160/160+220) =  5V.
- if the arduino reads 5V, then the Panel is generating 12V.

#### Battery Monitor Unit

![Battery monitor](13.png)

- The same voltage divider is configured as above.
- a Transistor switch is added, so that the user can choose when to read the battery's level.

#### 5V Buck Converter
- The 5V Buck converter is used to step down the the 12V from the battery to 5V to power up the arduino.
- Press the link below to see the full explanation.
[5V Buck converter](https://github.com/MaherAlaaDeen/PCB-Designs/blob/main/Project%208/README.md)

#### Soil Sensors Configuration

![Soil](14.png)

#### Relay Module

![Relay](15.png)

![Relay OUT](18.png)

- The relay module is used to turn ON an associated pump to irrigate the plant.
- Press on the link below to see the full explanation.
- [Relay](https://github.com/MaherAlaaDeen/PCB-Designs/blob/main/Project%205/READMED.md)

#### OLED

![OLED](17.png)

- The OLED is Embedded on the PCB in order to tell the user the Battery percentage during charging.

#### Control Unit

![control](16.png)

## PCB Design
### PCB Routing
#### Top Layer

![top](2.png)

#### Bottom Layer

![Bottom](3.png)

### PCB Layers
#### Top Layer

![top](6.png)

#### Bottom Layer

![bottom](7.png)

### PCB 3D View

![3D](4.png)

## Arduino Code
- Include libraries
```c
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
```

- OLED display width and height
```c
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
```

- Declaration for an SSD1306 display connected using I2C
```c
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);
```

- Battery Constants
```c
#define bulk_voltage_max 14.5
#define bulk_voltage_min 13
#define absorption_voltage 14.7
#define float_voltage_max 13
#define battery_min_voltage 10
#define solar_min_voltage 19
#define charging_current 2.0
#define absorption_max_current 2.0
#define absorption_min_current 0.1
#define float_voltage_min 13.2
#define float_voltage 13.4
#define float_max_current 0.12
#define OLED_refresh_rate 1000
byte BULK = 0;        // Give values to each mode
byte ABSORPTION = 1;
byte FLOAT = 2;
byte mode = 0;        // Start with mode 0 BULK
```

- Inputs
```c
#define solar_voltage_in A7
#define solar_current_in A1
#define battery_voltage_in A0
#define Soil1 A2
#define Soil2 A3
#define Soil3 A6
```

- Outputs
```c
#define PWM_out 3
#define load_enable 2 //VCTRL
#define Pump1 4
#define Pump2 5
#define Pump3 6
```

- Variables
```c
// Define moisture threshold
#define MOISTURE_THRESHOLD 250

// Variables
float bat_voltage = 0;
int pwm_value = 0;
float solar_current = 0;
float current_factor = 0.185;       // Value defined by manufacturer ACS712 5A
float solar_voltage = 0;
float solar_power = 0;
String load_status = "OFF";
int pwm_percentage = 0;
unsigned int before_millis = 0;
unsigned int now_millis = 0;
String mode_str = "BULK";
```

- Setup
```c
void setup() {
  pinMode(solar_voltage_in, INPUT);    // Set pins as inputs
  pinMode(solar_current_in, INPUT);
  pinMode(battery_voltage_in, INPUT);

  pinMode(PWM_out, OUTPUT);            // Set pins as outputs
  digitalWrite(PWM_out, LOW);          // Set PWM to LOW so MOSFET is off
  pinMode(load_enable, OUTPUT);
  digitalWrite(load_enable, LOW);      // Start with the relay turned off
  pinMode(Soil1, INPUT);
  pinMode(Soil2, INPUT);
  pinMode(Soil3, INPUT);
  pinMode(Pump1, OUTPUT);
  pinMode(Pump2, OUTPUT);
  pinMode(Pump3, OUTPUT);
  digitalWrite(Pump1, LOW);
  digitalWrite(Pump2, LOW);
  digitalWrite(Pump3, LOW);


  TCCR1B = TCCR1B & B11111000 | B00000001; // Timer 1 PWM frequency of 31372.55 Hz
  Serial.begin(9600);

  // Initialize OLED
  if (!display.begin(SSD1306_I2C_ADDRESS, 0x3C)) { // Address 0x3C for most OLEDs
    Serial.println(F("SSD1306 allocation failed"));
    for (;;);
  }
  display.clearDisplay();
  display.display();
  before_millis = millis;     // Used for OLED refresh rate
}
```

- Loop
```c
void loop() {
  solar_voltage = get_solar_voltage(15);
  bat_voltage = get_battery_voltage(15);
  solar_current = get_solar_current(15);
  solar_power = bat_voltage * solar_current;
  pwm_percentage = map(pwm_value, 0, 255, 0, 100);

  now_millis = millis();
  if (now_millis - before_millis > OLED_refresh_rate) {
    before_millis = now_millis;

    // Clear the OLED display
    display.clearDisplay();

    // Display solar and battery voltages
    display.setTextSize(1);
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(0, 0);
    display.print("Solar: ");
    display.print(solar_voltage, 2);
    display.print("V  Battery: ");
    display.print(bat_voltage, 2);
    display.println("V");

    // Display solar current and load status
    display.setCursor(0, 10);
    display.print("Current: ");
    display.print(solar_current, 2);
    display.print("A  Load: ");
    display.print(load_status);

    // Display solar power and PWM percentage
    display.setCursor(0, 20);
    display.print("Power: ");
    display.print(solar_power, 2);
    display.print("W  PWM: ");
    display.print(pwm_percentage);
    display.println("%");

    // Display the current mode
    display.setCursor(0, 30);
    display.print("Mode: ");
    display.print(mode_str);

    // Update the OLED display
    display.display();
  }

  if (bat_voltage < battery_min_voltage) {
    digitalWrite(load_enable, LOW);        // Disable the load if battery is undervoltage
    load_status = "OFF";
  } else {
    digitalWrite(load_enable, HIGH);       // Enable the load if battery charged
    load_status = "ON";
  }

  // Manage modes and PWM adjustment
  if (mode == FLOAT) {
    if (bat_voltage < float_voltage_min) {
      mode = BULK;
      mode_str = "BULK";
    } else {
      if (solar_current > float_max_current) {
        mode = BULK;
        mode_str = "BULK";
      } else {
        if (bat_voltage > float_voltage) {
          pwm_value--;
          pwm_value = constrain(pwm_value, 0, 254);
        } else {
          pwm_value++;
          pwm_value = constrain(pwm_value, 0, 254);
        }
      }
      analogWrite(PWM_out, pwm_value);
    }
  } else {
    if (bat_voltage < bulk_voltage_min) {
      mode = BULK;
      mode_str = "BULK";
    } else if (bat_voltage > bulk_voltage_max) {
      mode_str = "ABSORPTION";
      mode = ABSORPTION;
    }

    if (mode == BULK) {
      if (solar_current > charging_current) {
        pwm_value--;
        pwm_value = constrain(pwm_value, 0, 254);
      } else {
        pwm_value++;
        pwm_value = constrain(pwm_value, 0, 254);
      }
      analogWrite(PWM_out, pwm_value);
    }

    if (mode == ABSORPTION) {
      if (solar_current > absorption_max_current) {
        pwm_value--;
        pwm_value = constrain(pwm_value, 0, 254);
      } else {
        if (bat_voltage > absorption_voltage) {
          pwm_value++;
          pwm_value = constrain(pwm_value, 0, 254);
        } else {
          pwm_value--;
          pwm_value = constrain(pwm_value, 0, 254);
        }
        if (solar_current < absorption_min_current) {
          mode = FLOAT;
          mode_str = "FLOAT";
        }
      }
      analogWrite(PWM_out, pwm_value);
    }
  }
  checkSoilMoisture(SOIL_SENSOR_1, PUMP_1);
  checkSoilMoisture(SOIL_SENSOR_2, PUMP_2);
  checkSoilMoisture(SOIL_SENSOR_3, PUMP_3);

  // Debugging: print soil moisture levels
  Serial.print("Sensor 1: ");
  Serial.println(analogRead(SOIL_SENSOR_1));
  Serial.print("Sensor 2: ");
  Serial.println(analogRead(SOIL_SENSOR_2));
  Serial.print("Sensor 3: ");
  Serial.println(analogRead(SOIL_SENSOR_3));

  delay(1000); // Wait for 1 second before the next reading
}
```

- Get solar voltage function
```c
float get_solar_voltage(int n_samples) {
  const float V_REF = 5.0; // Reference voltage of the Arduino ADC
  const int ADC_RESOLUTION = 1023; // 10-bit ADC resolution
  const float DIVIDER_SCALING = 0.421; // Scaling factor for the voltage divider
  
  float voltage = 0;
  
  for (int i = 0; i < n_samples; i++) {
    // Read raw ADC value and convert to input pin voltage
    float adc_voltage = analogRead(solar_voltage_in) * (V_REF / ADC_RESOLUTION);
    // Scale up to the actual solar panel voltage
    voltage += adc_voltage / DIVIDER_SCALING;
  }

  // Average the sampled voltage
  voltage = voltage / n_samples;

  // Ensure no negative voltage (shouldn't happen with solar panels)
  if (voltage < 0) {
    voltage = 0;
  }

  return voltage;
}
```

- Get Battery Voltage function
```c
float get_battery_voltage(int n_samples) {
  const float V_REF = 5.0; // Reference voltage of the ADC
  const int ADC_RESOLUTION = 1023; // 10-bit ADC resolution
  const float DIVIDER_SCALING = 0.421; // Voltage divider scaling factor
  
  float voltage = 0;

  // Enable load
  digitalWrite(load_enable, HIGH);
  
  // Read and accumulate voltage samples
  for (int i = 0; i < n_samples; i++) {
    float adc_voltage = analogRead(battery_voltage_in) * (V_REF / ADC_RESOLUTION);
    voltage += adc_voltage / DIVIDER_SCALING;
  }

  // Average the voltage samples
  voltage = voltage / n_samples;

  // Ensure no negative voltage (shouldn't happen with a battery)
  if (voltage < 0) {
    voltage = 0;
  }

  return voltage;
}
```

- get solar current
```c
float get_solar_current(int n_samples) {
  float Sensor_voltage;
  float current = 0;
  float offset_voltage = 2.5; // Adjust this value if needed (e.g., for zero current)
  for (int i = 0; i < n_samples; i++) {
    // Read the voltage from the A7 pin and scale it to the 0-5V range
    Sensor_voltage = analogRead(A7) * (5.0 / 1023.0);
    
    // Subtract the offset voltage (e.g., 2.5V is a baseline for zero current)
    current += (Sensor_voltage - offset_voltage) / current_factor;
  }
  
  // Calculate the average current
  current = current / n_samples;
  
  // Ensure that current is non-negative
  if (current < 0) { current = 0; }
  
  return current;
}
```

- Check Moisture Function
```c
void checkSoilMoisture(int sensorPin, int pumpPin) {
  int moistureLevel = analogRead(sensorPin); // Read the soil moisture sensor value

  if (moistureLevel < MOISTURE_THRESHOLD) {
    // Soil needs water, turn on the pump
    digitalWrite(pumpPin, HIGH);
  } else {
    // Soil is sufficiently moist, turn off the pump
    digitalWrite(pumpPin, LOW);
  }
}
```

## Advantages of the Automated Plant Monitoring System with MPPT Controller
1. Efficient Energy Utilization: The MPPT (Maximum Power Point Tracking) controller ensures that the solar panel operates at its maximum power point, maximizing the energy harvested and minimizing waste.
2. Battery Longevity: The three-phase charging process (Bulk, Absorption, and Float) optimizes the charging cycle for the lead-acid battery, preventing overcharging and undercharging, thereby extending the battery's lifespan.
3. Sustainable Energy Source: The use of solar energy eliminates dependency on grid power, making the system environmentally friendly and suitable for remote areas.
4. Automated Plant Monitoring: The integration of soil moisture sensors and automated pumps ensures timely watering of plants, reducing manual intervention and promoting healthier plant growth.
5. Cost-Effectiveness: The system uses readily available components like Arduino, resistors, transistors, and diodes, making it an affordable solution compared to commercial alternatives.
6. Scalability and Flexibility:
- - The system can be easily expanded by adding more sensors or relays to monitor and control additional plants or devices.
  - It is also adaptable for use with other types of batteries or renewable energy sources.
7. Power Efficiency: The buck converters reduce the voltage levels efficiently, minimizing power losses and ensuring optimal performance for both the Arduino and the charging process.
8. Real-Time Monitoring and Control: The Arduino monitors voltage, current, and soil moisture levels in real time, ensuring precise control over the system's operation.
9. Reliability in Remote Applications: By utilizing solar power and battery storage, the system is reliable in remote or off-grid locations where electricity supply may be unavailable or intermittent.
10. Low Maintenance: Once installed, the system requires minimal maintenance. Components like the battery and sensors are durable and designed for long-term use.
11. Enhanced Plant Care: Automated watering based on real-time soil moisture readings prevents overwatering or underwatering, ensuring optimal conditions for plant health.
12. Eco-Friendly Design: The project promotes the use of renewable energy and reduces reliance on non-renewable energy sources, contributing to environmental sustainability.
This combination of renewable energy harvesting, intelligent charging management, and plant monitoring automation makes the project a versatile and impactful solution for energy and agricultural applications.

## Conclusion
The Automated Plant Monitoring System with MPPT Controller successfully integrates renewable energy harvesting, intelligent power management, and automated irrigation to address key challenges in sustainable agriculture and energy utilization. By employing an MPPT controller, the system maximizes the efficiency of solar power usage, ensuring that the lead-acid battery is charged optimally through a carefully designed three-phase charging process. This not only prolongs the battery's lifespan but also enhances the overall reliability of the system.
The automated irrigation feature, driven by real-time soil moisture sensing and relay-controlled pumps, ensures precise water delivery to plants, minimizing waste and promoting healthier growth. The use of readily available components like the Arduino microcontroller and off-the-shelf electronics makes the system cost-effective, scalable, and accessible to a broad audience.
This project demonstrates a practical approach to integrating renewable energy with smart automation, making it suitable for both residential and remote agricultural applications. By addressing the dual needs of energy efficiency and sustainable plant care, it contributes to the broader goals of environmental sustainability and resource optimization. Future enhancements could include the addition of remote monitoring capabilities or integration with advanced sensors for even greater versatility.
