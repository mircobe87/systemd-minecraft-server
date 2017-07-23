# systemd-minecraft-server

*Systemd script files to run minecraft server as a daemon to make possible run
the server at boot time.*


## Installing the server
First of all you have to download the server and put the jar file into a
dedicated directory. Then you can configure the server as usual setting up
server.propertie, whitelist, etc...


## Installing the scripts
There are two files:
 - **server**: is the executable bash script that start and stop your minecraft server instance.
 - **minecraft-server.service**: is the systemd unit file that define the service.

Put the **server** script where you want than edit it setting the `WD` variable:
make it to point to the server dedicated directory.

Now you have to edit the `[Service]` section of **minecraft-server.service** file setting the `User`,
`WorkingDirectory`, `ExecStart` and `ExecStop` properties:

- `User`: is the name of the user that will execute the server and it must have all permission on the server directory;
- `WorkingDirectory`: as its name says, it must point to the server directory (the same value for `WD` variable in the **server** script).
- `ExecStart`: the command to run the script **server** giving the `start` parameter
- `ExecStop`: the command to run the script **server** giving the `stop` parameter

After that you can put the unit file into systemd directory `/etc/systemd/system` setting *root* as owner and group and giving all execution rights to the file:

    sudo cp minecraft-server.service
    cd /etc/systemd/systemd
    sudo chown root:root minecraft-server.service
    sudo chomd ugo+x minecraft-server.service


## Starting the server
You start the server with the command

    sudo systemctl start minecraft-server.service

then stop it with

    sudo systemctl stop minecraft-server.service

If you want make the server start at boot time, you can enable it using the command

    sudo systemctl enable minecraft-server.service

To revert this configuration run the command

    sudo systemctl disable minecraft-server.service


## Some notes to the **server** script
When the script run the server, it create a file into the `WD` directory
containing the **PID** of the started server process. When you try to stop the
server, the script removes that file.

If you try to start an already running server instance, simply no actions are
performed; same behavior if you try to stop a non running server instance.
