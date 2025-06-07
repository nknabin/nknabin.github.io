---
layout: post
title: "How to use SSH with bridged networking on VirtualBox"
date: 2022-06-19 15:45:10 +0545
categories: virtualization
---

Using two virtual machines can drive your hardware to its limits. If you can run the server (or client too) in the background, i.e., without any GUI window, the amount of load on your CPU and memory will be reduced.

The idea is to start your virtual machine in headless mode and use ssh to login to the server. When a virtual machine is started in headless mode, no GUI window is attached to it. Instead, it is just started on the background. Then, you can use ssh to log in to the virtual machine and do your work. 

### Before we start

This tutorial assumes that you have a server installed inside a virtual machine and this virtual machine uses bridged networking.

If not, see [here]({% post_url 2022-05-24-install-ubuntu-on-virtualbox %}) to install Ubuntu in VirtualBox and [here]({% post_url 2022-06-19-use-virtualbox-bridged-networking-for-client-server-setup %}) to make the server use bridged networking.

### Start server in headless mode

Follow the steps below to start a guest machine in headless mode:

1. List your vms.<br>
   ```
   $ VBoxManage list vms
   ```

2. Start your vm in headless mode.<br>
   ```
   $ VBoxManage startvm <vm-name> --type headless
   ```

### Use ssh to log in to the server

This requires ssh server to be installed and enabled on boot so that ssh server is started and ready for use when the server boots up.

Note that if you use DHCP configuration, the server will change its IP address when it requires to. This makes it not possible to use ssh like this. Therefore, configure your server to use a static IP address. This way, you can always use one IP address to access your server. This is also how servers are accessed in real world, using a fixed ip address via ssh.

```
$ ssh <username>@<hostname>
```

For hostname, use the IP address of the server obtained from ip or ifconfig command.

### Tip
You can also start your guest machine in Detachable mode, which allows you to close the GUI when it's not needed, while keeping the machine running in the background. Select your guest and start it on Detachable Start by selecting from Machine > Start in the menu bar or the drop-down arrow in the toolbar. You can then hide the window when required by selecting Continue running in the background when closing the window. Show this window again by selecting the guest and clicking on Machine > Show in the menu bar or Show in the toolbar.

### Conclusion

You have successfully started the server in the background and logged in to the server using ssh. This configuration can be applied to any other virtual machines.

### Troubleshooting

- Unreachable host<br>

  Either the virtual machine did not start or you are using an incorrect IP address. Restart the virtual machine again.

  See if you are using the correct IP address for the server. If you are not using static IP, server may be using a different IP address from the last time. Start with guest from VirtualBox main window and check the IP address from within the guest window.

- Connection refused<br>

  The ssh server is not running. Go to VirtualBox main window, start the server with a GUI. Troubleshoot for ssh server (reinstall the server and enable it to start on boot).

  Lastly, check if you are using the correct username or the correct password.
