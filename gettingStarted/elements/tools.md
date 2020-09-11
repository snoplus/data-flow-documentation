---
layout: default
title: Tools
parent: Getting Started
nav_order: 2
---

## Tools

* ### GRID Certificate
  * You would need the certificate to access many websites, for Canadaian users, go to [this website](https://cert.gridcanada.ca/cgi-bin/pub/pki?cmd=getStaticPage&name=homePage).
  * In the Step 2 - **Request a Certificate**, choose **Server Request** and enter "snoplusdata.snolab.ca" as the host name.
  * For users from other countries, please follow the instructuion [here](https://snopl.us/docs/rat/grid_manual/html/certificates_and_initial_setup.html).

* ### SSH Key
  * To generate ssh key for making connections, please go to `.ssh` directory by entering `cd .ssh/` in your home directory. In case you do not have the directory, you can use `mkdir .ssh` in your home directory.
  * Try `ssh-keygen`, it would ask you to enter the name and the keyphrase(password for the key file).
  * After the generation, you would see two file one is the `<name>` your enter before, this one is your private ssh key; the other is `<name>.pub`. `<name>.pub` contains the public ssh key that you need to make connections. Try `cat <name>.pub` to see your public ssh key.
  
* ### VPN
  You would require a VPN to access Buffer1, please follow the instructions to install.
  * Go to [SNOLAB home page](https://www.snolab.ca/), then choose **Internal** to go to the internal site, which would require a snolab account.
  * In **IT Services**, choose **Computing Support Page**.
  * In **Work Tools** section, select **VPN - Virtual Private Network**.
  * Then you can choose the version to install according to your Operating System.
  
* ### Mailing lists
  Once you sign up the snolab account, you can join the mailing list. Two of the most important mailing lists are:
  * snoplus_vosupport@snolab.ca
  * snoplusdata@snolab.ca
  
  There are two way to join the list, feel free to choose the one that you are comfortable with:
  * Click [here](https://www.snolab.ca/sympa/search_list_request) and search the mailing list you want to join.
  * Email sympa@snolab.ca in the following format:
    ```
    subscribe <lists> <First name> <Last name>
    ```
