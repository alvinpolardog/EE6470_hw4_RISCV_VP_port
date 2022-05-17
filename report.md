# EE6470 HW4 Report
吳哲廷 學號:110061590

Link to [Github Repo](https://github.com/alvinpolardog/EE6470_hw4_RISCV_VP_port)
##

## Cross-compile Gaussian Blur to RISC-V VP platform

In HW4, we are tasked with porting the TLM gaussian filter created in the previous homeworks to the riscv virtual platform.
In this gaussian filter implementation, the Gaussian blur module receives one row of data at a time from the testbench, and store at most three row of data at any time.
The data transfer was done via DMA or direct memory access, allowing for lower latency between the modules.

### Modules
To port the gaussian filer to the risv-vp, we used the 'basic-acc' platform created in Lab 8. The gaussian filter is almost identical
to the version in HW2, with some modification to simplify the transfer of data. 'main.cpp' on the new platform was altered to include the new
module, and the bus was extended to allow for the addition of one more initiator socket.

The memory mapping was directly extended beyond the sobel filter
to allow guassian filter to have its own memory space, and the memory parser was
also changed to take this new space into consideration.

Since the TLM modules have great interoperability, neither the DMA nor simplebus module have to be altered.

### RISCV VP software
The 'main.cpp' in 'basic-gauss' is modified from the 'basic-sobel'. The main function is taken from previous homework, where
each line of the image is sent one at a time. The only modification is the data reading and writing are done via the new DMA
functions. The DMA allow the main function to share its memory with the guassian module directly, so it is critical 
that the memory address in the software needs to match with that provided to the platform.

### Result
Without DMA
```
[sys_open] lena_std_short.bmp, 0 (translated to 0), 438
======================================
          Reading from array
======================================
 input_rgb_raw_data_offset      = 54
 width                          = 256
 height                         = 256
 bytes_per_pixel                = 3
======================================
Start processing...(256, 256)
[sys_open] lena_std_out.bmp, 1537 (translated to 577), 438

Info: /OSCI/SystemC: Simulation stopped by user.
=[ core : 0 ]===========================
simulation time: 602332830 ns
...
...
pc = 285e4
num-instr = 15004332
```
With DMA
```
[sys_open] lena_std_short.bmp, 0 (translated to 0), 438
======================================
          Reading from array
======================================
 input_rgb_raw_data_offset      = 54
 width                          = 256
 height                         = 256
 bytes_per_pixel                = 3
======================================
Start processing...(256, 256)
[sys_open] lena_std_out.bmp, 1537 (translated to 577), 438

Info: /OSCI/SystemC: Simulation stopped by user.
=[ core : 0 ]===========================
simulation time: 557419750 ns
...
...
pc = 285e4
num-instr = 12494454
```
In this example, direct memory access reduced the simulation time from 602332830 ns to 557419750 ns. A reduction of 7.4%.

Original Image:
![Original Image](https://github.com/alvinpolardog/EE6470_hw4_RISCV_VP_port/blob/main/sw/basic-gauss/lena_std_short.bmp)
Result Image:
![result](https://github.com/alvinpolardog/EE6470_hw4_RISCV_VP_port/blob/main/sw/basic-gauss/lena_std_out.bmp)
