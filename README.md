# ciconia-mbed

Packages for microcontroller that is used in Ciciona (a hybrid VTOL aircraft)

## ciconia-mbed-cube-f103

A microcontroller, STM32F103C8T6, is used to read PWM values from Cube Orange, to drive Dynamixel servos, and control the drop mechanism. The figure, illustrates the configuration of the processor LQFP48 which is used in STM32F103C8T6. First of all, in the context of clock configuration, the system clock is set as 32MHz which is efficient to perform the given tasks. By setting the AHB (Advanced High-Performance Bus) Prescaler to 2, the clock speed of APB1 and APB2 (Advanced Peripheral Bus) is set to 16 MHz. Regarding Figure 5.4, the serial wire debugger pin is opened at the PA14 pin. Because there is no modes for reading both the rising edge and falling edge in the processor of LQFP48, a PWM output is connected to two different channels. Therefore, we had to use 2 different timers in this processor. Timer 2 and timer 4 are enabled and all channels are set to input capture direct mode to read PWM signals. Polarity selection is done as the rising edge for the half, and falling edge for the rest of the channels. The global interrupt mode is enabled for both timers in order to interrupt the code when a falling edge or rising edge is detected. Also, the prescaler and counter period parameters for each timer are set as 16-1 and 20000-1 respectively to divide 16 MHz in order to increment a counter by one each 10\mu s at 50Hz signal frequency.

![mc_config](https://github.com/konyalioglu/ciconia-mbed/blob/main/mc_config.png)

On the other hand, USART3 is enabled in the mode of half-duplex to drive Dynamixel servo motors. Baudrate is set to 57600 Bits/s, the word length is set to 8 bits, parity is set to none, and stop bits are set to 1. Also, the global interrupt mode for this peripheral is activated. Finally, PA10 and PA9 pins are enabled as GPIO output for the relay to open or close the locking mechanism.


## ciconia-mbed

This package is created to establish communication with NVIDIA Jetson and STM32F429ZITx microcontroller. Embedded computers can be insufficient to use them for driving bldc and obtaining data using serial communication from a device that uses non-standard baud-rates, for instance, SBUS protocol. Therefore, there is a need for some bridge. In short, using this package, the MC can receive PWM signals from embedded computer and transmit SBUS data to embedded computer after retrieving from RC controller.
