================================================================================
INSTALLING HUDSON
================================================================================

This can be downloaded from http://github.com/hugojosefson/hudson.josefson.org

--------------------------------------------------------------------------------
NOTE: These instructions are typed from memory, so please send any comments and
      suggestions for improvement to hugo@josefson.org

      You may alternatively fork this at github and send me a pull request with
      your changes.
--------------------------------------------------------------------------------

Here goes:

1.  Install Ubuntu Server 9.04 on a dedicated (possibly virtual) machine.
    # Let the admin account be named something like 'admin' or 'administrator'. Not 'hudson'.
    # ¡IMPORTANT! Activate automatic installation of security updates. Included in latest Ubuntu version. In older versions you might need to install "unattended-upgrades" and also configure it correctly. I say go with the latest Ubuntu version.

-----
As the administrator user:
-----

2.  sudo aptitude install openjdk-6-jre-headless xinetd git-core subversion
    # this java runtime is just for launching hudson, not for builds. you can let hudson download various versions of build tools (sun jdk, maven and ant) from the internet once you're up and running.
    # xinetd is used for publishing hudson on port 80 in a secure way.
    # git is needed for downloading my scripts during setup, and can also be used for builds inside hudson later.
    # subversion is commonly used for builds in hudson, so it's appropriate to install in any case.

3.  git clone git://github.com/hugojosefson/hudson.josefson.org.git

4.  sudo cp hudson.josefson.org/etc/init.d/hudson /etc/init.d/
    sudo chmod +x /etc/init.d/hudson
    sudo update-rc.d hudson defaults

5.  sudo cp hudson.josefson.org/etc/default/hudson /etc/default/

6.  sudo cp hudson.josefson.org/etc/xinetd.d/redirect-hudson-80 /etc/xinetd.d/
    sudo nano -w /etc/xinetd.d/redirect-hudson-80 # change the external ip address in there to your server's ip address, and save
    sudo /etc/init.d/xinetd reload

7.  sudo adduser --disabled-password hudson
    # this is the account which will run hudson

8.  sudo su - hudson

-----
As the 'hudson' user:
-----

9.  wget http://hudson.gotdns.com/latest/hudson.war

10. exit

-----
As the administrator user:
-----

11. sudo reboot

When the server has rebooted, you should be able to browse to http://your_server_ip_address/ and find hudson there.
First, go to Manage Hudson, Configure and add at least one JDK. You may also want to add one or more versions of Maven and Ant. 
