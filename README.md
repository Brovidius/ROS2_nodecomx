# ROS2_nodecomx
## About
This is a ROS 2 system consisting of an ra-NRC optimization algorithm `algo.py`, which uses the `gradient_hessian_calculator.py` to update incoming values. [The ROS 2 is the latest Humble version](https://docs.ros.org/en/humble/index.html). The communication is achieved using a UnetStack API called `unetpy`; more information can be found at the [UnetStack documentation](https://unetstack.net/handbook/unet-handbook_preface.html). In addition, there is a module called `FP16_converter.py` for splitting a float into segments, which is used for the transmission of the required data by the optimization algorithm; the transmission is using characters as the datatype for transmission. This  is developed under the Ubuntu OS, and should be run in the same envirom

### FP16_converter.py
 This module is used for splitting the floats within this project. The `processing_node.py` calls the `Converter` class, and its methods like `FP16_to_int8()` or `FP16_list_to_int8`; and the oppsite with `int8_to_FP16` or `int8_list_to_FP16`. There are other methods there that handle FP32, but they are currently not used within this repository. It should not be a problem by changing the returned datatypes, but they have to be alike for the splitting and merging part of this module. This module has an active external repository, and can be found [here](https://github.com/Brovidius/FP16_converter.git).

## Basic ROS 2 operation with UnetStack

### Launching a package
One launches a package by using the ROS 2 launch command, which has been implemented for this package. Executing 
```
ros2 launch nodecomx nodecomx.launch.py
```
 will launch all three nodes within the package. This information is covered more in the ROS 2 humble documentation, as mentioned above.

### Setting-up the simulation
To set up a node the `modem_info.py` needs to contain the respective modem information (private IP and portnumber). Due to the simulation, the UnetStack uses `'localhost'` as the IP and `1101` or `1102` as the portnumber. 

### Simulating two modems in one machine
In order to simulate two modems using UnetStack, one creates a new, identical package with a different name, but the same modules. The import of the required modules in the package need to be changed with respect to the new package name. As mentioned in the previous paragraph, the IP needs to be `localhost` on both of the `modem_info.py` files in each package, but the portnumbers have to be `1101` and `1102`, respectively. UnetStack documentation mentioned at the beginning covers this information and more.

### Alternative ways of achieving two nodes
One could simply duplicate the entire workspace, and before launching each node, one could seperate the ROS 2 domains by using the command 
```
export ROS_DOMAIN_ID=n
```
where the `n` is an uint-8 value. The default domain is 0, and the current domain can be found by 
```
printenv | grep ROS_DOMAIN_ID
```
However, a return will only happen if the domain has been set manually.

