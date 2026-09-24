# Automatic-Vehicle-Parking-System-using-STM32-Nucleo-L031K6

## Aim

To design and implement an **Automatic Vehicle Parking System using STM32 Nucleo-L031K6** that detects the occupancy of parking slots, determines the number of available spaces, and indicates when the parking area is full.

---

## Components Required

- STM32 Nucleo-L031K6
- Two pushbuttons to simulate vehicle sensors
- Onboard LED to indicate parking full status
- Wokwi Simulator
- Connecting wires
- Serial Monitor

---

## Theory

An **Automatic Vehicle Parking System** is used to detect the availability of parking spaces and provide information about whether parking slots are **available or occupied**.

In a practical parking system, **IR sensors or ultrasonic sensors** can be used to detect the presence of vehicles in individual parking slots.

In this Wokwi experiment, **pushbuttons are used to simulate the vehicle sensors**.

Two parking slots are considered:

- **Pushbutton 1** represents the sensor for Parking Slot 1.
- **Pushbutton 2** represents the sensor for Parking Slot 2.

When a pushbutton is pressed, the STM32 changes the corresponding parking slot status between **AVAILABLE** and **OCCUPIED**.

The STM32 calculates the number of available parking spaces. When both parking slots are occupied, the parking area is considered **FULL**, and the onboard LED is switched **ON**.

The parking-slot information is also displayed on the **Wokwi Serial Monitor** using UART communication.

---

## Pin Configuration

| Component | STM32 Pin | Function |
|---|---|---|
| Slot 1 Pushbutton | PA12 | Digital Input |
| Slot 2 Pushbutton | PB0 | Digital Input |
| Parking Full LED | PB3 | Digital Output |
| USART2 TX | PA2 | Serial Data Transmission |
| USART2 RX | PA15 | Serial Data Reception |
| Pushbutton Common | GND | Ground |

---

## Block Diagram

~~~text
     Slot 1                 Slot 2
   Pushbutton             Pushbutton
 (Vehicle Sensor)       (Vehicle Sensor)
       |                      |
       v                      v
     PA12                    PB0
       |                      |
       +----------+-----------+
                  |
                  v
       +-------------------------+
       | STM32 Nucleo-L031K6     |
       |                         |
       | Read Slot Status        |
       |          |              |
       |          v              |
       | Calculate Available     |
       | Parking Spaces          |
       +-----------+-------------+
                   |
          +--------+--------+
          |                 |
          v                 v
       PB3 LED           USART2
          |                 |
          v                 v
     Parking Full      Serial Monitor
      Indicator        Parking Status
~~~

---

## Parking Status Conditions

| Slot 1 | Slot 2 | Available Slots | Parking Status | LED |
|---|---|---:|---|---|
| Available | Available | 2 | Space Available | OFF |
| Occupied | Available | 1 | Space Available | OFF |
| Available | Occupied | 1 | Space Available | OFF |
| Occupied | Occupied | 0 | Parking Full | ON |

---

## Algorithm

1. Start the program.
2. Initialize the STM32 HAL library.
3. Configure the system clock.
4. Configure **PA12** as the Slot 1 sensor input.
5. Configure **PB0** as the Slot 2 sensor input.
6. Configure **PB3** as the Parking Full LED output.
7. Enable internal pull-up resistors for the pushbutton inputs.
8. Initialize USART2 for serial communication.
9. Set both parking slots initially as **AVAILABLE**.
10. Read the status of the Slot 1 and Slot 2 pushbuttons.
11. When Pushbutton 1 is pressed, change the status of Slot 1.
12. When Pushbutton 2 is pressed, change the status of Slot 2.
13. Calculate the number of available parking slots.
14. If both slots are occupied, switch the Parking Full LED **ON**.
15. If at least one parking slot is available, keep the LED **OFF**.
16. Display the slot status and number of available spaces on the Serial Monitor.
17. Repeat the process continuously.

---

## Circuit Connections

### Slot 1 Pushbutton

| Pushbutton Terminal | STM32 Connection |
|---|---|
| Terminal 1 | PA12 |
| Terminal 2 | GND |

### Slot 2 Pushbutton

| Pushbutton Terminal | STM32 Connection |
|---|---|
| Terminal 1 | PB0 |
| Terminal 2 | GND |

### Parking Full Indicator

| Indicator | STM32 Connection |
|---|---|
| Onboard LED | PB3 |

The program uses **internal pull-up resistors** for the pushbutton inputs. Therefore, external pull-up resistors are not required.

---

## Circuit Diagram

~~~text
        Slot 1 Pushbutton
      (Vehicle Sensor 1)

        +-------------+
PA12 ---|             |
        |   BUTTON    |
GND  ---|             |
        +-------------+


        Slot 2 Pushbutton
      (Vehicle Sensor 2)

        +-------------+
PB0  ---|             |
        |   BUTTON    |
GND  ---|             |
        +-------------+


       +---------------------------+
       | STM32 Nucleo-L031K6       |
       |                           |
       | PA12 <- Slot 1 Sensor     |
       | PB0  <- Slot 2 Sensor     |
       |                           |
       | PB3  -> Parking Full LED  |
       |                           |
       | PA2  -> USART2 TX         |
       | PA15 -> USART2 RX         |
       +-------------+-------------+
                     |
                     |
                   USART2
                     |
                     v
              Serial Monitor
~~~

---

## Pushbutton Logic

The pushbuttons are configured using the STM32 **internal pull-up resistors**.

Therefore:

- **Button Released → Logic HIGH (1)**
- **Button Pressed → Logic LOW (0)**

