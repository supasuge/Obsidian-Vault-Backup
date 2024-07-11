## Table of Contents

          - [Resources](#Resources)
          - [Resources](#Resources)

###### Resources
- [How to read pinouts(Youtube - Terry Sturtuvant)](https://www.youtube.com/watch?v=x4QgTwWuhdM)
- [Closer Look at a CPU](https://www.cs.emory.edu/~cheung/Courses/355/Syllabus/5-bus/CPU.html)    
___

- A **Micro-processor** is a complete CPU on a single chip.
![[Pasted image 20240101013801.png]]


- Pins attached to the chip allows the computer manufacturer to connect the CPU onto a "motherboard":

![](https://www.cs.emory.edu/~cheung/Courses/355/Syllabus/5-bus/motherboard.gif)


- The mother board contains **wiring** to connect the CPU to other components of the computer:

    
    
- The pins on a CPU chip are used as input and outputs to the CPU.
    
    Each pin signals a specific data, function or operation
    
    Every signal pin has a **signal name**, e.g., MREQ (memory request), READ, etc
    
- We say that a **signal** is **"asserted"** if the CPU is **requesting** the **operation** that is conveyed by the **signal**.
    
    To **assert** a **request**, the manufacturer of the CPU can choose **either** the **value 0** or the **value 1** for that signal.
    
    For example, the signal **MREQ** means **"Memory Request"**, i.e., the CPU requests a transfer operation with the memory. The manufacturer of the CPU can choose
    
    |   |
    |---|
    |- **MREQ = 1** to indicate that the **CPU wants to request a memory transfer** operations,       **OR**      <br>- **MREQ = 0** to indicate that the **CPU wants to request a memory transfer** operations|
    
    To denote the fact that **MREQ = 1** to indicate that the CPU **asserts** the **memory request**, the MREQ signal is written on the spec (specification) sheet as:
    
    ![](https://www.cs.emory.edu/~cheung/Courses/355/Syllabus/5-bus/assert1.gif)
    
    On the other hand, if the manufacturer of the CPU decides that **MREQ = 0** indicates that the CPU **asserts** the **memory request**, the MREQ signal is written on the spec sheet as:
    
    ![](https://www.cs.emory.edu/~cheung/Courses/355/Syllabus/5-bus/assert2.gif)
    
    ---
    
    ---
    
    ---
    
- Here is a **pin diagram** of the **8085A** (pretty old chip)
    
    |   |
    |---|
    |![](https://www.cs.emory.edu/~cheung/Courses/355/Syllabus/5-bus/FIGS/pin_diagram.jpg)|
    
    You can see that **some pins** are **asserted** on **high (= 1)** (such as **INTR**), while **other pins** are **asserted** on **low (= 0)** (such as **INTA**) !!!
    
    (Just remember that **"Asserted"** mean: **"the function/request was activated"**)
    
    ---
    
    ---
    
    ---
    
    ---
    
- The pins on a CPU chip can **functionally** be devided into **3 groups**:
    
    ![](https://www.cs.emory.edu/~cheung/Courses/355/Syllabus/5-bus/pins.gif)
    
    - **Address pins** are used to **convey** the **value** of the **memory address** (and also the **address** of an **I/O device** as will will see later)
        
    - **Data pins** are used to **convey** the **data** on the **data bus**
        
    - The **3rd group** are called **control pins** --- because they carry **control signals** between the **CPU** and **memory/IO device**.
        
        We will **discuss** these **pins** next...
        
    
    ---
    
    ---
    
    **Control pins:** (categorized)
    
    |   |
    |---|
    |- **Bus control:** output pins of the **CPU** that are used to **tell the memory and IO devices** which **operation** they should perform.<br>    <br>    The most important signals are:<br>    <br>    - **MREQ:** if asserted, the **CPU** is **making a request** to the **memory**<br>    - **IOREQ:** if asserted, the **CPU** is **making a request** to an **IO device**<br>    - **READ:** if asserted, the **CPU** is making a **read request** and  <br>                    if **not asserted**, the CPU is making a **write** request.<br>        <br>    - Note: the CPU may not any read/write requests at all. In this case, both MREQ and IOREQ are not asserted.<br>    <br>    ---<br>    <br>- **Status:** output pins indicating the current status of the CPU (allows the memory and IO devices to check on the state of the CPU)<br>    <br>    ---<br>    <br>- **Interrupt:** pins for IO devices to **synchronize** with the CPU. The most important signals are:<br>    <br>    - **IREQ** = interrupt request. Signal sent by an IO device to request the attention of the CPU<br>    - **IACK** = interrupt acknowledge. Signal sent by the CPU to signal the IO device that it is ready to serve the IO device<br>    <br>    ---<br>    <br>- **Bus arbitration:** pins used by CPU to request/acquire the right to use the system bus<br>    <br>    Every device connected to the system bus (including the CPU) must first make a request to use the system bus before it can actually use it. This must be done because it is not possible to allow multiple devices to transmit their electrical signals simultaneously on the system bus.<br>    <br>    ---<br>    <br>- **Co-processor signaling:** signal used by the CPU to communicate with its "co-processor" (e.g., math co-processor that perform complex calculations).|
    
    ---
    
    ---
    
    ---
    
    ---
    
- **Example: data sheet of M68000**
    
    - Here is a copy of the **M68000** data sheet: [**click here**](https://www.cs.emory.edu/~cheung/Courses/355/Syllabus/5-bus/Docs/M68000.pdf)
        
    - The signals from the **M68000** are given by the following diagram:
        
        |   |
        |---|
        |![](https://www.cs.emory.edu/~cheung/Courses/355/Syllabus/5-bus/FIGS/M68000.png)



###### Resources
- [How to read pinouts(Youtube - Terry Sturtuvant)](https://www.youtube.com/watch?v=x4QgTwWuhdM)
- [Closer Look at a CPU](https://www.cs.emory.edu/~cheung/Courses/355/Syllabus/5-bus/CPU.html)    
___
