# Lab 5: Build a sequential circuit

## Overview and Motivation
In this lab, we will design circuits that remember the previous output in order to achieve increased functionality. These are called sequential circuits. Before this lab, we created different combinational circuits in which the outputs of the circuits were determined by the inputs directly. This means we weren't required to worry about memory, nor any notion of the sequence of operations that may have happened in previous moments before the current state.

This week, we will be working with Sequential Circuits. Sequential circuits introduce the idea of different states and being able to remember and use the outputs of the previous moment as one of the inputs for the current state. The outputs can now be a function of both, inputs and previous internal state of the sequential circuit. 

The challenge for this lab is constructing a circuit that reads a binary number bit by bit and determines if that input (entire number) is divisible by 3. Each bit of the binary number will be clocked into the circuit sequentially, from the highest order bit to the lowest (left to right). As we recieve each bit, we must accumulate it into the binary array we have up until that moment and update our circuit's state accordingly.

This lab requires you to be quite familiar with Finite State Machine Design. To successfully be able to construct this circuit, we'll need to complete multiple tasks. We will discover those in this lab so let's jump in! 

## Materials
For this lab, we will need to consult IC data sheets for the NOT, AND, XOR, and the JK Flip-flop:
- IC data sheets

- PB-503 breadboard prototyping station with the Logic Probes, NC Push Button and Logic Switches 

- Wires and connection tools

- Logic Probes (if not included in the breadboard)

- Logic Switches (if not included in the breadboard)

- Push Button (if not included in the breadboard)

- 7404 NOT gate IC

- 7408 AND gate IC

- 7486 XOR gate IC

- 7476 JK Flip Flop IC

- 1 Resistor

Once getting all things prepared, we can get to work!

## Project Steps
In order to design a sequential circuits, we have to go through main 7 steps:

* **Step 1:** Design a finite state machine (FSA)
* **Step 2:** Construct a state table
* **Step 3:** Use the FSM and state table to build a function table
* **Step 4:** Read values from the function table into K-maps
* **Step 5:** Use the K-maps to build for the next state and output blocks.
* **Step 6:** Build the circuit in logisim and fully test the design. You want to uncover any errors at this point before
you begin physical construction.
* **Step 7:** Build the circuit on the breadboard.

### 1. Design a Finite State Machine (FSA) 
- For this task, we will draw a DFA for the task. We will draw a DFA that will accept the binary numbers that are divisible by 3. Notice that the 0 number will be accepted, we will have the first state as our accepting state. We know that number that is divisible by 3 is when the sum of the all digits in the number is divisible by 3. Writing out some examples of binary numbers that are/aren't divisible by 3, we can see that if the number has more than 1's or there are an even number of 0's between the 1's, the number is accepted. 

