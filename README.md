Download Link: https://assignmentchef.com/product/solved-csc-230-lab-3-introduction-to-assembly-language
<br>
Launch Atmel Studio<em> 7.0</em> and create a new project named “lab3”: click on the<em> Start</em>  button, then click on <em>Atmel Studio 7.0: </em>Create a new project named <em>lab3</em>: on the menu, click on File -&gt; New -&gt; Project:

In the new dialog box, on the left pane, under <em>Installed</em>, select <em>Assembler</em>. Type the project <u>Name</u> <em>lab3</em> and select a <u>Location</u> on drive “H:”. Click on the <em>OK</em> button.

When the new dialog box appears, choose <em>ATmega2560</em> for the <em>Device Family</em>. Click on the <em>OK </em>button.

The new project will look similar to this:

Remove:

start:            inc r16           rjmp start

And replace it with the following code:

Save the code and build the program: on the menu, click on <em>Build</em> -&gt; <em>Build Solution</em>:

The screen looks like this:

If there are any errors or warnings, you must address them and rebuild your program.

When there are no errors or warnings, you are ready to run your program using the simulator. Set up the configuration of the simulator: from the <em>Project</em> menu -&gt; <em>lab3 Properties…: </em>

In the settings window, click on Tool and select Simulator:

Save the project by click on “File” menu -&gt; “Save All”.

Start the simulator: from the menu, click on <em>Debug</em> -&gt; <em>Start Debugging and Break</em>

The editor should show a yellow arrow indicating the instruction about to be fetched. The left panel shows the “<em>Processor Status</em>”, where you can examine the register and processor values. The <em>“Program Counter”</em> shows the memory address of the next instruction to be fetched.

Execute the first instruction by clicking <em>Step Into</em> (or press the F11 key).

You can also do it by clicking this icon on the menu bar:

After executing the first instruction, the value in register 16 (R16) is changed from 0x00 to 0x01. Notice that whenever a value changes in the Processor Status, Memory, and I/O windows, they are highlighted in red. Also, the instruction to be executed NEXT is indicated by the yellow arrow on the side.

You can stop the debugging session anytime by clicking on the “Stop Debugging” under the “Debug” menu (or press Crtl+Shift+F5)

<ol>

 <li><strong>Converting Assembly language instructions to binary machine code.</strong></li>

</ol>

Let’s consider the LDI instruction, which can be found on page 94 in the <em>AVR Instruction Set Manual</em>.

The binary opcode (operation code) for the instruction “LDI Rd, K” is “<strong>1110</strong>” and together the opcode and the operands are arranged as “<strong>1110</strong> <strong>KKKK</strong> <strong>dddd</strong> <strong>KKKK</strong>”. Here, and everywhere else in the manual, Rd stands for destination register, and the corresponding dd…d specifies which register will be used as Rd. Similarly, KK…K specifies a value in binary format.

For example, to load the value 0x82 into register 16, we would write an Assembly language instruction:

LDI r16, 0x82

In this example:

<ul>

 <li>K = 0x82, which is 0b<strong>10000010</strong> in binary, which must be in range 0 ≤ K ≤ 255.</li>

 <li>d = 16, is 0b1<strong>0000</strong> in binary, which must be in range 16 ≤ d ≤ 31 (thus only 4 LSB<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> are used).</li>

</ul>

Note that the manual specifies the minimum and maximum values for the operands. Since there are only 4 bits available to specify the destination register, we can only specify 16 out of 32 registers. Thus, 0b0000 corresponds to r16 and 0b1111 corresponds to r31.

The Assembly instruction above translates to 0b<strong>1110</strong><strong>1000</strong><strong>0000</strong><strong>0010</strong> 16-bit binary machine code, which is 0x<strong>E</strong><strong>8</strong><strong>0</strong><strong>2</strong>. However, in program memory, you see 0x<strong>0</strong><strong>2</strong><strong>E</strong><strong>8</strong>. Why? Because it is stored in the Little-Endian format such that, in each word (two bytes), the least significant byte is stored.In summary, the Assembly instruction “LDI r16, 0x82” corresponds to the binary machine code 0b1110100000000010, or 0xE802, which is stored as 0x02E8 in the AVR program memory.

<ul>

 <li><strong>Exercises:</strong></li>

</ul>

<ol start="2">

 <li>Write the following Assembly language instructions (mnemonics) and convert them into the binary machine code using the Assembly Instruction Set manual. Verify the results by reviewing the machine code (hexadecimal numbers) in the program memory using the debugging procedure described above.

  <ol>

   <li>ANDI r16, 0x3A</li>

   <li>LDI r17, -5</li>

   <li>MOV r17, r0</li>

  </ol></li>

 <li>Write a program that calculates the number of students in the hypothetical course CSC 999. There are two lab sections in this course, B01 and B02. The number of students registered in these sections are 23 in B01 and 21 in B02.</li>

</ol>

Given that the maximum enrollment for the course is 60, set register 0 to indicate whether there are more students in the course than allowed by the enrollment limit. So, set R0 to 1 if there are more than 60 students enrolled in the labs and 0 otherwise. You can assume that the total number of students will fewer than 256. Here is the pseudo code for the algorithm:

<ul>

 <li>Initialize the output register (set it to 0): R0 &lt;= 0x00 o Set R19 to the value of maximum enrollment</li>

 <li>Set R16 to the value indicating the number of students enrolled in B01 o Set R17 to the value indicating the number of students enrolled in B02 o Add R16 and R17, store the result in R16 o If R16 &gt; R19, set R0 to 1</li>

 <li>Done</li>

</ul>

<ol start="3">

 <li><strong> </strong>Write a program that checks if an 8-bit unsigned number is an even number. If the input number is even, the program outputs 0x00, otherwise 0xFF. Let’s use R18 for input and R19 for output. To test the program, initially set the input to 0x09 and test it. Then change the input to 0x08 and manually check if the output is correct via the debugging procedure described in this lab. Here is the pseudo code for the algorithm:

  <ul>

   <li>Initialize the output register R19 (set it to 0): R19 &lt;= 0x00 o Set R18 to 9 as input to test: R18 &lt;= 0x09</li>

   <li>Set all bits except the last (least significant bit) of R18 to 0 (hint: use ANDI with a mask) o If R18 = 0x01, set R19 to 0xFF (note that you could use the zero flag in SREG directly after masking, thus reducing the required number of instructions. In other words, you don’t need to compare R18 with 0x01 separately after the masking operation.) o Done</li>

  </ul></li>

</ol>

<a href="#_ftnref1" name="_ftn1">[1]</a> LSB stands for Least Significant Bit.