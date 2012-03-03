OurCraft-init-script-Fork
=========================================================================
OurCraft-init-script-Fork is a fork of [Codeprogrammers.net](http://codeprogrammers.net) - Codecraft Init Script.
It is the code changes used for OurCraft minecraft server.
I recommend you use the [orginal script](https://github.com/Codeprogrammers/codecraft-init-script).

*These scripts are free to use and modify under the GPLv2 license.*

This is a Linux service script for starting a Minecraft or Bukkit server as a service on Debian and Enterprise Linux based systems.

Features
--------

 * Start Minecraft/Bukkit server on system startup
 * Start and stop the service properly on runlevel change (Startup, Shutdown, Restart, etc.)
 * Option to load worlds in ramdisk
 * Backup command for worlds and ramdisk
 * Remote access to Minecraft/Bukkit console with Screen and SSH
 
Requirements
------------

 * Screen

DISCLAIMER
----------

We are not responsible for any damage or lost data this script may cause to your server. 
These scripts are still in development and should be considered beta quality.

It is **highly recommended** that you test this script out yourself in a controlled environment before using it with your production server.

Setup
=====

This script is designed to be as easy to use, and as useful as possible. However, there is a bit of setup required.

Step 1: *Directory Setup*
-------------------------

This script expects certain folders to be placed in certain directories. We'll be telling the script the name of these folders in Step 2, but we need to create them first.
Any variable ending in `PATH` should contain a full directory path (i.e. anything that starts with "/", for example: `/mnt/codecraft`).

* The exception to this is the `ROOT_BUPATH` variable which can also begin with the `ROOT_PATH` variable instead of a "/"

Any variable ending in `DIR` should contain the **name of the folder only**.

1. Make the root directory for your server. This will be used by the `$ROOT_PATH` variable.

2. Make a folder for your server versions/configuraitons inside of the root directory. This will be used by the `SERVER_DIR` variable.

3. Make a folder for your server's core files inside of the `SERVER_DIR` directory. This should be given the same name as the build or version number of the server software you're using. This will be used by the `BUILD` variable.

4. Make a folder for your worlds. This will be used by the `WORLD_DIR` variable.

5. Make a folder for the ramdisk. This will be used by the `RD_DIR` variable.

6. Make a folder for your backups. This will be used by the `ROOT_BUPATH` variable.
 * The script will make folders inside of this one later to keep your backups organized.

**Example:**
    ROOT_PATH="/mnt/codecraft"
    WORLD_DIR="worlds"
    SERVER_DIR="servers"
    RD_DIR="ramdisk"
    ROOT_BUPATH="$ROOT_PATH/archives"

Step 2:  *Installation and Configuration*
-----------------------------------------

First things first. If you haven't already, go grab the latest version of the script from the Github repo. You can find it in the downloads section.

1. Download either the vanilla minecraft server software or bukkit and place it in `$ROOT_PATH/$SERVER_DIR/$BUILD`

2. Place your worlds into the `$WORLD_DIR` directory and place symbolic links in `$ROOT_PATH/$SERVER_DIR/$BUILD` which point to the worlds in `$ROOT_PATH/$WORLD_DIR`
 * To do this, open up a terminal and type the following (replacing the variables with your chosen directory paths):

      cd $ROOT_PATH/$SERVER_DIR/$BUILD
      ln -s $ROOT_PATH/$WORLD_DIR/[world_name_here] [world_name_here]

 * Repeat the last command for every world on your server.
 * **Note:** If you don't currently have any worlds, you'll need to run the service first (bukkit/minecraft will automatically create them), and then move the worlds into the `$WORLD_DIR` directory and issue the afformentioned commands.

3. Extract the script from the tarball and change the settings and file paths/directory names as you see fit. More information on configuraiton can be found in the Notes section.

4. Place the script in your /etc/init.d folder and make sure the script is executable (just `chmod +x minecraft` and that should do it).

5. Configure the script to start Minecraft/Bukkit on startup and stop it on shutdown.
 * __*RHEL / CentOS:*__ Run in terminal `chkconfig --add minecraft`
 * __*Debian / Ubuntu:*__ Run in terminal `update-rc.d -n minecraft defaults`

Step 3:  *Start the Service*
----------------------------

As root, run `service minecraft start`

<br />
###Congratulations, you now have Minecraft/Bukkit running as a service on your server!

As always, make sure your firewall, plugins, and bukkit/vanilla minecraft are configured properly. Though this script may still be in development, if your server fails to start properly or it starts and you can't connect to it (especially the latter), it's likely due to an issue with your configuration, so check that first before reporting bugs.

<br />
Notes
=====

This scrript starts up the Minecraft server (or bukkit, or whatever you use) on system boot as a service.

This script **MUST** be run as root or as a user with mount priveleges if the `$RAMDISK` variable is set to "true".
<br />

Available commands
------------------

<table>
  <tr>
    <td><b>start</b></td> <td>Starts the service</td>
  </tr>
  <tr>
    <td><b>stop</b></td> <td>Stops the service (initiates save-all and then stop)</td>
  </tr>
  <tr>
    <td><b>restart</b></td> <td>Restarts the service (stop followed by start, or just start if service is not running)</td>
  </tr>
  <tr>
    <td><b>status</b></td> <td>Displays the status of the service (Whether or not it's running)</td>
  </tr>
  <tr>
    <td><b>backup worlds</b></td> <td>Backs up your worlds to <i>$ROOT_BUPATH/$WORLD_BUDIR</i></td>
  </tr>
  <tr>
    <td><b>backup ramdisk</b></td> <td>Backs up the entire ramdisk to <i>$ROOT_BUPATH/$RD_BUDIR</i></td>
  </tr>
</table>

<br />
Settings
--------

There are many variables in this script which you may modify to suite your
operating environment.

<table>
  <tr>
    <td>**USERNAME**</td>
    <td>This is the username which the Minecraft server will run under</td>
  </tr>

  <tr>
    <td>**GROUPNAME**</td>
    <td>This variable should be set to the group which will have access to all files related to your Minecraft server.</td>
  </tr> 

  <tr>
    <td>**SCREEN_NAME**</td>
    <td>
      The script starts the service in a screen session for easy remote accessibility. <br /> 
      This variable sets the name of the session.
    </td>
  </tr> 
  
  <tr>
    <td>**BUILD**</td>
    <td>
      This should be set to the build number or version of the server you're running (like b1337 for bukkit, or v1.8.1 for vanilla). <br />
      This is only used when looking for the directory your server runs under (for example, our server has directories named "b1240" and "b1337", and since we run off of b1337 as it's the most recent, we set this variable to b1337).
    </td>
  </tr> 

  <tr>
    <td>**SERVICE**</td>
    <td>
      This should be set to the name of the .jar file which starts your server (craftbukkit.jar for bukkit, minecraft.jar for vanilla). <br />
      This must be set to the name of the .jar file used in the invocation since the service will show up in top or gnome-system-monitor with the name of the .jar file used in the invocation.
    </td>
  </tr>
  
  <tr>
    <td>**INVOCATION**</td>
    <td>
      This should be set to the command-line statements used to start your server. <br />
      For example: `java -Xmn256M -Xms512M -Xmx1024M -jar craftbukkit.jar`
    </td>
  </tr>
  
  <tr>
    <td>**RAMDISK**</td>
    <td>
      This should be set to "true" if you want your worlds to be loaded into a ramdisk. <br />
      It's highly recommended that you set up automatic backups while using this feature. <br />
      For our server, we simply set up a cron job that runs `/etc/init.d/minecraft worldbackup` every hour.
    </td>
  </tr>

</table>

**Note:** This script will implement automatic backups in the future as a toggleable feature (similar to the ramdisk feature).

<br />
### File Paths:

* **ROOT_PATH**
 * This should be set to the directory location where all files pertaining to your Minecraft server will be stored.
 * Since this is a path variable and not a directory variable, this can point to wherever you like.
 * For example: `/mnt/codecraft`

* **WORLD_DIR**
 * This is the name of the folder where your worlds will be located.
 * This script expects the `$WORLD_DIR` folder to be located at `$ROOT_PATH/$WORLD_DIR`
 * For example, if `WORLD_DIR="worlds" then it might be located here: `/mnt/codecraft/worlds`

* **SERVER_DIR**
 * This is the name of the folder where your server files will be located.
 * This script expects the `$SERVER_DIR` folder to be located at `$ROOT_PATH/$SERVER_DIR`
 * For example, if `SERVER_DIR="servers" then it might be located here: `/mnt/codecraft/servers`

* **RD_DIR**
 * This is the name of the folder where your ramdisk will be located.
 * This script expects the `$RD_DIR` folder to be located at `$ROOT_PATH/$RD_DIR`
 * For example, if `RD_DIR="ramdisk" then it might be located here: `/mnt/codecraft/ramdisk`

* **ROOT_BUPATH**
 * This is the directory location where all your backups will be stored.
 * Since this is a path variable and not a directory variable, this can point to wherever you like.
 * For example: `/mnt/codecraft/archives` or `$ROOT_PATH/archives` or even `/opt/codecraft/archives`
  
<br />
### Messages:

These variables represent messages which are sent to the bukkit/vanilla console with the "say" command. They should be fairly self explanitory. For example,
when you use the "worldbackup" command, SAVE_OFF_MSG will be displayed to users in-game, followed by BACKUP_START_MSG and, if the backup is successful, followed
by BACKUP_DONE_MSG. It should end with SAVE_ON_MSG.

##### Examples:
<table>
  <tr>
    <td><b>SAVE_OFF_MSG</b></td> <td>Disabling saving. Server going readonly...</td>
  </tr>
  <tr>
    <td><b>SAVE_ON_MSG</b></td> <td>Enabling saving. Server going read-write...</td>
  </tr>
  <tr>
    <td><b>BACKUP_START_MSG</b></td> <td>SERVER BACKUP STARTING</td>
  </tr>
  <tr>
    <td><b>BACKUP_RD_START_MSG</b></td> <td>RAMDISK BACKUP STARTING</td>
  </tr>
  <tr>
    <td><b>BACKUP_DONE_MSG</b></td> <td>SERVER BACKUP COMPLETE</td>
  </tr>
  <tr>
    <td><b>BACKUP_FAIL_MSG</b></td> <td>SERVER BACKUP FAILED</td>
  </tr>
  <tr>
    <td><b>STOP_MSG</b></td> <td>SERVER SHUTTING DOWN IN 15 SECONDS</td>
  </tr>
  <tr>
    <td><b>RESTART_MSG</b></td> <td>SERVER GOING FOR RESTART</td>
  </tr>
</table>
