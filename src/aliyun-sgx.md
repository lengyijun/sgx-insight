- install linux-sgx-driver
	- ```
	  sudo yum install kernel-devel
	   sudo yum install kernel
	    sudo reboot
	    
	    cd linux-sgx-driver
	     make
	      sudo mkdir -p "/lib/modules/"`uname -r`"/kernel/drivers/intel/sgx"  
	       sudo cp isgx.ko "/lib/modules/"`uname -r`"/kernel/drivers/intel/sgx"  
	       sudo sh -c "cat /etc/modules | grep -Fxq isgx || echo isgx >> /etc/modules"    
	       sudo /sbin/depmod
	       sudo /sbin/modprobe isgx
	       
	       
	  ```
-
- docker run -ti -v /home/prusti/backup/bin:/opt/bin --device /dev/sgx/enclave --device /dev/sgx/provision hyperledger/fabric-ccenv
-