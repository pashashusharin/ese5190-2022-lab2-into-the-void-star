University of Pennsylvania, ESE 5190: Intro to Embedded Systems, Lab 2A

    Pavel Shusharin
        
    Tested on: Lenovo Yoga 720, Windows 10


3.2 
•  Why is bit-banging impractical on your laptop, despite it having a much faster processor than the RP2040?
Using bit-banging in modern processors is impractical since it will interrupt the natural flow of operation of the processor that has a large amount of in-flight operations performing at once.Additionally the modern processor speeds are extremelly fast and correctly enterring interrupt handler is increasingly uncertain.    

• What are some cases where directly using the GPIO might be a better choice than using the PIO hardware? 
GPIO might be a better choice for simple operations like switches, LED interface. 

• How do you get data into a PIO state machine?
- Data is first recieved from Pins and Other input sources
- It is then passed through to a bi-directional shifter 
- Use IN command to shift instructions 1...32 bits at a time into an Input Shift Register
- Use PUSH commaond to write the ISR content into RX FIFO which is then transferred into the system 

• How do you get data out of a PIO state machine? 
- first the data comes into the State machine from the system TX FIFO 
- Use PULL command to remove a 32 bit word from TX FIFO and place it into Output Shift Register (OSR)
- Once the data is transferred into OSR use OUT command to shift data from OSR to other destinations, 1..32 bit at a time (SM will automatically refill OSR upon command execution)

• How do you program a PIO state machine?
PIO has a total of 9 instructions and are programmed through short binary programs written in textual PIO assembly format. The PIO programs are assembled using an included assemblaer callerd pioasm. 
 
• In the example, which low-level C SDK function is directly responsible for telling the PIO to set the LED to a new color? How is this function accessed from the main “application” code?

pio_sm_put_blocking() is able to push data from the processor directly into the state machine's TX FIFOs. 
This function is accessed in the application code through a function called put_pixel 

• What role does the pioasm “assembler” play in the example, and how does this interact with CMake?
pioasm converts PIO assembly textual code into a binary file which can be accessed by the processor. CMake can then call the assembled program directly


