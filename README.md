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

2. I am planning to build a small security system protorype with red LEDs and a speaker, along with a sensor (or a camera module probably). When an object is detected approaching, I would expect the LEDs to flash and the speaker to scream. I think it'd be cool because this will be fun to demo. Also it can work with IoT, like uploading the trespassing information onto internet.

3. I would request several LEDs and a (or a few) speaker. A camera also if possible.

4. Will it be accpetable to borrow a small camera module? Are we allowed to implement other MCUs in our design?
