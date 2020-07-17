# JetsonXavierAGX
Tipps and Tricks for the JetsonXavierAGX

## Installation and Flashing

### First of all make a full install of Ubuntu 16 to some drive, do not use a USB or Try out version since this causes issues.
### Second make sure that there is a large enough device with ext4 to store the data from Ubuntu + the Flashing tools

Now you can install the SDK Manager. 
The Deb file can be aquired here https://developer.nvidia.com/nvidia-sdk-manager
Install command 'dpkg -i SDKMANAGERNAMEHERE.deb'

Plug in the USB to the AGX and boot it to FlashMode (Viewing on the buttons press the most right and the middle button)

Now follow the main procedure to install the OS and the required packages. If you encounter an error because the device is offline check the connection (Reboot to Flashmode again and retry)

Once the SDK Manager is finished, jack in a Keyboard and the HDMI of a Monitor and reboot the AGX.
Follow the ubuntu install instructions.


# Install Python and Tensorflow

Also you can check this ressource https://docs.nvidia.com/deeplearning/frameworks/install-tf-jetson-platform/index.html

## Installing Python
sudo apt-get update
sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran

sudo apt-get install python3-pip
sudo pip3 install -U pip testresources setuptools

sudo pip3 install -U numpy==1.16.1 future==0.17.1 mock==3.0.5 h5py==2.9.0 keras_preprocessing==1.0.5 keras_applications==1.0.8 gast==0.2.2 futures protobuf pybind11


## Install tensorflow (MOST CURRENT)
sudo pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v44 tensorflow

## Install tensorflow 1.x
sudo pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v44 ‘tensorflow<2’

## Install specific version of tensorflow
Also it is possible to install a specific tensorflow version
https://developer.download.nvidia.com/compute/redist/jp/v44/tensorflow/

## Install Pytorch
To install pytorch one can use the whl files in the following post
https://forums.developer.nvidia.com/t/pytorch-for-jetson-nano-version-1-5-0-now-available/72048

## Special Pytorch + Tensorflow Settings
Sometimes if Tensorflow is used with Pytorch, it can try to use all memory.
This is of course bad news, because the RAM is a combined RAM for GPU and CPU.
To prevent a overallocation for CUDA one can Limit the RAM CUDA uses. For TF2 this looks like this:

gpus = tf.config.experimental.list_physical_devices('GPU')
tf.config.experimental.set_virtual_device_configuration(gpus[0], [tf.config.experimental.VirtualDeviceConfiguration(memory_limit=CUDARAMINMB)])

CUDARAMINMB Is the CUDA RAM Limit in MB. The two lines above can be placed after the import of Tensorflow.

Also when using Tensorflow with Pytorch, there often is a weird error message about a block issue.
This happens when Tensorflow is imported first.
Make sure to import Tensorflow AFTER Pytorch, then it will work.





# Change Powermode

Check this ressource on the topic: https://docs.nvidia.com/jetson/l4t/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/power_management_jetson_xavier.html

To change powermode run following command where x is the mode index.

sudo /usr/sbin/nvpmodel -m <x>
  
Mode 2 is the default setting.
Mode 0 is the fullpower mode.

The command below will display the current mode
sudo /usr/sbin/nvpmodel -q

## Alternativly run

Alternativly  the command below can be used to switch into high performance mode.
On Reboot the AGX will be again in default setting.
The command will max out all clocks from all CPUS and GPUS also activate all CPUS and set Fanspeed to 255.

sudo /usr/bin/jetson_clocks

## Check Fanspeed

### Fanmode

Run command below where mode is quiet or cool
sudo /usr/sbin/nvpmodel -d MODE

## Manually override fanspeed

Run command below where X is value in 0-255. 

echo X | sudo tee /sys/devices/pwm-fan/target_pwm

To view the current fanspeed run command below
sudo cat /sys/devices/pwm-fan/target_pwm



## Install JTOP => Highly recommend it.

Ressource https://github.com/rbonghi/jetson_stats

### Installing:

sudo -H pip install -U jetson-stats

### Running:

sudo jtop

