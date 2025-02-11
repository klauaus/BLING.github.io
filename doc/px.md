# Use Px as Local Proxy on a Windows Host

This documentation is __for Windows hosts only without direct internet access__.

## Indroduction

Host computers without direct internet access are using a proxy server to connect to the internet. __In this case we have to provide the proxy service also for a SDK builder VM running on this host__.

We recommend to install and run __px.exe__ as local proxy server on your Windows host. This local proxy forwards http and https requests to the configured remote proxy.

Here some advantages of px.exe:

* Simple to install and configure
* Same port for http and https forwarding
* No need to store credential informations (user, password) in config file(s) because Px uses the encrypted Windows credentials.
* Preconfigured usage in the ctrlX AUTOMATION SDK QEMU VMs

For more informations about Px see [What is Px](https://github.com/genotrance/px)
 
## Installation of Px as Local Proxy Server 

* Download Px.exe from [https://github.com/genotrance/px](https://github.com/genotrance/px) - Px for Windows Latest
* Copy Px.exe to a separate folder
* Configure Px.exe

    px.exe --proxy=__address_of_your_network_proxy__:__port__ --save

!!! important 
    Px.exe provides both http and https access via default port __3128__. If this port is not available on the host it can be changed in px.ini BUT: The alternative port has to be changed in the VM too - see [Setup a QEMU VM](setup_qemu_ubuntu.md) and [Setup a Virtual Box VM](setup_windows_virtualbox_ubuntu.md). 


* Add Px to the Windows registry to run on startup:

    px.exe --install


Enter this command for further usage of Px:

    px.exe --help


## Proxy Settings in the SDK Builder VM

As mentioned a VM running on a host without direct internet access has to use a proxy server too. 

For the VM Px on the host is reachable via the URL __http://10.0.2.2:3128__

Note: 10.0.2.2 is the IP address of the host from the perspective of the VM. See also [QEMU Virtual Machine Networking](setup_qemu_ubuntu.md#qemu-virtual-machine-networking)

For your information only - do not change if not necessary:

This URL is stored as so called "Proxy Settings" in the VM e.g. in following files:


* /etc/environment *1)
* /etc/wgetrc
* ~/.nuget/NuGet/NuGet.Config

*1) Here these environment variables are defined:

* http_proxy="http://10.0.2.2:3128"
* https_proxy="http://10.0.2.2:3128"
* HTTP_PROXY="http://10.0.2.2:3128"
* HTTPS_PROXY="http://10.0.2.2:3128"
* no_proxy=localhost,127.0.0.1,.local
 
## Troubleshooting

Check proxy function

    wget http://www.boschrexroth.com

If no error occurs the internet access via the proxy server is working. In this case remove the downloaded file.

    rm index.html*

Check the following points if there are problems with Px:

* SDK Builder VM: Are the proxy settings fitting to the px.ini file on the host?
* For more informations about the settings in px.in see [px.ini on github](https://github.com/genotrance/px/blob/master/px.ini)
* Host PC: Is another proxy running e.g. cntlm? 
* Host PC: Enable logging with parameter log = 1 in px.ini
