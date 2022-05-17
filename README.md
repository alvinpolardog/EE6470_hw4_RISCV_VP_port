# EE6470 HW4
Gaussian Filter ported to the RISCV Virtual Platform

How to run:
1. Log in to the Docker provided by EE6470
2. Download the RISCV-VP repo to $EE6470
```
$ cd $EE6470
$ git clone https://github.com/agra-uni-bremen/riscv-vp.git
$ cd riscv-vp
```

3. Build a local version of the SystemC and softfloat library:
```
$ cd vp/dependencies
$ ./build_systemc_233.sh
$ ././build_softfloat.sh
```

4. Add the 'basic-acc' directory to platform with the additional sobel and gaussian filter module
```
$ cp -r {downloaded_directory}/platform/* $EE6470/riscv-vp/vp/src/platform
$ cd $EE6470/riscv-vp/vp/build
$ cmake ..
$ make install
```
5. Copy the riscv-vp testbench to /sw
```
$ cp -r {downloaded_directory}/sw/* $EE6470/riscv-vp/sw
```
6. Build Gaussian Filter
```
$ cd $EE6470
$ cd riscv-vp/sw
$ cd basic-gauss
$ make
$ make sim
```
7. Resulting image is saved as 'lena_std_out.bmp'


吳哲廷 學號:110061590