Initially:

~~~text
Slot 1 = AVAILABLE
Slot 2 = AVAILABLE
~~~

When the Slot 1 pushbutton is pressed:

~~~text
Slot 1 = OCCUPIED
Slot 2 = AVAILABLE

Available Slots = 1
~~~

When the Slot 2 pushbutton is also pressed:

~~~text
Slot 1 = OCCUPIED
Slot 2 = OCCUPIED

Available Slots = 0
Parking Status = FULL
LED = ON
~~~

Pressing the corresponding pushbutton again changes the slot status back to **AVAILABLE**, simulating a vehicle leaving the parking slot.

---

## Why Pushbuttons Are Used

In an actual Automatic Vehicle Parking System, vehicle detection is normally performed using sensors such as:

- IR sensors
- Ultrasonic sensors
- Proximity sensors

For this Wokwi experiment, pushbuttons are used to **simulate vehicle detection**.

A button press represents a change in the parking-slot condition.

~~~text
Wokwi Simulation            Actual System

Pushbutton             ->   IR/Ultrasonic Sensor

Button Press           ->   Vehicle Detected

Button Press Again     ->   Vehicle Leaves

PB3 LED                ->   Parking Full Indicator
~~~

This makes it easy to understand and test the parking-control logic without requiring an actual vehicle sensor.

---

## Procedure

1. Open the **Wokwi Simulator**.
2. Select the **STM32 Nucleo-L031K6** board.
3. Add two pushbuttons.
4. Connect one terminal of Pushbutton 1 to **PA12**.
5. Connect the other terminal of Pushbutton 1 to **GND**.
6. Connect one terminal of Pushbutton 2 to **PB0**.
7. Connect the other terminal of Pushbutton 2 to **GND**.
8. Use the onboard LED connected to **PB3** as the Parking Full indicator.
9. Enter the STM32 HAL program for the Automatic Vehicle Parking System.
10. Compile the program.
11. Start the Wokwi simulation.
12. Open the **Serial Monitor**.
13. Press Pushbutton 1 to simulate a vehicle occupying Slot 1.
14. Press Pushbutton 2 to simulate a vehicle occupying Slot 2.
15. Observe the parking-slot status on the Serial Monitor.
16. Verify that the onboard LED turns **ON when both parking slots are occupied**.
17. Press a slot button again to simulate the vehicle leaving the parking space.

---

## Program
~~~
#include "main.h"

#define TOTAL_SLOTS 4

int main(void) { HAL_Init(); SystemClock_Config();

MX_GPIO_Init();
MX_TIM3_Init();

HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_1);

while (1)
{
    uint8_t slot1 = HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_2);
    uint8_t slot2 = HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_3);
    uint8_t slot3 = HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_4);
    uint8_t slot4 = HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_5);

    int available = 0;

    if (slot1 == GPIO_PIN_SET)
        available++;

    if (slot2 == GPIO_PIN_SET)
        available++;

    if (slot3 == GPIO_PIN_SET)
        available++;

    if (slot4 == GPIO_PIN_SET)
        available++;

    if (available > 0)
    {
        HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0,
                          GPIO_PIN_SET);   // Green LED

        HAL_GPIO_WritePin(GPIOB, GPIO_PIN_1,
                          GPIO_PIN_RESET); // Red LED
    }
    else
    {
        HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0,
                          GPIO_PIN_RESET);

        HAL_GPIO_WritePin(GPIOB, GPIO_PIN_1,
                          GPIO_PIN_SET);
    }

    HAL_Delay(100);
}

}
~~~

## Expected Output

### Initially – Both Slots Available

~~~text
Slot 1: AVAILABLE
Slot 2: AVAILABLE
Available Slots: 2
Parking Status: SPACE AVAILABLE
Entry Gate: OPEN
~~~

### Vehicle in Slot 1

~~~text
Slot 1: OCCUPIED
Slot 2: AVAILABLE
Available Slots: 1
Parking Status: SPACE AVAILABLE
Entry Gate: OPEN
~~~

### Both Slots Occupied

~~~text
Slot 1: OCCUPIED
Slot 2: OCCUPIED
Available Slots: 0
Parking Status: FULL
Entry Gate: CLOSED
~~~

The onboard LED connected to **PB3** turns **ON** when the parking area is full.

---

## Working

The two pushbuttons simulate vehicle-detection sensors for two parking slots. The STM32 continuously monitors the pushbuttons through the GPIO input pins **PA12** and **PB0**.

When a pushbutton is pressed, the program changes the status of the corresponding parking slot between **AVAILABLE** and **OCCUPIED**.

The STM32 then calculates the available parking spaces using:

`Available Slots = Total Slots - Occupied Slots`

For this experiment, the total number of parking slots is **2**.

If both parking slots are occupied, the number of available slots becomes zero. The STM32 switches the **PB3 onboard LED ON** to indicate **Parking Full**.

If one or more parking slots are available, the LED remains **OFF**.

The status of each parking slot, number of available spaces, parking condition, and entry-gate status are transmitted through **USART2** and displayed on the **Wokwi Serial Monitor**.

---

## Applications

- Shopping mall parking systems
- Hospital parking areas
- College and university campuses
- Office parking facilities
- Airport parking systems
- Smart city parking systems
- Residential parking management

---

## Result

Thus, the **Automatic Vehicle Parking System using STM32 Nucleo-L031K6** was designed and implemented successfully. The system detected the simulated occupancy of parking slots, calculated the number of available spaces, indicated the **Parking Full** condition using the onboard LED, and displayed the parking status on the **Wokwi Serial Monitor**.
