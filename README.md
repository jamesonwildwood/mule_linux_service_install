# Mule ESB Ubuntu/Debian Service Installation
Init.d Script for Running Mule ESB in Debian and Ubuntu as a service. RHEL is similar

1) Create directory /usr/local/mule

2) Untar Mule ESB into /usr/local/mule creating ./mule-enterprise-3.6.2

3) Instead of /usr/local/mule/mule-enterprise-3.6.2 which is typical when you unzip I 
create a symbolic link:
```
ln -s /usr/local/mule/mule-enterprise-3.6.2 /usr/local/mule/mule-dev
```
This way when you install a new version, you can unzip into another folder and simply 
move the symlink

## Init.d script
```
Place the etc/init.d/mule-dev script in /etc/init.d/
sudo chown root:root mule-dev
sudo chmod 755 mule-dev
sudo update-rc.d mule-dev defaults
```
## Init.d parameter file
```
Place the etc/mule/mule-dev parameter file in /etc/mule [need to create directory]
sudo chown -R root:root /etc/mule/
sudo chmod 700 /etc/mule
sudo chmod 600 /etc/mule/mule-dev
```

Edit the parameter file for the variables that you need.  I suggest also renaming the 
scripts as appropriate, especially if you have multiple mule instances on the same server. 
For example mule-qa1 mule-qa2 mule-highvolume

## Wrapper.conf
Notice that the wrapper.conf file has variables in a few places and that these variables 
are being represented in the init script.  Before you overwrite your wrapper.conf note 
down any customizations you may have done to the file.  And optionally add them to the 
init.d script or the parameter file.  

```
Wrapper.conf goes in
/usr/local/mule/mule-dev/conf
```

## Other things to note about Mule Instance Installation
* Place a fresh copy of JDK 1.7 into the /usr/local/java directory, you may want to also 
have a copy of 1.8 but at the time of this writing 1.7 was supported in Mule 3.6.x

* Change the user that Mule is running under generally.  I have it listed here as 'root' that's 
required under some circumstances: Opening ports under 1024 for example. If it's possible 
to run Mule under it's own user, then do so.  Make sure to get the hidden directories as well 
```
sudo chown -R mule /usr/local/mule/mule-dev
sudo chown -R mule /usr/local/mule/mule-dev/.mule
sudo chown -R mule /usr/local/mule/mule-dev/.mule/.agent
```

