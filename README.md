This project consists of 2 main parts; the first one will be configuring low level UART driver, and the second one will be implementation of the application layer of the microcontroller.

Configuring low-level UART Driver:

Let's build UART driver that consists of many files such as "uart.c" and "uart.h". However, we will focus on the functions exist in "uart.c" in order to be able to configure the UART module.

As we learned through the lesson, configuring UART registers is about setting some bits to one or clearing some bits to zero, so this is general formula on how to set or clear a bit in any register:

Setting a bit in any register:

To set a bit in any register in embedded systems programming (such as working with Atmega16), you can use the bitwise OR operator (|) along with the bit shift operator (<<).

Syntax:

register |= (1 << bit_position);

Explanation:

register: The name of the register where you want to set the bit.
1: A binary value with a single bit set to 1.
<< bit_position: This shifts the 1 by bit_position places to the left, creating a mask with the bit you want to set.
|=: The bitwise OR operation ensures that the specified bit is set to 1, while leaving the other bits unchanged.
Example:

Let's say you want to set bit 3 in the PORTB register

PORTB |= (1 << 3);

This operation only affects bit 3 of the PORTB register, and all other bits remain unchanged.

Clearing a bit in any register:

To clear a bit in any register, you can use the bitwise AND operator (&) along with the bitwise NOT operator (~) and the bit shift operator (<<). This will set a specific bit to 0 while leaving all other bits unchanged.

Syntax:

register &= ~(1 << bit_position);

Explanation:

register: The name of the register where you want to clear the bit.
1: A binary value with a single bit set to 1.
<< bit_position: This shifts the 1 by bit_position places to the left, creating a mask with the bit you want to clear.
~: The bitwise NOT operator inverts all bits in the mask (so the bit you want to clear becomes 0 and all others become 1).
&=: The bitwise AND operation clears the specified bit (sets it to 0) while leaving the other bits unchanged.
Example:

Let's say you want to clear bit 3 in the PORTB register:

PORTB &= ~(1 << 3);

This operation only affects bit 3 of the PORTB register, leaving all other bits unchanged.

Data Direction Registers (DDR)

In microcontrollers like the Atmega16, each I/O port has a Data Direction Register (DDR) that controls whether each pin in the port is configured as an input or output. There are several data direction registers for different ports, like DDRA, DDRB, DDRC, and DDRD.

DDR Register controls the direction of data flow:
Setting a bit in the DDR register to 1 configures the corresponding pin as an output.
Setting a bit to 0 configures the corresponding pin as an input.
For example:

DDRB controls the direction for the pins in Port B.
DDRC controls the direction for the pins in Port C.
How to Set a Pin as Output or Input:

Setting a Pin as Output:

To set a pin as an output, you set the corresponding bit in the DDR register to 1.

Syntax:

DDRx |= (1 << pin_number); // Set pin as output

Example:

To configure pin 2 of Port B (PB2) as an output:

DDRB |= (1 << 2); // Set PB2 as output

This sets bit 2 of DDRB to 1, making PB2 an output pin.

Setting a Pin as Input:

To set a pin as an input, you clear the corresponding bit in the DDR register to 0.

Syntax:

DDRx &= ~(1 << pin_number); // Set pin as input

Example:

To configure pin 3 of Port B (PB3) as an input:

DDRB &= ~(1 << 3); // Set PB3 as input

This clears bit 3 of DDRB, making PB3 an input pin.

Setting Multiple Pins as Input or Output:

You can configure multiple pins at once by combining the bitwise OR (|) and bitwise AND (&) operations for different pin numbers.

Example 1 (Multiple Outputs):

To set PB1 and PB2 as output pins:

DDRB |= (1 << PB1) | (1 << PB2); // Set PB1 and PB2 as outputs

Example 2 (Multiple Inputs):

To set PC0 and PC1 as input pins:

DDRC &= ~(1 << PC0) & ~(1 << PC1); // Set PC0 and PC1 as inputs

Pull-Up Resistors for Input Pins:

When configuring a pin as input, you might want to enable the internal pull-up resistor to avoid a floating input. This is done by writing a 1 to the corresponding PORT register.

Example:

To set PB3 as an input and enable the internal pull-up resistor:

DDRB &= ~(1 << 3); // Set PB3 as input

PORTB |= (1 << 3); // Enable pull-up resistor on PB3

Summary:

Use DDRx |= (1 << pin_number) to set a pin as output.
Use DDRx &= ~(1 << pin_number) to set a pin as input.
Enable pull-up resistors for input pins by setting the corresponding bit in the PORTx register.
Task 1:

You will need to implement UART_init function at which the following requirements should be satisfied:

Configure UART to be working with double transmission speed mode.
Enable the transmitter and receiver of UART.
Check that you are working with 8-bit data mode.
Adjust the Baud Rate to be 9600 using #define in "uart.h"
Use this macro to set the baud rate value in its register in a right way #define BAUD_PRESCALE (((F_CPU / (USART_BAUDRATE * 8UL))) - 1)
You will find both microcontrollers are configured with internal 1Mhz Clock in "micro_config.h".
Task 2:

You will need to implement UART_sendByte function at which the following requirements should be satisfied:

it will takes the data of type uint8 to be sent as argument and save it to UDR to be sent by UART.
from your understanding to UART registers, check the needed flag to implement the function, and you can use the macros in common_macros.h file to match your need.
Task 3:

You will need to implement UART_recieveByte function at which the following requirements should be satisfied:

it will take nothing as argument "void" and will return data of type uint8.
from your understanding to UART registers, check the needed flag to implement the function, and you can use the macros in common_macros.h file to match your need.
Implementation of Application layer

The project consists of 1 Microcontroller which is the Transmitter and PC which is the Receiver.

Task 4:

Write Embedded C code using ATmega16 Microcontroller to use UART Driver. the requirements are as follows:

Configure the microcontroller with internal 1Mhz Clock.
Receive a byte from UART which was sent by PC terminal then send it back to PC as "Echo".
As a way of validating that there are no errors, you can check that the code compiles from the terminal using the following commands:

For Main.c: avr-gcc -mmcu=atmega16 -DF_CPU=1000000UL main.c uart.c -o a.out
