## Tools

* ### SSH key
  * To generate ssh key for making connections, please go to `.ssh` directory by entering `cd .ssh/` in your home directory. In case you do not have the directory, you can use `mkdir .ssh` in your home directory.
  * Try `ssh-keygen`, it would ask you to enter the name and the keyphrase(password for the key file).
  * After the generation, you would see two file one is the `<name>` your enter before, this one is your private ssh key; the other is `<name>.pub`. `<name>.pub` contains the public ssh key that you need to make connections. Try `cat <name>.pub` to see your public ssh key.
  
* ### Cisco Anyconnect VPN to Buffer1
  * Go to [SNOLAB home page](https://www.snolab.ca/), then choose **Internal** to go to the internal site, which would require a snolab account.
  * In **IT Services**, choose **Computing Support Page**.
  * In **Work Tools** section, select **VPN - Virtual Private Network**.
  * Then you can choose the version to install according to your Operating System.
  
