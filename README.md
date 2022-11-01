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

## Code

        #include <stdio.h>
#include <string.h>
#include "pico/stdlib.h"
#include "pico/binary_info.h"
#include "hardware/pio.h"
#include "hardware/i2c.h"

int addr = 0x39;

static void APDS9960_reset() {
    // Two byte reset. First byte register, second byte data
    // There are a load more options to set up the device in different ways that could be added here
    uint8_t buf[] = {0x80, 0x25};
    i2c_write_blocking(i2c1, addr, buf, 2, false);
}

static void mpu6050_read_raw(int8_t *temp) {
    // For this particular device, we send the device the register we want to read
    // first, then subsequently read from the device. The register is auto incrementing
    // so we don't need to keep sending the register we want, just the first.

    uint8_t buffer[1];

    // Start reading acceleration registers from register 0x3B for 6 bytes
    uint8_t val = 0x9C;
    i2c_write_blocking(i2c1, addr, &val, 1, true); // true to keep master control of bus
    i2c_read_blocking(i2c1, addr, buffer, 1, false);

    *temp = buffer[0];
}

int main() {
    const uint SDA_PIN = 22;
    const uint SCL_PIN = 23;
    const uint LED_PIN = 29;

    stdio_init_all();

    i2c_init(i2c1, 200*1000);
    gpio_set_function(SDA_PIN, GPIO_FUNC_I2C);
    gpio_set_function(SCL_PIN, GPIO_FUNC_I2C);
    gpio_pull_up(SDA_PIN);
    gpio_pull_up(SCL_PIN);
    // Make the I2C pins available to picotool
    bi_decl(bi_2pins_with_func(SDA_PIN, SCL_PIN, GPIO_FUNC_I2C));
    APDS9960_reset();

    // init LED gpio
    gpio_init(LED_PIN);
    gpio_set_dir(LED_PIN, GPIO_OUT);

    int8_t temp = 50;

    while (true) {     
        mpu6050_read_raw(&temp);
        if (temp > 50) {
            gpio_put(LED_PIN, 1);
        } else {
            gpio_put(LED_PIN, 0);
        }
        sleep_ms(10);
    }
}

