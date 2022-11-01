# ESE519_Lab2B

1. The GIF is shown here:

    ![a](https://github.com/ZhijingY/ESE519_Lab2B/blob/main/Zhijing_LabB.gif)

    The Code of modified blink.c is shown below:
    
        #include "pico/stdlib.h"

        int main() {
            const uint LED_PIN = 22;
            gpio_init(LED_PIN);
            gpio_set_dir(LED_PIN, GPIO_OUT);
            while (true) {
                gpio_put(LED_PIN, 1);
                sleep_ms(250);
                gpio_put(LED_PIN, 0);
                sleep_ms(250);
            }
        }

2. I am planning to build a small security system protorype with red LED, along with a sensor (or a camera module probably). When an object is detected approaching, I would expect the LEDs to flash. I think it'd be cool because this will be fun to demo. Also it can work with IoT, like uploading the trespassing information onto internet.

3. I would request several LEDs and a (or a few) speaker. A camera also if possible.

4. Will it be accpetable to borrow a small camera module? Are we allowed to implement other MCUs in our design?


# Update on Oct.31st

## Components used for protoboard work

- RP2040
- APDS9960
- 510 Ohm resistor
- Red LED
- Protoboard
- Wires

## Peripheral used

- GPIO
- I2C1

## How the prototype works

This prototype is a simple security system. The proximity sensor on APDS9960 will be enabled upon starting, and it will sense the proximity of objects and send the value to RP2040 via I2C. Once the detected proximity value is over 50, the red LED on board will blink. The schematic and demo GIF is shown below.

## Schematic

![a](https://github.com/ZhijingY/ESE519_Lab2B/blob/main/lab2B_prototype_Schematic.png)

## Demo GIF

![a](https://github.com/ZhijingY/ESE519_Lab2B/blob/main/ezgif.com-gif-maker.gif)
