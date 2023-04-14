# **Information Processing Logbook

# Lab 1 - Introduction

# Lab 2 - Design a NIOS II System



## Hardware Design

Connect to the ports: 

![https://lh6.googleusercontent.com/EQFxbKgVfFap_PUC2BXst_qAMV3qiILaO97jZtCwL0E-ycvnAA9mNjIhAeCuzAjh9wBM0VUp8_8g7ll80AtiS8zKbgyCn5Bhel46HcswGacURxkaGGn7lAuD1Xf5ZBt-QGYah7-3MK24acyaLQ8f8ds](https://lh6.googleusercontent.com/EQFxbKgVfFap_PUC2BXst_qAMV3qiILaO97jZtCwL0E-ycvnAA9mNjIhAeCuzAjh9wBM0VUp8_8g7ll80AtiS8zKbgyCn5Bhel46HcswGacURxkaGGn7lAuD1Xf5ZBt-QGYah7-3MK24acyaLQ8f8ds)

![https://lh5.googleusercontent.com/dC9x7LjdhJ81wPcGAf_Lhjr-8IoNKsQU2rvRvEEXg09abhILJbEqVjbc3zg8cMqd4EcchEqi2qpvD1KzYiNk4oNF70TFhE2ovg3QjXD24iaT7dm5pF3UgmJ6E-hu-9WiWpfoIq-Zq-rQYDesTB8f9EI](https://lh5.googleusercontent.com/dC9x7LjdhJ81wPcGAf_Lhjr-8IoNKsQU2rvRvEEXg09abhILJbEqVjbc3zg8cMqd4EcchEqi2qpvD1KzYiNk4oNF70TFhE2ovg3QjXD24iaT7dm5pF3UgmJ6E-hu-9WiWpfoIq-Zq-rQYDesTB8f9EI)

## Software Design

```c
#include <sys/alt_stdio.h>
#include <stdio.h>
#include "altera_avalon_pio_regs.h"
#include "system.h"

int main()
{
	int switch_datain;
	alt_putstr("Hello from Nios II!\n");
	alt_putstr("When you press Push Button 0,1 the switching on of the LEDs is done by software\n");
	alt_putstr("But, Switching on/off of LED 2 by SW 2 is done by hardware\n");
	/* Event loop never exits. Read the PB, display on the LED */

	while (1)
	{
		//Gets the data from the pb, recall that a 0 means the button is pressed
		switch_datain = ~IORD_ALTERA_AVALON_PIO_DATA(BUTTON_BASE);
		//Mask the bits so the leftmost LEDs are off (we only care about LED3-0)
		switch_datain &= (0b0000000011);
		//Send the data to the LED
		IOWR_ALTERA_AVALON_PIO_DATA(LED_BASE,switch_datain);

	}
	return 0;
}
```

![https://lh4.googleusercontent.com/25z5fyBArVEG9jwdbIS-uEFkmVvRwjd9V-piRFU_RMYZE1-mEuxt2V9ZBhI_DMFezV7C3rmDv2P51pU5lcTumieiQGrcmnfc8ayykE-6kaK3T7gNRKOkC3DqxopLlfsemUuQmdAbKGVjtyK2LpK9208](https://lh4.googleusercontent.com/25z5fyBArVEG9jwdbIS-uEFkmVvRwjd9V-piRFU_RMYZE1-mEuxt2V9ZBhI_DMFezV7C3rmDv2P51pU5lcTumieiQGrcmnfc8ayykE-6kaK3T7gNRKOkC3DqxopLlfsemUuQmdAbKGVjtyK2LpK9208)

Compile, blast, download .elf:

![https://lh4.googleusercontent.com/FROzo1rQqA3MWPdRn43ZAO_3d1ovJJ17uvyFqPQ9BKzHUWnSBSpOt2UupN3pTqmstANEtvieDXL9cKDpLi_JfW5YzAsPan1vOwHVsZxydfvufgZkjsnHVi19eg-OzkDFEQdCtRosoybcOtmsi268ocQ](https://lh4.googleusercontent.com/FROzo1rQqA3MWPdRn43ZAO_3d1ovJJ17uvyFqPQ9BKzHUWnSBSpOt2UupN3pTqmstANEtvieDXL9cKDpLi_JfW5YzAsPan1vOwHVsZxydfvufgZkjsnHVi19eg-OzkDFEQdCtRosoybcOtmsi268ocQ)

## Challenge: LED sequence

![https://lh4.googleusercontent.com/apLbhiZB0boQeStaUSJz7Al_MpeZVSxwEKO-666rSxl9JYubZu9y538scPeWF7SLtUsw1NkDe-tLd8Ybv8c-l2Dep74csWQRRGzaHPLeESxzQEaZoAihU0EPsLbDxETzhK1TKGuVzLSVYdpVlS3CGOk](https://lh4.googleusercontent.com/apLbhiZB0boQeStaUSJz7Al_MpeZVSxwEKO-666rSxl9JYubZu9y538scPeWF7SLtUsw1NkDe-tLd8Ybv8c-l2Dep74csWQRRGzaHPLeESxzQEaZoAihU0EPsLbDxETzhK1TKGuVzLSVYdpVlS3CGOk)

![https://lh3.googleusercontent.com/F9eIsN9WEXJ3vXe5Hw_L2qvRQLPbRQMf5zFjwUzgsFQi8h1JK1XKN39bLRsCY6BFn9Lj3E2DCe73sKc8tQbnBMBMLdkm8M4ddJOigZqHZjGprRDu5xcuCbP3iw7FQol8RXpmcxXgyWi6BV02WB61AvM](https://lh3.googleusercontent.com/F9eIsN9WEXJ3vXe5Hw_L2qvRQLPbRQMf5zFjwUzgsFQi8h1JK1XKN39bLRsCY6BFn9Lj3E2DCe73sKc8tQbnBMBMLdkm8M4ddJOigZqHZjGprRDu5xcuCbP3iw7FQol8RXpmcxXgyWi6BV02WB61AvM)

The above code is one way to implement it. The code uses a state machine to allow the user to switch the LED flashing sequence and pattern.

# Lab 3 - Integrate an Accelerometer with NIOS



## 1 NIOS II System

![https://lh4.googleusercontent.com/aDvpMxTmJbtq8qPTWIAIl6hDPMWOpeemOnE9_0eHXHdQDhOIVAZpoGbQqzZrX-aKSU5-qDRLUvJAUlMuaGKDehtDw3Rxsl0rvjV-PdnCYI1U0oOJzUrQcZwwBq28QprPgnwaQZh9CMc0Qdh0hGDVIeY](https://lh4.googleusercontent.com/aDvpMxTmJbtq8qPTWIAIl6hDPMWOpeemOnE9_0eHXHdQDhOIVAZpoGbQqzZrX-aKSU5-qDRLUvJAUlMuaGKDehtDw3Rxsl0rvjV-PdnCYI1U0oOJzUrQcZwwBq28QprPgnwaQZh9CMc0Qdh0hGDVIeY)

![https://lh5.googleusercontent.com/YvaKzFvO7a_Nk6l7ZGPHncfALXHtWkRMV8MIrlV7G15XY-sc4t-6IfG9VRTYy0VJGH1N_0tKg1DgTcc2330Bi-GKJDxo02IqrJFT5VO2suQISf9KuUyN-IGa8YGdWVJSPKaM_3xeijzLxM49iqOFZyg](https://lh5.googleusercontent.com/YvaKzFvO7a_Nk6l7ZGPHncfALXHtWkRMV8MIrlV7G15XY-sc4t-6IfG9VRTYy0VJGH1N_0tKg1DgTcc2330Bi-GKJDxo02IqrJFT5VO2suQISf9KuUyN-IGa8YGdWVJSPKaM_3xeijzLxM49iqOFZyg)

## 2 Code

```c
// led_accelerometer_main.c

#include "system.h"
#include "altera_up_avalon_accelerometer_spi.h"
#include "altera_avalon_timer_regs.h"
#include "altera_avalon_timer.h"
#include "altera_avalon_pio_regs.h"
#include "sys/alt_irq.h"
#include <stdlib.h>

#define OFFSET -32
#define PWM_PERIOD 16

alt_8 pwm = 0;
alt_u8 led;
int level;

void led_write(alt_u8 led_pattern) {
    IOWR(LED_BASE, 0, led_pattern);
}

void convert_read(alt_32 acc_read, int * level, alt_u8 * led) {
    acc_read += OFFSET;
    alt_u8 val = (acc_read >> 6) & 0x07;
    * led = (8 >> val) | (8 << (8 - val));
    * level = (acc_read >> 1) & 0x1f;
}

void sys_timer_isr() {
    IOWR_ALTERA_AVALON_TIMER_STATUS(TIMER_BASE, 0);

    if (pwm < abs(level)) {

        if (level < 0) {
            led_write(led << 1);
        } else {
            led_write(led >> 1);
        }

    } else {
        led_write(led);
    }

    if (pwm > PWM_PERIOD) {
        pwm = 0;
    } else {
        pwm++;
    }

}

void timer_init(void * isr) {

    IOWR_ALTERA_AVALON_TIMER_CONTROL(TIMER_BASE, 0x0003);
    IOWR_ALTERA_AVALON_TIMER_STATUS(TIMER_BASE, 0);
    IOWR_ALTERA_AVALON_TIMER_PERIODL(TIMER_BASE, 0x0900);
    IOWR_ALTERA_AVALON_TIMER_PERIODH(TIMER_BASE, 0x0000);
    alt_irq_register(TIMER_IRQ, 0, isr);
    IOWR_ALTERA_AVALON_TIMER_CONTROL(TIMER_BASE, 0x0007);

}

int main() {

    alt_32 x_read;
    alt_up_accelerometer_spi_dev * acc_dev;
    acc_dev = alt_up_accelerometer_spi_open_dev("/dev/accelerometer_spi");
    if (acc_dev == NULL) { // if return 1, check if the spi ip name is "accelerometer_spi"
        return 1;
    }

    timer_init(sys_timer_isr);
    while (1) {
    	
        alt_up_accelerometer_spi_read_x_axis(acc_dev, & x_read);
        alt_printf("raw data: %x\n", x_read);

        convert_read(x_read, & level, & led);
    }

    return 0;
}
```

![https://lh6.googleusercontent.com/hKcykHLzyGKE_y9Sb6chD99V1Aafs_qfQRgVvxe1Bht1ou0TZWuQGdumf6kDkFtTtWzVWAUMqG-ujL0C4YP9pEt2qmWA35Gz0Ewhz1UIMU2rbKtLUGa66gJBnOBViYhcrWm9yNvfnfhXaoUvbTapq4g](https://lh6.googleusercontent.com/hKcykHLzyGKE_y9Sb6chD99V1Aafs_qfQRgVvxe1Bht1ou0TZWuQGdumf6kDkFtTtWzVWAUMqG-ujL0C4YP9pEt2qmWA35Gz0Ewhz1UIMU2rbKtLUGa66gJBnOBViYhcrWm9yNvfnfhXaoUvbTapq4g)

![https://lh3.googleusercontent.com/7m5RkIcD6WBzTWqZrN96AMtH1X5do8vPuJ2oQQ7jqpbInsfEhr3X388TFqyXYZBLfHAk_ZiEZ6QXVhyvz7wp5jgKFFarkAUX5FFepllAYAXo6yQNRnoBotPSCcCGmbfFsxNQqHyCbOM-5Y5DXcpcg18](https://lh3.googleusercontent.com/7m5RkIcD6WBzTWqZrN96AMtH1X5do8vPuJ2oQQ7jqpbInsfEhr3X388TFqyXYZBLfHAk_ZiEZ6QXVhyvz7wp5jgKFFarkAUX5FFepllAYAXo6yQNRnoBotPSCcCGmbfFsxNQqHyCbOM-5Y5DXcpcg18)

![https://lh6.googleusercontent.com/_QmE0ot1GS9a8a7I8RHvP-9BkUuOgtyf_qJkAB2VdlF-3uZUogWpefz6MKBmCzUycwVdIc1XQTxhx7yhJJlMRfUyL6Y8WXk0oThaayZOKhehJEWGQI0GcCzD14EnVfuEoI6tYKcgHfifiWr4rfzalU8](https://lh6.googleusercontent.com/_QmE0ot1GS9a8a7I8RHvP-9BkUuOgtyf_qJkAB2VdlF-3uZUogWpefz6MKBmCzUycwVdIc1XQTxhx7yhJJlMRfUyL6Y8WXk0oThaayZOKhehJEWGQI0GcCzD14EnVfuEoI6tYKcgHfifiWr4rfzalU8)

## 3 FIR filter - moving average

![https://lh4.googleusercontent.com/kBLL0l6e9uFiOvkQycUFDojxA1PJo8vAeXugFRuXQiYYyHYWVkjRBHSNt72sMIdeQAl9Wfvk7BDJi7l7VRfbOYZXjFgdor4umYDsDfAxrzBrdzzWKD4bNf0Cjit8ecZ2tvSimIts0PMna00Z0zQQLJI](https://lh4.googleusercontent.com/kBLL0l6e9uFiOvkQycUFDojxA1PJo8vAeXugFRuXQiYYyHYWVkjRBHSNt72sMIdeQAl9Wfvk7BDJi7l7VRfbOYZXjFgdor4umYDsDfAxrzBrdzzWKD4bNf0Cjit8ecZ2tvSimIts0PMna00Z0zQQLJI)

### Filter from Matlab

Coefficients have been optimized to take advantage of fixed point operation speed. See challenge for the optimized version of FIR filter from Matlab.

## (Challenge) Optimize FIR

Idea: instead of calculating in float each time, change coefficient into integer and optimize the efficiency, divide by the sample number in the end; 

![https://lh4.googleusercontent.com/ZZzSIiX9LbOnZdhYwAGdreMi2CpE3Y1mv474EWFaxDQtS_CcmjcGvy5MKfFFSqiOS2uUy1cnY58alnH4Y46JIFLz6trgIbZn_-K0GDzNfFHn--mHuEVGGrCzJ430LjdGkFJxjh2hQH3VxTtMH-2oV5s](https://lh4.googleusercontent.com/ZZzSIiX9LbOnZdhYwAGdreMi2CpE3Y1mv474EWFaxDQtS_CcmjcGvy5MKfFFSqiOS2uUy1cnY58alnH4Y46JIFLz6trgIbZn_-K0GDzNfFHn--mHuEVGGrCzJ430LjdGkFJxjh2hQH3VxTtMH-2oV5s)

### Coefficient array:

```c
int coefficients[49] = {46, 74, -24, -71, 33, 1, -94, 40, 44, -133, 30, 114, -179, -11, 223, -225, -109, 396, -263, -338, 752, -289, -1204, 2879, 6369, 2879, -1204, -289, 752, -338, -263, 396, -109, -225, 223, -11, -179, 114, 30, -133, 44, 40, -94, 1, 33, -71, -24, 74, 46};
```

Filter function:

![https://lh3.googleusercontent.com/sGmMKAgZ37BHKfzRq1OvuepxgMMUJkRkfMV3fWojlRgJyZB3tbZi9OE3rfNpQoYbYV9KvujQqiix0pghkmtv9XPy9Qi8_GYNJDQNRw1oXDuKxc9D7ndebL3rcx-cb_psqja_EIdyjYMerGTxsLb3aUM](https://lh3.googleusercontent.com/sGmMKAgZ37BHKfzRq1OvuepxgMMUJkRkfMV3fWojlRgJyZB3tbZi9OE3rfNpQoYbYV9KvujQqiix0pghkmtv9XPy9Qi8_GYNJDQNRw1oXDuKxc9D7ndebL3rcx-cb_psqja_EIdyjYMerGTxsLb3aUM)

Note: x is a global array keeping accelerometer data, on which operations are applied to calculate the filtered value.

Findings:

Perceived latency in LED response is significantly improved after implementing fixed point operation.

## Measuring sampling frequency

![https://lh5.googleusercontent.com/aP7C0QpJLRxsp1DVmyFgMks1YmXyNUdNNMNRHvMVFG9XOZPZOqgcAPlH-23pxVlgU5YSf5Qv9jm1wN2jtsq2UYYiRnIw0lPKqRiQOTu7lf8hDK32t1qkfDiNWCc26lwY7SwJSnkdzAXlQCtpHOHIFWU](https://lh5.googleusercontent.com/aP7C0QpJLRxsp1DVmyFgMks1YmXyNUdNNMNRHvMVFG9XOZPZOqgcAPlH-23pxVlgU5YSf5Qv9jm1wN2jtsq2UYYiRnIw0lPKqRiQOTu7lf8hDK32t1qkfDiNWCc26lwY7SwJSnkdzAXlQCtpHOHIFWU)

# Lab 4 - UART



## 2 Extend lab 3 system

Change in host side code:

![https://lh6.googleusercontent.com/lgV8itM6BPK2-1An5MPlzdlJ328chOi2X0VsT6mmUWHA1H2paf3uCHF0_GR5XMIcplrBO79xAJOrVHKzFctunXvbTGmgWqY8pdgyBwUb1WoiFA8RHQEGaCZbjoCWC1v77305obTfK5-FE7Cc9BR1kSI](https://lh6.googleusercontent.com/lgV8itM6BPK2-1An5MPlzdlJ328chOi2X0VsT6mmUWHA1H2paf3uCHF0_GR5XMIcplrBO79xAJOrVHKzFctunXvbTGmgWqY8pdgyBwUb1WoiFA8RHQEGaCZbjoCWC1v77305obTfK5-FE7Cc9BR1kSI)

change in board side code:

![https://lh4.googleusercontent.com/nLrIDbUEsiRU47YTwbn4wPdiNUPpm94n4tR7jl13BMSvQjVhknxYwOZ7oOPDTHEgH6_-4x4O79FADdSQioHpLIRCd-0bq7aI3vVyxzEQU_gh_EXfgN_fpMXr70unWd15R3Jc6MFkJqcQ4MOTpv5AJ-M](https://lh4.googleusercontent.com/nLrIDbUEsiRU47YTwbn4wPdiNUPpm94n4tR7jl13BMSvQjVhknxYwOZ7oOPDTHEgH6_-4x4O79FADdSQioHpLIRCd-0bq7aI3vVyxzEQU_gh_EXfgN_fpMXr70unWd15R3Jc6MFkJqcQ4MOTpv5AJ-M)

![https://lh4.googleusercontent.com/AuDYsctm-9JLwRpDi2WPy36zDa1KgkZqyOyTJlfytlrNjb4lGAaWT3vvf9jS9m8GtHtSGMXohRrYxlfqFlZu0KArcQIz_WMolHVxE3mB0H2PSsrlocpKIBP51rrvvpyzaeyAfF1ufKGvnA40lltla5A](https://lh4.googleusercontent.com/AuDYsctm-9JLwRpDi2WPy36zDa1KgkZqyOyTJlfytlrNjb4lGAaWT3vvf9jS9m8GtHtSGMXohRrYxlfqFlZu0KArcQIz_WMolHVxE3mB0H2PSsrlocpKIBP51rrvvpyzaeyAfF1ufKGvnA40lltla5A)

evidence of working accelerometer:

![https://lh5.googleusercontent.com/ZyEBM24AcD1OxYHwYQQpEV2fWwRkjIVJuUjr1NlX2PfAwmIyUbsTbEbkbTMuQCZFpLNnohzeouJf39usgTn1I0Q7S0sp1eXLsuozUGH5TH2Y-2yBLjMr_b6QyKiSHMixShLxUAO-_bGt1jfO-jiHtnA](https://lh5.googleusercontent.com/ZyEBM24AcD1OxYHwYQQpEV2fWwRkjIVJuUjr1NlX2PfAwmIyUbsTbEbkbTMuQCZFpLNnohzeouJf39usgTn1I0Q7S0sp1eXLsuozUGH5TH2Y-2yBLjMr_b6QyKiSHMixShLxUAO-_bGt1jfO-jiHtnA)

# Lab 5 - AWS, SSH Clients, Client Server Programming



## 2.1 Instantiate an EC2 instance

![https://lh6.googleusercontent.com/S-Pz7kvxWkKo4gtVxNUVPm4LZVfdGCiLQiZIrPUJLgqoOzdeM8mkjUE3td16wljQzRLDzlt08sFPbUtwGpXOMA0japqNThEa8PSnoCr8wTddZpK_8DWFku8mCC_lctWHa0iPlSl7zOPsuDHdSFOVf28](https://lh6.googleusercontent.com/S-Pz7kvxWkKo4gtVxNUVPm4LZVfdGCiLQiZIrPUJLgqoOzdeM8mkjUE3td16wljQzRLDzlt08sFPbUtwGpXOMA0japqNThEa8PSnoCr8wTddZpK_8DWFku8mCC_lctWHa0iPlSl7zOPsuDHdSFOVf28)

## 2.2 Use PuTTY to connect to the EC2 instance

### PuTTY

![https://lh6.googleusercontent.com/6wmuGBer2DwrjN8tXHBTfEcCMmCMuDUdqiPRCV44SsriyyN_Xwm6m8ijgG_GqrHbo_mNoHdXvRTZfhUDGMI6Tvje_mE7FqIy-IJnYnvtoQ_VXipleduPKPhKQmkQKImmjdFDhfhg82BDlNx3VAO84dI](https://lh6.googleusercontent.com/6wmuGBer2DwrjN8tXHBTfEcCMmCMuDUdqiPRCV44SsriyyN_Xwm6m8ijgG_GqrHbo_mNoHdXvRTZfhUDGMI6Tvje_mE7FqIy-IJnYnvtoQ_VXipleduPKPhKQmkQKImmjdFDhfhg82BDlNx3VAO84dI)

In the remote machine, run python and linux command:

![https://lh4.googleusercontent.com/v-GlfHJxK5bFmUFGbQpwxVwp7Q4SBmkMl-tMrVme1oF09pfGho3-Pxd9OpWSTYhiM2-Ynw9vWrsAUMXlZ6ebFqfbhQDG7Kahv1wYskll1Uwa5HW4sLvc14pBvFygCgGjM3jECytbDl8SMDb9sFftMVs](https://lh4.googleusercontent.com/v-GlfHJxK5bFmUFGbQpwxVwp7Q4SBmkMl-tMrVme1oF09pfGho3-Pxd9OpWSTYhiM2-Ynw9vWrsAUMXlZ6ebFqfbhQDG7Kahv1wYskll1Uwa5HW4sLvc14pBvFygCgGjM3jECytbDl8SMDb9sFftMVs)

![https://lh5.googleusercontent.com/MiYR1BdyJOJakE8Esp4FqdEDuTGboxX8TNK9W9Wa0rJYukYrIWGB5n2EvWG0tJm-9CWYfud5NuiGbvZD4o47_wLZPSK1-BsK5cCQamCUUyCivjXLNlsRmN15uQy8jbOUHAJzy4alviJq5KcBtIdJnLo](https://lh5.googleusercontent.com/MiYR1BdyJOJakE8Esp4FqdEDuTGboxX8TNK9W9Wa0rJYukYrIWGB5n2EvWG0tJm-9CWYfud5NuiGbvZD4o47_wLZPSK1-BsK5cCQamCUUyCivjXLNlsRmN15uQy8jbOUHAJzy4alviJq5KcBtIdJnLo)

### FileZilla

Connect to the site in Site Manager:

![https://lh3.googleusercontent.com/8UIaNzF77iee8XEY2L7OKleeiR0Vmf7JE2AjsZCERCP6gFwKcIOQJYWMsuP7d1aw0hfM1iVY8GxgdcN5_YLZTw38ppOgkzzD4P_Xt2Ee8sB6tevBT5XQsTcqrG0nR7pMKcxUJ55AkiF3ru8KUHnpxr0](https://lh3.googleusercontent.com/8UIaNzF77iee8XEY2L7OKleeiR0Vmf7JE2AjsZCERCP6gFwKcIOQJYWMsuP7d1aw0hfM1iVY8GxgdcN5_YLZTw38ppOgkzzD4P_Xt2Ee8sB6tevBT5XQsTcqrG0nR7pMKcxUJ55AkiF3ru8KUHnpxr0)

- Protocol = SFTP-SSH File transfer protocol;
- Host = Public IPv4 DNS of instance;
- Port = 22;
- Logon Type = key file;
- User = ubuntu;
- Key file = .ppk file;

Move files to the server using FileZilla:

![https://lh6.googleusercontent.com/N9coWO6i5kWsMclfvBLO8GQEb7e_sDPFZU9w_-e9w9loclN1Uj1aEeHE2pqaa5gaC1kWG-exkFjoEsFVNDhnOLI4AuQc7TDHLmwzYWxPi4d0d_lTIjv_2kNyjAzzGCZjOLf9ys40C2V9DmRua_PLfvQ](https://lh6.googleusercontent.com/N9coWO6i5kWsMclfvBLO8GQEb7e_sDPFZU9w_-e9w9loclN1Uj1aEeHE2pqaa5gaC1kWG-exkFjoEsFVNDhnOLI4AuQc7TDHLmwzYWxPi4d0d_lTIjv_2kNyjAzzGCZjOLf9ys40C2V9DmRua_PLfvQ)

It can then be executed in the remote machine:

![https://lh6.googleusercontent.com/nVznyj-3wp2PJJf71Z5WFd_9H45D8h6x0xFjnd2iMqQmaA5Eyb4Bhhj2_96HNWtm2URgEDGusy-JzxZ4GyYMol36KZeZY4rjipvhDx_K90g_UGnSKjmZygVpFgrVrcuXY5Pqgn40aa5Ka3ezq1UvN20](https://lh6.googleusercontent.com/nVznyj-3wp2PJJf71Z5WFd_9H45D8h6x0xFjnd2iMqQmaA5Eyb4Bhhj2_96HNWtm2URgEDGusy-JzxZ4GyYMol36KZeZY4rjipvhDx_K90g_UGnSKjmZygVpFgrVrcuXY5Pqgn40aa5Ka3ezq1UvN20)

## 2.3 Running a TCP Server on the EC2 instance

### Run .py files on EC2 instance:

Server:

![https://lh4.googleusercontent.com/dswkwCBSroCYRpiv2XF0REy5vRwQWdlvuQZ45udTg1XHh3MPSM4lNms0x5j7SbluN1AQocHUa9MHP_ikTu6f-kjFQEWri1K2lhQDpmDNB8ly63cdEfc7oSAwdZEpn1UCACL9hrW9gzOu7w2P-1Hu5hM](https://lh4.googleusercontent.com/dswkwCBSroCYRpiv2XF0REy5vRwQWdlvuQZ45udTg1XHh3MPSM4lNms0x5j7SbluN1AQocHUa9MHP_ikTu6f-kjFQEWri1K2lhQDpmDNB8ly63cdEfc7oSAwdZEpn1UCACL9hrW9gzOu7w2P-1Hu5hM)

![https://lh4.googleusercontent.com/OBUDOUI0SP_MzTWagyjNhrFcn8pnvZYFx35vHnGK9txLbXYZIjOAUYOy-R97zUIUuVcD5pdM4l9KBUo6_Dsd0e3Ub14OHsCiYA9novDbFnP2jd4rve-8lYjEIT2j3Wme9aoFbemKCcLqwd9LB9IkJog](https://lh4.googleusercontent.com/OBUDOUI0SP_MzTWagyjNhrFcn8pnvZYFx35vHnGK9txLbXYZIjOAUYOy-R97zUIUuVcD5pdM4l9KBUo6_Dsd0e3Ub14OHsCiYA9novDbFnP2jd4rve-8lYjEIT2j3Wme9aoFbemKCcLqwd9LB9IkJog)

Client:

![https://lh4.googleusercontent.com/Yu4tjILsSVqViLmlzfCkQRBCN6kznrfON5sPWAlOh29IYqcYc0XMd53dyRbQtenPVaLARw8AFoieCl1LpCdRLk3lmjUUlLhUQxpxsFW8X9Ra9W0xKs4QmM4oY0Lf2-ZF4yi_IGwHve-ul0tm80H1KRM](https://lh4.googleusercontent.com/Yu4tjILsSVqViLmlzfCkQRBCN6kznrfON5sPWAlOh29IYqcYc0XMd53dyRbQtenPVaLARw8AFoieCl1LpCdRLk3lmjUUlLhUQxpxsFW8X9Ra9W0xKs4QmM4oY0Lf2-ZF4yi_IGwHve-ul0tm80H1KRM)

![https://lh4.googleusercontent.com/lTo81Uafpi4JWktxqeCUHGk_BlpLZChn7uq-OIx3VRjmYfjmzMBCMPmZKbMRIUXYE79IX7eQfwCnc0-dV4xNH2sCUl1xmsSzPXmXDzGC0wZsH_k4VqJwi4SE8-uk4-dy3hwA7Besf36zIOnDUR1NkVE](https://lh4.googleusercontent.com/lTo81Uafpi4JWktxqeCUHGk_BlpLZChn7uq-OIx3VRjmYfjmzMBCMPmZKbMRIUXYE79IX7eQfwCnc0-dV4xNH2sCUl1xmsSzPXmXDzGC0wZsH_k4VqJwi4SE8-uk4-dy3hwA7Besf36zIOnDUR1NkVE)

### Running the TCP Server as a service:

![https://lh6.googleusercontent.com/dqzBIuipHtZ39sjU9RxFOdHqvSj4lNRTs1ZoqbLDMAEJw74LnBvhyLaVVK4SYw5It2vVU1Xy8y9igyTrzqaUigws-8qs8oeg_mMN7fBLi8vhJhYwjkX6nHgkO-S9gZUinHdm1Xr3yFe0NracccuFxvQ](https://lh6.googleusercontent.com/dqzBIuipHtZ39sjU9RxFOdHqvSj4lNRTs1ZoqbLDMAEJw74LnBvhyLaVVK4SYw5It2vVU1Xy8y9igyTrzqaUigws-8qs8oeg_mMN7fBLi8vhJhYwjkX6nHgkO-S9gZUinHdm1Xr3yFe0NracccuFxvQ)

![https://lh6.googleusercontent.com/oB4dGHMS9k35zIwwPEknxCAKgFzCJXJAGapxoBtga0LoXlk7vruB4EnFtNem-xKL-C4k2BWdW27dRBRzh2W9yFUSkrp1ZLI762s1ML3PkV2AbKf6dMh3yacjMgK1Sa0wVamSqaPSHvr-rcFX2V_JADU](https://lh6.googleusercontent.com/oB4dGHMS9k35zIwwPEknxCAKgFzCJXJAGapxoBtga0LoXlk7vruB4EnFtNem-xKL-C4k2BWdW27dRBRzh2W9yFUSkrp1ZLI762s1ML3PkV2AbKf6dMh3yacjMgK1Sa0wVamSqaPSHvr-rcFX2V_JADU)

- reload the services information
- enable service
- start service

![https://lh6.googleusercontent.com/yC8RzvqkKsYRYtH87Y40tF7wACx33RK0BNehNQHsg8XRVrtYjq9b6hrPr60plEOOFRs848PY_6w0p9CBQOuu9ciLLW8TYi6hoc1x3DWDJkbLP7BFD4xjUu4f_v6YPgU3SWrLTV3B9VOPyYyIcU2sdOc](https://lh6.googleusercontent.com/yC8RzvqkKsYRYtH87Y40tF7wACx33RK0BNehNQHsg8XRVrtYjq9b6hrPr60plEOOFRs848PY_6w0p9CBQOuu9ciLLW8TYi6hoc1x3DWDJkbLP7BFD4xjUu4f_v6YPgU3SWrLTV3B9VOPyYyIcU2sdOc)

Communicate between client and server service:

![https://lh6.googleusercontent.com/ELDrgsYBmMsE7jRiK_f-84vzN36P6LEM3rs5tVlygh3wQ1wuiOejOcNJ5e2uTd1Dj1qOPOW0CT2Zu3L4Kd1fL9QElCMtPzCWunWKmiB06OZe4BdjgvIBaqUrJ53NsNprFc1fvTPobJpOqXLVjENAgTY](https://lh6.googleusercontent.com/ELDrgsYBmMsE7jRiK_f-84vzN36P6LEM3rs5tVlygh3wQ1wuiOejOcNJ5e2uTd1Dj1qOPOW0CT2Zu3L4Kd1fL9QElCMtPzCWunWKmiB06OZe4BdjgvIBaqUrJ53NsNprFc1fvTPobJpOqXLVjENAgTY)

To kill a process:

![https://lh4.googleusercontent.com/SaRPyNIaf3uMcToLINKZWpk0Yz6FrsdJYo04bhHyH3vc4kCR79eQghfAopJs7RuE_Xr_6WWITfUNQaTbMIKjteDdLPWMpGtGK3eJ6AQVLKkzDwTfb8Wq1wCojMy97EiKLcku3bRIovOVxt0KiFQ0y_U](https://lh4.googleusercontent.com/SaRPyNIaf3uMcToLINKZWpk0Yz6FrsdJYo04bhHyH3vc4kCR79eQghfAopJs7RuE_Xr_6WWITfUNQaTbMIKjteDdLPWMpGtGK3eJ6AQVLKkzDwTfb8Wq1wCojMy97EiKLcku3bRIovOVxt0KiFQ0y_U)

To kill all python processes:

![https://lh4.googleusercontent.com/6A70X_jgVt0YdApET2V9wc5pEQrE3jOsYW1D4lDW-34znML4pWjlQLiEYIc9JIjzmE8XlRmJaOWOXy_3Txs_4Nb-v4txK44TstigcuRESmnVG3tncf2mJDX9HgBu6BfnSkLtCh-v7RJSYebgcMH6kB4](https://lh4.googleusercontent.com/6A70X_jgVt0YdApET2V9wc5pEQrE3jOsYW1D4lDW-34znML4pWjlQLiEYIc9JIjzmE8XlRmJaOWOXy_3Txs_4Nb-v4txK44TstigcuRESmnVG3tncf2mJDX9HgBu6BfnSkLtCh-v7RJSYebgcMH6kB4)

## 2.4 (Optional) Exercises

### Compute the average RTT

client code:

![https://lh4.googleusercontent.com/TCnBPlo99zBEmZAXevpip8g24r8-NG7XyHJlvJPdnWgGIE91LVw9QuISJLhL2L3az3ZXTWocYnNlWeDyq4krVW84DT8GzRzhEg2r7IsxB9zhgKCXdnEtKJZJ5xNLavg9vgLJcjWOVQm519hG6apCcXc](https://lh4.googleusercontent.com/TCnBPlo99zBEmZAXevpip8g24r8-NG7XyHJlvJPdnWgGIE91LVw9QuISJLhL2L3az3ZXTWocYnNlWeDyq4krVW84DT8GzRzhEg2r7IsxB9zhgKCXdnEtKJZJ5xNLavg9vgLJcjWOVQm519hG6apCcXc)

server code:

![https://lh5.googleusercontent.com/eum746xVHTMUayK7phCmjknQxxmKwi_EdIIkCVAsKxoQtT264MTvZVUAQjnamhVZ7P0gKPLEn7aMPFIiwzkT0xqzbAW-drw1rAeupysUZvl8-6X4z8eSEmFJ6AYHZmkI7mAr2Rj2L9yMRomaf80Eq70](https://lh5.googleusercontent.com/eum746xVHTMUayK7phCmjknQxxmKwi_EdIIkCVAsKxoQtT264MTvZVUAQjnamhVZ7P0gKPLEn7aMPFIiwzkT0xqzbAW-drw1rAeupysUZvl8-6X4z8eSEmFJ6AYHZmkI7mAr2Rj2L9yMRomaf80Eq70)

results:

![https://lh4.googleusercontent.com/wtTWpz8JrVX-RXGz2SR-eFbsCiUSsSehXB5eo_sfE1eVqcPKcCYsF4BJPi3KQa5AgoXpIg6YcpK3kcf5JEQC_yinIG7VopK0cWJQlPpHehV9npmkkRvmaBWkDVEOYayJjYofAgD6EPSF-R7C3u9mqHo](https://lh4.googleusercontent.com/wtTWpz8JrVX-RXGz2SR-eFbsCiUSsSehXB5eo_sfE1eVqcPKcCYsF4BJPi3KQa5AgoXpIg6YcpK3kcf5JEQC_yinIG7VopK0cWJQlPpHehV9npmkkRvmaBWkDVEOYayJjYofAgD6EPSF-R7C3u9mqHo)

![https://lh4.googleusercontent.com/6Y6h9cubZ5I8R0_guovain6xo8L4ZqT4a0iWHm5LV18EOzeI69o-8Tm6kB-A8rbzP8zmuhUxXNG4JOgvxWQWVQ4fnH2m3IQGoL5qnPHrLjit00dF9yTH6iFI0mFvhawdKiW3Z5x8Fhc4oIUUBwqIU_E](https://lh4.googleusercontent.com/6Y6h9cubZ5I8R0_guovain6xo8L4ZqT4a0iWHm5LV18EOzeI69o-8Tm6kB-A8rbzP8zmuhUxXNG4JOgvxWQWVQ4fnH2m3IQGoL5qnPHrLjit00dF9yTH6iFI0mFvhawdKiW3Z5x8Fhc4oIUUBwqIU_E)

### Accept accelerometer data from FPGA board and send to server

This process requires the local host to send data to the remote server. Make the following change to the code: 

```python
#...set up server name and port
def send_server():
	print ("receiving data from fpga...")
	res = send_on_jtag('0')
	print (res)v
  msg = str(res)
  client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  client_socket.connect((server_name, server_port))
  client_socket.send(msg.encode())

def main(): 
	...
	while(1): 
		send_server()

```

# Lab 6 - Using DynamoDB with EC2



Downloading and installing Boto3, which is the AWS SKD for Python:

![https://lh5.googleusercontent.com/mV6rXe8fXL1EvTjUAPhSQo3uhp7vksTM2y99e6O7ogMChQ8vdQYPswKNv650cG8RIIToclRt2WLsbHziXun75IJygSDQZV2Mfp7JzPLP-ba8f0BXOnvCjstAP-qq2j6EDk1ftZyzakeIXaQOnJwPVFM](https://lh5.googleusercontent.com/mV6rXe8fXL1EvTjUAPhSQo3uhp7vksTM2y99e6O7ogMChQ8vdQYPswKNv650cG8RIIToclRt2WLsbHziXun75IJygSDQZV2Mfp7JzPLP-ba8f0BXOnvCjstAP-qq2j6EDk1ftZyzakeIXaQOnJwPVFM)

![https://lh6.googleusercontent.com/Bsdy1bvMg-ipHAIx5EoQpalnVrOs44DEOG27z726SWEGkejTSDiuUORMwlwKXIL6i8Rev2bLR_zpf6PESgw5n2rjd8-iZRyt1Mx3zWN5UyYUGeKv2Tol0jcpmgUp_k7ip9-h1myn-O4O4dqwC1nvRWI](https://lh6.googleusercontent.com/Bsdy1bvMg-ipHAIx5EoQpalnVrOs44DEOG27z726SWEGkejTSDiuUORMwlwKXIL6i8Rev2bLR_zpf6PESgw5n2rjd8-iZRyt1Mx3zWN5UyYUGeKv2Tol0jcpmgUp_k7ip9-h1myn-O4O4dqwC1nvRWI)

## 2.1 Creating a table

python3 MoviesCreateTable.py

![https://lh3.googleusercontent.com/tW74GF-c8CTQAP37EWwnF6535uSR9AOnNeE_hJdkLcq7fbMti302swzToOPMl_aUUfKQlPxMhFbJoLbnZAGmK77c6FggAY33ZZsQpyOoYfELaTxykipppdLWL0tArtF1F2sHAdHNgS1FA_5MgSNJmwo](https://lh3.googleusercontent.com/tW74GF-c8CTQAP37EWwnF6535uSR9AOnNeE_hJdkLcq7fbMti302swzToOPMl_aUUfKQlPxMhFbJoLbnZAGmK77c6FggAY33ZZsQpyOoYfELaTxykipppdLWL0tArtF1F2sHAdHNgS1FA_5MgSNJmwo)

![https://lh5.googleusercontent.com/z4nszfw-HIIYBigeFjDHjZrOhAs8LZ7YVe4viZSw41Oc6gzK9nBDBQAxQ3PMsSEOzL0pUgRg7tjFDPzqqT-pK4j-Mlrq5tctk0HrQ2oL0zbZT-F_aI55ySoH-vQgk68IokN8sJMeGFGaANA4XQqCJ44](https://lh5.googleusercontent.com/z4nszfw-HIIYBigeFjDHjZrOhAs8LZ7YVe4viZSw41Oc6gzK9nBDBQAxQ3PMsSEOzL0pUgRg7tjFDPzqqT-pK4j-Mlrq5tctk0HrQ2oL0zbZT-F_aI55ySoH-vQgk68IokN8sJMeGFGaANA4XQqCJ44)

![https://lh5.googleusercontent.com/jGqrYUmhyofM-MMVq-NgANkSeAoX32gHVq1IF8eyJQYdFNrMG7Mepoe2uybXj_ihI_zC_ZMaAGz_fiaxtQoDx3GCrFQ0plwOdTcA2okc3rcyDKV9P4qs0HOeKSHXtqd-2Tubv78vIsLCtKiVuXTdRMg](https://lh5.googleusercontent.com/jGqrYUmhyofM-MMVq-NgANkSeAoX32gHVq1IF8eyJQYdFNrMG7Mepoe2uybXj_ihI_zC_ZMaAGz_fiaxtQoDx3GCrFQ0plwOdTcA2okc3rcyDKV9P4qs0HOeKSHXtqd-2Tubv78vIsLCtKiVuXTdRMg)

## 2.2 Loading sample data

data is in the form: 

```python
[
	{
		"year" : ... , 
		"title" : ... , 
		"info" : { ... }
	}, 
	{
		"year" : ... , 
		"title" : ... , 
		"info" : { ... }
	},

	...

]
```

![https://lh6.googleusercontent.com/ZYEGrRehfWIFE7cEqfFrpu_mU-CpaahrSV_wX_F-CV8Wudc2Wfw2aJgTPD3yJdp4SxZMBH_gV-qcfPXWkQqtPhZ0u-rQkzc9gPPKE10DCEQjL1Qkxq6uNIUmKf30TeIb8g9VIwQtEavmPCEQaQaXYlQ](https://lh6.googleusercontent.com/ZYEGrRehfWIFE7cEqfFrpu_mU-CpaahrSV_wX_F-CV8Wudc2Wfw2aJgTPD3yJdp4SxZMBH_gV-qcfPXWkQqtPhZ0u-rQkzc9gPPKE10DCEQjL1Qkxq6uNIUmKf30TeIb8g9VIwQtEavmPCEQaQaXYlQ)

```python
~$ python3 MoviesLoadData.py
```

![https://lh4.googleusercontent.com/8X4l7Wuv3WGoJmsm93fg96XpWaZTuQMlKk5z7lxT4YicFnArWBVDEXHElDv_dG64EiqWU73HnOscVqv7jGUOsntWRsSMSxft9qkyAdrTZUyJ5Knbknk-339PnnsEeI3EdYpE26jShQQQQzVe8uC7okI](https://lh4.googleusercontent.com/8X4l7Wuv3WGoJmsm93fg96XpWaZTuQMlKk5z7lxT4YicFnArWBVDEXHElDv_dG64EiqWU73HnOscVqv7jGUOsntWRsSMSxft9qkyAdrTZUyJ5Knbknk-339PnnsEeI3EdYpE26jShQQQQzVe8uC7okI)

![https://lh3.googleusercontent.com/epcnAARkL9fELxW2LG51vwN69mVOaspfuab-n6UhMz7R7I8okG4u6FYmRTGYAwIn_xR2qWlFV6sbBV0CjfpCyA2iuSNJ2NqmA2ZzYnKPycZDE1NOrZ54sp0hYW1_j7O9c-Mmy2WehCh9qdO9__5uirc](https://lh3.googleusercontent.com/epcnAARkL9fELxW2LG51vwN69mVOaspfuab-n6UhMz7R7I8okG4u6FYmRTGYAwIn_xR2qWlFV6sbBV0CjfpCyA2iuSNJ2NqmA2ZzYnKPycZDE1NOrZ54sp0hYW1_j7O9c-Mmy2WehCh9qdO9__5uirc)

## 2.3 Crud operations

### 2.3.1 Create a new item

python3 MoviesItemOps01.py

![https://lh4.googleusercontent.com/BIAmXNfN8b8LpuQd1lP6pWuCW_bGx3hee0wuBoFbyBM5aCzyx_PhSj8ld23Aogdy5liLYKFBrvqEXfKviwMqavMYN_VRH60A43eqSoFegvLQBkoH1m5eG_xK4Y3wqAJk4stG_k9S1-DqISaVGZ5zMUM](https://lh4.googleusercontent.com/BIAmXNfN8b8LpuQd1lP6pWuCW_bGx3hee0wuBoFbyBM5aCzyx_PhSj8ld23Aogdy5liLYKFBrvqEXfKviwMqavMYN_VRH60A43eqSoFegvLQBkoH1m5eG_xK4Y3wqAJk4stG_k9S1-DqISaVGZ5zMUM)

![https://lh6.googleusercontent.com/DelzlR0n6fIwOy_gT6Imf8V4QIZyC2Z_vF2ACiZUUKhwiD5y5M5sy3mcTCR_PZONNPw6zil29r7VnsYfr8U_u-2TRvClUbOqiadakm0XshDqUdhJdO-uAUx9hhKVUULRjbFs4bpJdvXK-qRcmeaaOwg](https://lh6.googleusercontent.com/DelzlR0n6fIwOy_gT6Imf8V4QIZyC2Z_vF2ACiZUUKhwiD5y5M5sy3mcTCR_PZONNPw6zil29r7VnsYfr8U_u-2TRvClUbOqiadakm0XshDqUdhJdO-uAUx9hhKVUULRjbFs4bpJdvXK-qRcmeaaOwg)

### 2.3.2 Reading an item

python3 MoviesItemOps02.py

![https://lh4.googleusercontent.com/M4K9109_ffShpGnWs9b7eOloYijQ0LE5Ob8O2jJ6rDKxsYKWhnbBF4wOTdeVFK1muVKAofyD5ueIyfV_hRQqlsgI6wOgbmRwkEBhomYnPqxfZu3mEediKkNmpHL4JgjJxsuphQEQfmFjJw-E-OhR1TA](https://lh4.googleusercontent.com/M4K9109_ffShpGnWs9b7eOloYijQ0LE5Ob8O2jJ6rDKxsYKWhnbBF4wOTdeVFK1muVKAofyD5ueIyfV_hRQqlsgI6wOgbmRwkEBhomYnPqxfZu3mEediKkNmpHL4JgjJxsuphQEQfmFjJw-E-OhR1TA)

![https://lh5.googleusercontent.com/R_-An7mxr8tWE_oBE49RM7HSKGxwGeR99levEp6Eknn8fpzEy7lspokGE3wBUQ-uCxMcXtCA8-tWS1yPjpmC4MGh2GxHgTLU7KFagMqAvxUjnRXPBuz_5y0lshk7TazHuKLLvzgqcXrv_JGe0TzjbJs](https://lh5.googleusercontent.com/R_-An7mxr8tWE_oBE49RM7HSKGxwGeR99levEp6Eknn8fpzEy7lspokGE3wBUQ-uCxMcXtCA8-tWS1yPjpmC4MGh2GxHgTLU7KFagMqAvxUjnRXPBuz_5y0lshk7TazHuKLLvzgqcXrv_JGe0TzjbJs)

### 2.3.3 Update an item

python3 MoviesItemOps03.py

![https://lh4.googleusercontent.com/nJ0LGE7KQx986fnMsCt3_jX9jTqC2MKhgsNM9HL-wal3QMK4_ntNTgSsRTVyhSm0Z6AR9KYaLyqRKQ_36KYRPG5vd_OuX51SVJIOMekjUr9smlB2z5WhEUfdY8WnDCPUqLtTRhV6o0pSaRZeu2U-p-4](https://lh4.googleusercontent.com/nJ0LGE7KQx986fnMsCt3_jX9jTqC2MKhgsNM9HL-wal3QMK4_ntNTgSsRTVyhSm0Z6AR9KYaLyqRKQ_36KYRPG5vd_OuX51SVJIOMekjUr9smlB2z5WhEUfdY8WnDCPUqLtTRhV6o0pSaRZeu2U-p-4)

![https://lh6.googleusercontent.com/VfNgjugweXMQCDHsaUhoqHHAwG1RwTMRziRdcGlNSQK2pNMMwGwrkZTDiJuxS7PEdazOt3dMufWzDb52nz6Y1xTYGbsIFdvY0DPZ00Gz-cNNnx4oD_XuO7v2QKm3ig0BFEV5Dhj3SxQQjXu4_LMJAP0](https://lh6.googleusercontent.com/VfNgjugweXMQCDHsaUhoqHHAwG1RwTMRziRdcGlNSQK2pNMMwGwrkZTDiJuxS7PEdazOt3dMufWzDb52nz6Y1xTYGbsIFdvY0DPZ00Gz-cNNnx4oD_XuO7v2QKm3ig0BFEV5Dhj3SxQQjXu4_LMJAP0)

### 2.3.4 Delete an item

python3 MoviesItemOps04.py

![https://lh5.googleusercontent.com/ygySm235PoNTpOSKoOQ9vqzQnltl3QPSFTCVLy6zHxqOKO7TXp_vcbGHwqrGaXudxS3Xr8tr5wTAvsSivW5bNc1ADZwHt6YG10lS3wGt60z5ybiQiJZFhHJek1qTOuoI1jk1R8YkXDpw8IbUXFxP6tA](https://lh5.googleusercontent.com/ygySm235PoNTpOSKoOQ9vqzQnltl3QPSFTCVLy6zHxqOKO7TXp_vcbGHwqrGaXudxS3Xr8tr5wTAvsSivW5bNc1ADZwHt6YG10lS3wGt60z5ybiQiJZFhHJek1qTOuoI1jk1R8YkXDpw8IbUXFxP6tA)

![https://lh5.googleusercontent.com/_KOdRSmvG8X75q9uX4-sasq0IEitTeTglhJTEE5YtCH4vDnXYeg4e6yn77deVfaJb5ey5w6DeLaJSY4tcS53-W72xe2ttcEL3tF3SXimWe1x2gG0xHll5-5l874yzGFiW5lqCSjVhPnUSJL24ltBALY](https://lh5.googleusercontent.com/_KOdRSmvG8X75q9uX4-sasq0IEitTeTglhJTEE5YtCH4vDnXYeg4e6yn77deVfaJb5ey5w6DeLaJSY4tcS53-W72xe2ttcEL3tF3SXimWe1x2gG0xHll5-5l874yzGFiW5lqCSjVhPnUSJL24ltBALY)

## 2.5 Query the data

python3 MoviesQuery01.py

![https://lh4.googleusercontent.com/fCA9bYYoowShNn8qPVwh_t1kqf5-GU6zkKrCi_-cT8mfReRV7j4CrnSChKM5wMGHuLB6CXGrHFoKCrGo5OLW-696ZGQuiIiLeiUqyOsbgw8ttyvliOdGnUdl95mtfMYeVBw6e_XsflHB5gCnQqDFIMk](https://lh4.googleusercontent.com/fCA9bYYoowShNn8qPVwh_t1kqf5-GU6zkKrCi_-cT8mfReRV7j4CrnSChKM5wMGHuLB6CXGrHFoKCrGo5OLW-696ZGQuiIiLeiUqyOsbgw8ttyvliOdGnUdl95mtfMYeVBw6e_XsflHB5gCnQqDFIMk)

![https://lh6.googleusercontent.com/Nu_f4vq5k1khkGFW0IN-7M1CFdgtvYCi29Emtu3U3KsSZi38D-Wiiq-d6kF-qhecAbZP1YuPs37wi3TRM-M8dBxEfVnoru46nHHVXxCWgB-XLTu7LLxiBgTWhZiQj-vkTYdKkj-7uQM6L9VJDg_qIKQ](https://lh6.googleusercontent.com/Nu_f4vq5k1khkGFW0IN-7M1CFdgtvYCi29Emtu3U3KsSZi38D-Wiiq-d6kF-qhecAbZP1YuPs37wi3TRM-M8dBxEfVnoru46nHHVXxCWgB-XLTu7LLxiBgTWhZiQj-vkTYdKkj-7uQM6L9VJDg_qIKQ)

…

python3 MoviesQuery02.py

![https://lh3.googleusercontent.com/uATcAZLhKz-3sJFNa45boiGnSSRPH4DzacrugqqJgRwwRPI5-5iWdcnzZxrRoozxXAC-waHMf2LqGjAnnTwLWoyTfuC29BVzQbDoPiKa3WwxDQGfTLc4xC0FJ7mMPSlp_0K81hl-krAKmZLgB9LhXUQ](https://lh3.googleusercontent.com/uATcAZLhKz-3sJFNa45boiGnSSRPH4DzacrugqqJgRwwRPI5-5iWdcnzZxrRoozxXAC-waHMf2LqGjAnnTwLWoyTfuC29BVzQbDoPiKa3WwxDQGfTLc4xC0FJ7mMPSlp_0K81hl-krAKmZLgB9LhXUQ)

![https://lh6.googleusercontent.com/Poh1G2xSbEkS3HyaYAfy_1ry8kwQjgiubBaXO5uYGJfAAPTZ4sM3tblONIl4W0P9Jkz1_4tqGFHhSAiLN0gShRzolTkku8fwHUb_18K7acJErcxcC5DueXNMKiTnPaH3gGMHOMNdeqTsZMfFRCO3QCA](https://lh6.googleusercontent.com/Poh1G2xSbEkS3HyaYAfy_1ry8kwQjgiubBaXO5uYGJfAAPTZ4sM3tblONIl4W0P9Jkz1_4tqGFHhSAiLN0gShRzolTkku8fwHUb_18K7acJErcxcC5DueXNMKiTnPaH3gGMHOMNdeqTsZMfFRCO3QCA)

…

## 2.6 Scan the data

python3 MoviesScan.py

![https://lh6.googleusercontent.com/JQTZpll_yoN17hDgwx0yv3oUysJe0wpMeJzo5j9A40EqIvYLIMs8cxeTpp7uy73QGQdz5GCx2BUf16oGOF3gUKyjc6R_YzWxS7LsKAc4JpTV7yzEKtP2QWzSMOUjIg-WxRxYt6C2Pf0l_qw1QRY_vqU](https://lh6.googleusercontent.com/JQTZpll_yoN17hDgwx0yv3oUysJe0wpMeJzo5j9A40EqIvYLIMs8cxeTpp7uy73QGQdz5GCx2BUf16oGOF3gUKyjc6R_YzWxS7LsKAc4JpTV7yzEKtP2QWzSMOUjIg-WxRxYt6C2Pf0l_qw1QRY_vqU)

![https://lh6.googleusercontent.com/TzGVgQdSGpZqGeX28j441sehi_NJ7rGAqNuH1yUWe5FLjbMJNiLiuEfCwdypSjcbldKSrzho9pe1jMwr5QyROF9kG_K-2r80lVibOJk01-GvkYh51UEEVFDxQnDRTbxSVkqh5RddxA3M2r9o7nMOHDs](https://lh6.googleusercontent.com/TzGVgQdSGpZqGeX28j441sehi_NJ7rGAqNuH1yUWe5FLjbMJNiLiuEfCwdypSjcbldKSrzho9pe1jMwr5QyROF9kG_K-2r80lVibOJk01-GvkYh51UEEVFDxQnDRTbxSVkqh5RddxA3M2r9o7nMOHDs)

…

## 2.7 Delete the table

python3 MoviesDeleteTable.py

![https://lh5.googleusercontent.com/g3pmDAPkuMuG9eaTOHootk37utSfXXhBhPDkNJWWHuXZ_dCzIRucePiirxlr-dnT7qYFv_P449PJY6th5lu85d1Axo19GkfUx5xRQNlHMfoWcAPDLu3ztA2bCnBvzzu_SaOauHEDFljvBPZ2kmiP9EE](https://lh5.googleusercontent.com/g3pmDAPkuMuG9eaTOHootk37utSfXXhBhPDkNJWWHuXZ_dCzIRucePiirxlr-dnT7qYFv_P449PJY6th5lu85d1Axo19GkfUx5xRQNlHMfoWcAPDLu3ztA2bCnBvzzu_SaOauHEDFljvBPZ2kmiP9EE)

![https://lh3.googleusercontent.com/oGN0s4hs_tfMPMjHfMTdSEWrAGob20EphXPHLz9i-MkLO-m5YyEckk7JiuD8S_oOkHe6DnRwLKMFQ9N925h_xue7OCB7bGn7nXwCdcdjTGt2REw6YYCCi7OEGqIBsFS0kDXBNbHcvJ8CZsmL-9jzufQ](https://lh3.googleusercontent.com/oGN0s4hs_tfMPMjHfMTdSEWrAGob20EphXPHLz9i-MkLO-m5YyEckk7JiuD8S_oOkHe6DnRwLKMFQ9N925h_xue7OCB7bGn7nXwCdcdjTGt2REw6YYCCi7OEGqIBsFS0kDXBNbHcvJ8CZsmL-9jzufQ)

## 2.8 (Optional) Practice exercises

### 1 Print the titles of all movies released in 1994

![https://lh6.googleusercontent.com/5k4VtakgjjqsVvCvQeikqO2RHBd5kSpOnXcd7zQ_dMWApkvkYaYhz1SsZ1JnPmgBpGM9uf5bXFbBKnF6d06uRvee2AZtmUem7d89UNQZwjIg_qOpFXQEq_AiZWWXCG3k7GGnjXdgOPWCILnjnG6240g](https://lh6.googleusercontent.com/5k4VtakgjjqsVvCvQeikqO2RHBd5kSpOnXcd7zQ_dMWApkvkYaYhz1SsZ1JnPmgBpGM9uf5bXFbBKnF6d06uRvee2AZtmUem7d89UNQZwjIg_qOpFXQEq_AiZWWXCG3k7GGnjXdgOPWCILnjnG6240g)

![https://lh3.googleusercontent.com/wQ5T1nGQD-URMSEe6KQFeMuDklxCMSOMS6ee2TZiBjF6y0BOuXbyRU-pGY9u2e_NI7uvvbGJ2WSAERZ25CB70n-xEnlrtZc3i0pcfgINTeHITNkIr5HxwpN4DtNlJgbJnIim0NJmX_zVvlNxmoRPFsk](https://lh3.googleusercontent.com/wQ5T1nGQD-URMSEe6KQFeMuDklxCMSOMS6ee2TZiBjF6y0BOuXbyRU-pGY9u2e_NI7uvvbGJ2WSAERZ25CB70n-xEnlrtZc3i0pcfgINTeHITNkIr5HxwpN4DtNlJgbJnIim0NJmX_zVvlNxmoRPFsk)

…

### 2 Print complete information on the movie ‘After Hours’ released in 1985

![https://lh3.googleusercontent.com/9wAGWOxOU3IUJQT9rFcHhYk0olFe8MnF0iW6KY4bHk7VuhoZE3rhdjhwtU5U3p7Wy5EyLFAHmZ17whaYMjxMQ2Z2XIqtoLH9zgmufwfLJxVovsHOJHD5qgQp_oYEv9ILTxnV7V9y6HjfB3I7BvDUu04](https://lh3.googleusercontent.com/9wAGWOxOU3IUJQT9rFcHhYk0olFe8MnF0iW6KY4bHk7VuhoZE3rhdjhwtU5U3p7Wy5EyLFAHmZ17whaYMjxMQ2Z2XIqtoLH9zgmufwfLJxVovsHOJHD5qgQp_oYEv9ILTxnV7V9y6HjfB3I7BvDUu04)

![https://lh3.googleusercontent.com/jG6juMMUuyc5TAxIBKGdDPAY-Fw5xvgGUsFgRp_JV8FN2QD4su49McIXfQfn6Q50UUmYgZZVLu75ab-2S05q1XtVm8401krT15oEK5gqJpnALup8YNt0Nmfv8WVpqcUM0kYNsuTNpiiWHDv75jVCfaE](https://lh3.googleusercontent.com/jG6juMMUuyc5TAxIBKGdDPAY-Fw5xvgGUsFgRp_JV8FN2QD4su49McIXfQfn6Q50UUmYgZZVLu75ab-2S05q1XtVm8401krT15oEK5gqJpnALup8YNt0Nmfv8WVpqcUM0kYNsuTNpiiWHDv75jVCfaE)

### 3 Print all movies released before 2000

![https://lh6.googleusercontent.com/XBpxJqVMpgFw2GFBYW8FzNupolC3f9ryhECuCUeMi3A-2Ee7ouD53S7OtHC9fdsOH2o4RXS6KUrdfZiA7yuwzQc8PsLlKxbdVx04EMmPV8YrjihU1x4N55HCxhCbvNQBTgI-Yqog5wK-BeBA2LnRM3w](https://lh6.googleusercontent.com/XBpxJqVMpgFw2GFBYW8FzNupolC3f9ryhECuCUeMi3A-2Ee7ouD53S7OtHC9fdsOH2o4RXS6KUrdfZiA7yuwzQc8PsLlKxbdVx04EMmPV8YrjihU1x4N55HCxhCbvNQBTgI-Yqog5wK-BeBA2LnRM3w)

![https://lh3.googleusercontent.com/ecJ_5E8iMx0TphFrC8PfxDZdpaeBstqHeBXmxj7GNbeO1TW2fdXQyyW5fR9ocQf1Lfd9gz4XdcFX6vWGS2YhNp1T38oPG6YzndHzLeU6LzxdULhvAVgxZyPBoxLL80apSByy4pENrdzHDqxiLZ5fmJY](https://lh3.googleusercontent.com/ecJ_5E8iMx0TphFrC8PfxDZdpaeBstqHeBXmxj7GNbeO1TW2fdXQyyW5fR9ocQf1Lfd9gz4XdcFX6vWGS2YhNp1T38oPG6YzndHzLeU6LzxdULhvAVgxZyPBoxLL80apSByy4pENrdzHDqxiLZ5fmJY)

…

### 4 Print only the years and titles of movies starring Tom Hanks

![https://lh4.googleusercontent.com/1p1Me_Y7yi0RewJKJtPXsM57Bm_qu6PvmFeomyWr7aqMnjKfE9AU7B_m_HmxoQ9SpK7QM14nIOv_j_XqXHchebDUw_otn65Es9oG6WP1-nPVe4YN01FQVXOfGHGApjKigIkUMQ3CKAFmvoj2CLAPi2I](https://lh4.googleusercontent.com/1p1Me_Y7yi0RewJKJtPXsM57Bm_qu6PvmFeomyWr7aqMnjKfE9AU7B_m_HmxoQ9SpK7QM14nIOv_j_XqXHchebDUw_otn65Es9oG6WP1-nPVe4YN01FQVXOfGHGApjKigIkUMQ3CKAFmvoj2CLAPi2I)

![https://lh4.googleusercontent.com/sRD7oOl2FIQY1-EoZQLlacmLl9zCUbYP2HdudXYSutYhMLDiLy9j_kKvxnT-jaWi7Xa3Hxj_5f7HyNrw7-dIS6JGFJLgCQvqXE1xQSshet6SiJvUi-4OFehIGSoZYY-JUp49wqWFjcAW3Sgq81sR4-E](https://lh4.googleusercontent.com/sRD7oOl2FIQY1-EoZQLlacmLl9zCUbYP2HdudXYSutYhMLDiLy9j_kKvxnT-jaWi7Xa3Hxj_5f7HyNrw7-dIS6JGFJLgCQvqXE1xQSshet6SiJvUi-4OFehIGSoZYY-JUp49wqWFjcAW3Sgq81sR4-E)

…

### 5 Remove all movies released before 2000

```python
from pprint import pprint
from decimal import Decimal
import boto3
from botocore.exceptions import ClientError
from boto3.dynamodb.conditions import Key

def scan_movies(year_range, dynamodb=None):
    if not dynamodb:
        dynamodb = boto3.resource('dynamodb', region_name='us-east-1')

    table = dynamodb.Table('Movies')

    response = table.scan(FilterExpression=Key('year').between(year_range[0], year_range[1]));
    data = response['Items']

    while 'LastEvaluatedKey' in response:
        response = table.scan(FilterExpression=Key('year').between(year_range[0], year_range[1]), ExclusiveStartKey=response['LastEvaluatedKey'])
        data.extend(response['Items'])

    return data

def delete_underrated_movie(title, year, dynamodb=None):
    if not dynamodb:
        dynamodb = boto3.resource('dynamodb', region_name='us-east-1')

    table = dynamodb.Table('Movies')

    try:
        response = table.delete_item(
            Key={
                'year': year,
                'title': title
            }
        )
    except ClientError as e:
        if e.response['Error']['Code'] == "ConditionalCheckFailedException":
            print(e.response['Error']['Message'])
        else:
            raise
    else:
        return response      
    

if __name__ == '__main__': 
    query_range = (0, 2000)
    print("Finding movies released before 2000...")
    movies = scan_movies(query_range)

    for movie in movies:
        print("Attempting a conditional delete...")
        delete_response = delete_underrated_movie(movie['title'], movie['year'])
        if delete_response:
            print("Delete movie succeeded:")
            pprint(delete_response)
```

![https://lh6.googleusercontent.com/qTDQOTxemXnybCBn3ocSNePZtx9lltBqQbYU7fD0ZT6nfSVtoCjxp82QfmhmQwlNn25vcxJzCp_2ZkYanwuSeHW2xo5NYUsjJddMyFFhHCN0cQOsH3KaFlBb0vRcGj49JDjK9q4WGG562G56gHSZE6g](https://lh6.googleusercontent.com/qTDQOTxemXnybCBn3ocSNePZtx9lltBqQbYU7fD0ZT6nfSVtoCjxp82QfmhmQwlNn25vcxJzCp_2ZkYanwuSeHW2xo5NYUsjJddMyFFhHCN0cQOsH3KaFlBb0vRcGj49JDjK9q4WGG562G56gHSZE6g)

run task3 again, finds nothing this time.