- We have the transition table as follow:
[Transition table](https://drive.google.com/file/d/13HbGzndM91nAsxtJWEFQTZ1iAskhfdT-/view)

From the transition table, we can easily construct the DFA. 

[DFA](https://drive.google.com/file/d/1S50cKXSJQLSqXuNXfSK790zJrPmv61I9/view)

#### Construct a state table
Now, let's set our state stable as follow (the output at each state table can be arbitrarily chosen):
[State table](https://drive.google.com/file/d/1sZio_7WSW4t37cYnj2W7NabdL0Hb_9US/view)

Now, based on the DFA we just construct, sketch out the truth table for our circuit. However, before drawing the truth table we should notice the the basic idea of how the JK flip-flop works according to our inputs and expected outputs. 

[J-K input table](https://drive.google.com/file/d/1h7cKYnVSzQUT1-621IHJomugLkQs4_Rf/view)

#### 2. Use the FSM and state table to build a function table
Now, we are ready to draw our truth table.

[Truth table](https://drive.google.com/file/d/1MBYbH9pDKPhsy4Z7Dk1nSxiK1QTjULVc/view)

#### Read values from the function table into K-maps
Once we have the function table, we can go ahead to draw K-map. We will use the K-map to reduce our Bolean expression

[K-maps for J1, K1, J0, K0, and overall output F](https://drive.google.com/file/d/1xl4nwATRYq2E6sDXlFrhGH2li2mazk-b/view)

After sketching out K-maps and reducing boolean expressions, we have: 
$$J_1 = Q_0 \cdot \bar{x}$$

$$K_1 = \bar{x}$$

$$K_0=1$$

$$J_0 = Q_1 \oplus x$$

$$F = \bar{Q_0} \cdot \bar{Q_1}$$

#### Construct circuits:
Before jumping right into buiding a real circuit, we should always test the logic of our circuits using Logisim with a few inputs. In our case, we used 0, 01, 010, 0101, 01011. Only input 0 makes `F` output turns `HIGH` or give 1. The other inputs should output 0 at `F`. 

[Logisim circuit](https://drive.google.com/file/d/1n73eU4hQQs94zPmSFmsSJpLabodTOr8N/view)

#### Construct real circuits:
In this section, we will get to construct the real sequential circuit. The wiring steps for this circuit is quite complicated. As stated above, we will need a 7476 JK flip-flop, a 7404 NOT chip, a 7408 AND chip, and a 7486 XOR chip. 

##### Wire input and Clock:

Following the Logisim, we will have Clock and an input point. We will use the `NC PB` push button as our Clock. To wire the push button, 

- We wire a resistor with the `HIGH` hole on the breadboard. Then, wire the other end of the resistor to the `NC PB`. Use a wire and connect the hole on the `NC PB` with a row on the breadboard. 

- For the input, we use Switch 1 `S1` on the breaboard. 

- We will wire the input in the order of the JK flip-flop K's and J's. Since each 7476 JK flip-flop chip has 2 JK flip-flops, we will use, in this lab, the **JK gate 1** (at the top of the 7476 JK chip) for the **JK flip-flop 1** in our Logisim circuit. **JK gate 2** will be used as **JK gate 0** in our Logisim circuit. 

#### Wire the 7476 JK flip-flop chip:
First, we will need to wire the basics for the JK flip-flop:
- Wire the `GND` of the chip to the `GND` on the breadboard and `Vcc` to the `HIGH` hole. 

- Now, each JK flip-flop has to be connected to the Clock. Wire `1CLK` and `2CLK` to the row on the breadboard that is connected with the `NC PB` we just wired in the previous step. 

- We also need to wire `~CLR`. We use 1 Switch for `~CLR` pins of the 2 JK flip-flops. We use another Switch (we used `S8`) and connect the switch to `1~CLR` and `2~CLR` of the 2 JK flip-flops. 

##### Wire K_0:
Based on our reduced SOP, $K_0 = 1$. This is simple! We just need to take a wire and connect the `K_0` to a `HIGH` hole on the breadboard. 

##### Wire J0:
We have `J0 = Q1 XOR x`. Hence, take a 7486 XOR chip and follow the instructions:

- Wire the input `x` (from the Switch 1) to an `XOR` gate on the 7486 (XOR) chip. 
- Use another wire to connect the output `Q_1` from the gate 1 (`1Q` pin on the chip) on the 7478 JK chip to the other input pin on the `XOR` chip. 
- Wire the output of the `XOR` chip to `J0` (`2J` on the chip).

##### Wire K1:
From our SOP, `K1 = ~x`. Hence,

- From the Switch 1 `S1`, wire the input to 1 input pin on the NOT chip. 
- Take the output of the NOT gate and connect to the `K1` on the first JK flip-flop of the 7476 JK chip (`1K` pin on the chip). 

#### Wire J1:
We have `J1 = Q0 (~x)`. To wire the circuit:

- Take the 7408 AND chip. Wire the chip `Vcc` and `GND` with `HIGH` and `LOW` holes on the breadboard. 
- From the output of the **second JK flip flop** output, connect the output pin to the first input pin on the 7408 AND chip. 
- On the row connected with the `~(x)` we just created at **Wire K1** step, plug a wire to connect the output `~x` to the second input pin of the 7408 AND chip. Now, we're done with wiring the `J1`!
- Wire the output of the AND gate to `J1` (which is `1J` on the breadboard).

##### Wire output F:
Lastly, we will need to wire our overall output `F`. See that `F = (~Q0)(~Q1)`. To wire `F`, we follow:

- Each JK flip-flop gate has `Q` and `~Q` outputs. We will wire the `~Q0` (equivalent to `~1Q` pin on the chip) pin with one input pin of another AND gate on the 7408 AND chip. 
- Similarly, wire the `~Q1` (equivalent to `~1Q` on the real JK flip-flop) to the second input pin of the AND gate. 
- Connect the output of the AND gate to one Logic probe (we used `LED 8`) to check for the output. 

## Testing
Now that we hae completed wiring the circuit, let's see if our circuit works as expected. 

**Brief explanation of our circuit:**
- The JK flip-flops we use for this lab are **falling-edge JK flip-flop**. Hence, the JK flip-flops will be triggered on the falling-edge. The JK flip-flops are connected to a CLEAR switch. This is actually `~CLR` pin which means the JK flip-flops will be reset when we turn the `CLR` switch off.

- As we are building a circuit in which only binary numbers that are divisible by 3 are accepted. Thus, we expect the overall output `F` will light up `HIGH` whenever the current binary number is a multiple of 3. The input is an accumulated array of 0's (equivalent to `LOW` signal) and 1's (equivalent to `HIGH` signal). New signal (or input, `0` or `1`) will be added in the front of the array. 

- To closely watch the behavior of this circuit, we wire the outputs of each JK flip-flop, `Q0` (`2Q` pin in this lab) and `Q1` (`1Q` pin in this lab), to a LED lights to see if both works as expected. Though this step will help with debugging our circuit, if the circuit works properly, this step can be skipped. 

- For our testing, we will test multiple binary numbers to see if they correctly output the desired response. For example, input 01 which is 1 in decimal will show `LOW` because it is not divisible by 3. 011 which is 3 in decimal will show `HIGH` because 3 is divisible by 3.

- First, we put in 5 test cases: 0, 01, 010, 0101, 01011 (notice that the input will accumulated into the current binary number until we turn off `~CLR` switch). The circuit should light up `HIGH` for 0 and `LOW` for all the others. The circuit should ouput as the video below:

[Output for 0, 01, 010, 0101, 01011](https://drive.google.com/file/d/1iq485izF94V7J8ssvAgPeJz1bxwVLe0e/view)

- Then, we will input more accepted number, 011(=3) and 1111(=15). For both cases, the circuit should light up `HIGH`. 

[Testing 011](https://drive.google.com/file/d/1lhcldwx7P9Mw5BXU-ClRg6SsIs5eRt6D/view?t=3)

[Testing 1111](https://drive.google.com/file/d/1lwVgB8CQQOLy5zm9UnlnSkBp58gH6f61/view?t=2)

If your circuit works as the one in the video above, you have successfully completed this lab!

## Conclusion

This lab is where we learned the concept of memory and different states. Systematically, we learned to utilize Finite State Machine design which is very essential to form the backbone of sequential circuit implementation. We also got to apply the theoritical knowledge we learned in class about the sequential circuits like J-K Flip Flops e.t.c. and combining them with the combinational circuits in a practical scenerio. The project itself, building a circuit that reads a binary number that is inputted in a stream and determines if the input in decimal is divisible by 3, is very important and essential in a real world scenario. Overall, we learn the procedures of designing and constructing a sequential circuit that behaves accordingly to our expectations. 

By going through the whole process of completing this lab, we not only honed our theory and technical skills but also learned how to debug/troubleshoot our problems by backtracing, and solving problem step by step. This is an opportunity to practice circuit debugging and circuit testing. We enhance our understanding of sequential circuits by spending our time with this lab as a group.



