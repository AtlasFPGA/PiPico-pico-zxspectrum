# PiPico-pico-zxspectrum
Instruction for compile pico-zxspectrum from fruit-bat for atlasfpga with PiPico-fpga-relocator

You need main board AtlasFPGA, that you can check at:
https://github.com/theexperimentgroup/Atlas-FPGA

After you need the PiPico-fpga-Relocator for can plug a pipico cpu:
https://github.com/atlasFPGA/piPico-Fpga-Relocator/

I compile with a Debian version 9.13. Maybe you need install some tools like git version as Pipico SDK need.

Code to run:
```
cd ~

mkdir pipico
cd pipico
git clone https://github.com/raspberrypi/pico-sdk

export PICO_SDK_PATH=~/pipico/pico-sdk
cd $PICO_SDK_PATH/lib/
mv tinyusb tinyusb.orig
git clone https://github.com/fruit-bat/tinyusb.git
cd tinyusb
git checkout hid_micro_parser

cd ~/pipico/
git clone https://github.com/Wren6991/PicoDVI

cd PicoDVI/software/apps
git clone https://github.com/fruit-bat/pico-zxspectrum.git zxspectrum
git clone https://github.com/fruit-bat/no-OS-FatFS-SD-SPI-RPi-Pico.git
git clone https://github.com/fruit-bat/pico-dvi-menu
git clone https://github.com/fruit-bat/pico-emu-utils

Change in file "zxspectrum/src/hw_config.c" next line:
.card_detected_true = 1
with
.card_detected_true = -1

Add to CMakeLists.txt file next lines (others can comment out with #):
add_subdirectory(pico-dvi-menu)
add_subdirectory(pico-emu-utils)
add_subdirectory(zxspectrum)
add_subdirectory(no-OS-FatFS-SD-SPI-RPi-Pico/FatFs_SPI)

cmake -DPICO_COPY_TO_RAM=0 ..
make clean
make -j4 ZxSpectrum

cp apps/zxspectrum/ZxSpectrum.uf2 /media/pi/RPI-RP2/
```

For do cp maybe you need pipico at download mode, press boot and without release press/release reset on relocator board.

Thats all!
