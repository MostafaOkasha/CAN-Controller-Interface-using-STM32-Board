# CAN-Controller-Interface-using-STM32-Board
#### Note: TA station here is the Main Station we are trying to communicate with
Using the STM32 CAN and an external CAN transceiver to communicate with the MAIN Station to send and receive messages.  There are also other Master Nodes present to communicate with the Main Station. More details provided in the document. Implementation using C++ code on Uvision4

# Brief note about Controller Area Networks:
Controller Area Network or CAN is a single BUS communication type that allows devices (known as Nodes or Masters) to communicate with each other without the need of a host to send messages from one node to the other. Each Node sends the information to the BUS and every other Node will receive that message. It is up to each Node to know whether that message is meant for it or not by using the necessary filters to only accept the messages meant for it. Messages are sent using Non-Differential Wires to prevent noise interruption altering the messages. 


## 1.	What is the difference between a standard CAN and an extended CAN?

Standard and Extended CAN are the two different specifications for CAN 2.0. 
CAN 2.0A is for the standard CAN specification and CAN 2.0B is for the Extended CAN specification. The standard format has an 11 bit identifier and the extended format has a 29 bit identifier. So standard CAN can have up to 2048 identifiers where extended CAN up to more than half a billion identifiers. Each format is used depending on the need of the system it is implemented in. 

## 2.	If one of the stations in a CAN network uses extended CAN protocol and another uses the standard protocol and both start transmitting at the same time, which station will win?

If both networks start transmission at the same time then arbitration will take place to decide which signal prevails and thus, which station will win. The first node to transmit a recessive bit loses arbitration. However if both signals have the exact same identifier for the first 11 bits then 2.0 A (Standard specification) will win because the IDE is dominant and so the Station transmitting in Standard protocol will dominate. Also the SRR bit for the extended data frame is recessive as the RTR frame in the standard protocol is always dominant. This allows for the distinction between both frames and helps both protocols to run side by side on the same bus without any problems occurring even though both protocols are implemented differently.

## 3.	What is meant by the non-destructive arbitration property of CAN?

The non-destructive arbitration of CAN is basically using an arbitrator to settle the dispute between the signals by allowing for the highest priority message to remain on the BUS and thus giving the highest priority message a non-interrupted transmission. This arbitration method is non-destructive because the Node sending a message constantly checks the Bus to see if the signal it sent to the bus is different and if so, it stops sending and waits for an arbitrary time then tries again. This is non-destructive because it allows for the continuous transmission of a signal and thus, no messages are ever lost. This is a collision resolution and prevents messages from being destroyed hence the name, non-destructive.

## 4. Is a drawing that is not included.

## 5.	Explain the purpose of the resistors that should have appeared in your answer to above question, with reference to the differential signalling used in the Lab 1 implementation of CAN. What is the advantage of this type of signaling?

The 120 OHM resistor should have appeared in the above schematic but was not implemented because our bus was short and did not require a termination resistor and thus termination was irrelevant. This was because there would be plenty of time for the reflections (from high to low or vice versa) introduced from a wire impedance to die out as our baud rate is not very high. The termination resistors purpose is to prevent reflections in the BUS cables. As the CAN implemented in lab 1 used differential wires to send signals, the difference between the CAN_High and CAN_Low gave the required signal that had to be sent to the BUS. The advantage of differential signalling is to prevent any disturbances from changing the value on the BUS especially in harsh environments. Any change to the signal will occur to both wires and thus their difference will still remain the same which prevents changes from occurring.

## 6.	Explain how was the lab set up to try to prevent a faulty implementation by a group from interfering with all the important responses from the TA station.

Every node in the BUS was to set up a filter that only accepted the signals sent from the TA station. The TA station would detect any spamming occurring from any of the nodes and automatically report that to the TA. The TAS identifier was of highest priority so when sending a signal on the bus, any faulty implementation would not interfere with the TA’s response as that signal would be cut off due to arbitration.
