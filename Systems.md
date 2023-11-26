# Systems

<details>
<summary>Preface</summary>

The design of the software is subject to change,
so use common sense and know that Google is Your best friend.

</details>

## Environment

<details>
<summary>Virtualization Clients</summary>

Whoa, whoa, whoa, don't You forgot to download [Ubuntu Server] LTS iso?...
Good for You.

### VirtualBox Set up

Download VirtualBox:

- Visit the official VirtualBox website at https://www.virtualbox.org/.
- Navigate to the "Downloads" section.
- Choose the version of VirtualBox that corresponds to Your host operating system
(e.g., Windows, macOS, Linux, etc.).
- Click on the download link to start the download of the VirtualBox installer.

And now You (probably) can run virtualbox,
by windows search bar (WIN + Q and type in "VirtualBox" and click on "Oracle VM VirtualBox").

Virtual Machine set up:

- Click on the "Quick create..." in the tools panel.
- Enter the name of the virtual machine.
- In the text box with the "ISO Image" name, enter the path to the .iso file.
- Check the box next to "Skip Unattended Instalation".
- And click finish.

To configure the virtual machine, just right-click on it and select "Settings".

To start the virtual machine,
just right-click on it and select "Start -> Normal Start".

To turn off the virtual machine,
just right-click on it and select "Stop -> Power Off".

You're done (need to celebrate with a NOT very large piece of cake)!

![Ups, the image is somewhere](./images/environment/virtualization_clients/virtualbox.png)

### Hyper-V Set up

First, You are obligated to have windows (10|11) pro.

- Open the "Control Panel" on Your Windows machine.
- Go to "Programs" -> "Programs and Features" -> "Turn Windows features on or off."
- Scroll down and find "Hyper-V."
- Check the box next to "Hyper-V" and click "OK."
- Windows will prompt You to restart Your computer. Save Your work and restart.

And now You (probably) can run Hyper-v,
by windows search bar (WIN + Q and type in "Hyper-v" and click on "Hyper-v Manager").

Virtual Machine set up:

- Select a server,
it can be Your computer or another machine You can connect to.
The list of (currently) available servers is displayed in the left panel.
- Click on the "Quick create..." in the right panel.
- In the pop-up window, click on "_Local installation source".
- Uncheck the box next to "Windows Secure Boot."
- And select the .iso file on your local storage via button
"Change installation source".
- That blue-filled button with "Create Virtual Machine" on it beckons you,
and you press it with an irresistible urge.

To configure the virtual machine, just right-click on it and select "Settings".

To start the virtual machine, just right-click on it and select "Start".

To turn off the virtual machine, just right-click on it and select "Turn off".

You're done (need to celebrate with a very large piece of cake)!

![Ups, the image is somewhere](./images/environment/virtualization_clients/hyper-v.png)

</details>

<details>
<summary>Ubuntu Server Installation</summary>

Since I am not obliged to explain all the steps of the installation..., good luck!

### Partitioning in the installer

On the storage configuration page:

- Choice the drive that will be used as boot device.
- Press enter when you choice the drive name and select: "Use As Boot Device".
- For a healthy and proper Linux installation you should create:
root ('/'), home ('/home') and boot ('/boot') partitions.
- Select free space point (under the drive name) and click on "Add GPT Partition".
- Specify the size of the partition space.
- Select the "Mount" You want it to be.

After all Mounts get created with proper size, you're done with that section.

### Partitioning using LVM (Logical Volume Manager)

Soooo, why again You should make life more difficult for yourself and use LVM?
Ah, yeah, how can i forgor, to:

- Increase flexibility - You can easily add more space to an existing volume
by adding a new drive to the group.
- Improve performance - You can spread data between drives,
so also spread the reading of that data.
- Make the system more fault tolerance - create a mirroring for the logical volume
(space - 20G, but you can use as user only 10G).

To start use LVM:

- Select "Create volume group".
- Check the devices You want to be in the group and click create.
- And after that You can use the free space of the group to create partitions
like in section above.

### Swap space

We all get hungry from time to time.
The operating system is no exception,
so we need to provide it with a special place
from which it can draw memory when it runs out of RAM.
That's what the swap space is made for.

So to specify the swap space:

- From the free space, select - "Create volume group".
- Specify the size of the swap space
- In the section "Format", choice - "swap".

And that's all You need to create swap space.

### Example Storage Configuration

![Ups, the image is somewhere](./images/environment/ubuntu_server_installation/Example_storage_configuration.png)

To run machine the drive with grub should be on the first sata port (sata 0).

</details>

<details>
<summary>Operations on Virtual Machine</summary>

### Virtualbox

#### ***Network Configuration***

To specify the network adapter:
  
- Right-click on VM and select "Settings".
- In the left panel select "Network".
- Make the "Enable Network Adapter" checked.
- Select the type of adapter ("Attached to..."): NAT, Bridge, Hostonly.

The difference between them is that:

- Hostonly: VM has no access to the network, so only host can access it.
- Nat: the VM can be accessed only from the local network.
- Bridge: any machine on the internet can access the VM.

#### ***Creating a clone of the system***

To create a clone:

- Right-click on VM.
- Select "Clone...".
- Name it and give the place for this clone.
- Decide if you want to keep MAC addresses for
network adapters or genrerate new ones.

It's your first clone (for this guide at least)!

#### ***Creating a snapshots***

To create a snapshot:

- On the right side of the VM board click on 3-dot menu.
- Select the "Snapshots" point.
- Click "Take" on the tools bar on the top.

It's your first snapshot (for this guide at least)!

### Hyper-v

#### ***Network-adapter Configuration***

To specify the network adapter:
  
- In the left panel "Virtual switch manager...".
- And choice the type of switch:
external(bridge), internal(nat), private(hostonly).
- Then "OK".
- Right-click on VM, "Settings...".
- In hardware section, "Add hardware".
- Select "Network adapter" and then "add".
- Click on the adapter
- In drop-down menu "virtual switch" select
the switch you created in point 2.

#### ***Export of the system***

To export (create a clone):

- Right-click on VM.
- Select "Export...".
- Select where to export

#### ***Creating a checkpoints***

To create a checkpoint (snapshot):

- Right-click on VM.
- Select "Checkpoint".

#### ***Microsoft specials***

Why the f**k you always tend to have your own naming...

### Clone vs Snapshots

If the options to clone and make snapshot literally do the same thing,
then why do we have these options?

Snapshots are less large in space than clones
because they are incremental copies of a virtual machine's state.
This means that they only store the changes
that have been made to the virtual machine since the last snapshot was created.

But clones in exange can be transferred from one machine to another.

</details>

## Basics of Linux system

<details>
<summary>Essential Linux Skills</summary>

<div style="margin-left: 20px;">
<details>
<summary>First login to the sHELL</summary>

### Lame way to log in

When your coolers start spinning,
the rgb lights play all sorts of hues
and the screen shows a mysterious picture.

Welcome to the user login screen!
It should show something like this:

![Ups, the image is somewhere](./images/basics/essential_linux_skills/login_screen.png)

To log in,all you have to do is enter your login (username)
and then enter the most secret password the mind has ever had a chance to create.

And yes, that's all you had to do to complete this extremely hard subsection.

### Interesting way

Psst, psst, reader, do you like to remember passwords?

Yeah, me too. So why shouldn't we create additional login options?

You know, something like fido2, u2f security key,
or maybe we should use our own flesh for authentication?

First of all we must have some security authentication usb device.
It is a great pity that I do not have one.

In that case... We will use a simple usb drive!

We will use the [pam_usb] tool to achieve our destiny.

- Update the apt using "sudo apt update".
- Install all packages form the instruction for "debian based" on [pam_usb].
- Also install make, gcc.
- Make "git clone 'link here'".
- Move Yours current position to the pam_usb folder.
- Use "make" command.
- And "sudo make install"

All that's left to do is configure [pam_usb].

- Find the name of your us drive by "sudo fdisk -l"
- Find the name of user, who'll be log in by usb.
"getent passwd | awk -F: '$3 >= 1000 && $3 <= 60000 {print $1}'"
($3 = UID)
- Add the device using "sudo pamusb-conf --add-device YourDeviceName"
- And then user "sudo pamusb-conf --add-user YourUserName"
- The last task is to add our authentication method
so open file "/etc/pam.d/common-auth" via root
and add "auth sufficient pam_usb.so" to the top of the file.

The result of these efforts - will be a folder with a file on the drive:

![Ups, the image is somewhere](./images/basics/essential_linux_skills/password_usb0.png)

![Ups, the image is somewhere](./images/basics/essential_linux_skills/password_usb1.png)

And the login process now looks something like this:

![Ups, the image is somewhere](./images/basics/essential_linux_skills/login0.png)

![Ups, the image is somewhere](./images/basics/essential_linux_skills/login1.png)

</details>

<details>
<summary>Command line help</summary>

In this cruel world of injustice and suffering,
ahem... in our beloved linux (and especially ubuntu),
You, my amigo, definitely need reliable friends!

### man

First thing that you should recall when you encounter problems
(especially dementia) - man. Just write "man 'your command'"
and if man have something to say you, he will show you
the help instruction.

### A bit of luck

No one is ever privy to such details, but,
if the developer has sufficient knowledge in the field of UX,
then you can try your luck and simply write a command
without parameters. And if the front side of the coin shows an eagle,
it is even possible to see how to get help or maybe help itself.

![Ups, the image is somewhere](./images/basics/essential_linux_skills/help_itself.png)

### Uncle Google

You are desperate?
Want to find an answer?
Even more, you would like to find complete solution?

It's time to experience full power of internet,
we are going to use browser!

But before,... docker instalation!

- sudo apt install apt-transport-https ca-certificates curl software-properties-common
- curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
- sudo add-apt-repository "deb [arch=amd64]
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
- sudo apt update
- sudo apt install docker-ce

And now, installation and first run of perfect gui browser:
"sudo docker run -ti browsh/browsh https://youtube.com".

Here we are on the best site of tutorials!

![Ups, the image is somewhere](./images/basics/essential_linux_skills/youtube_agreement.png)

Mhm, youtube agreement, so..., I suppose
we just reject all of that. Aha, so i can't make any click
and tab also does not work...

Solutions to this problem are not plentiful:

1. Connect to the our machine from the another machine with cursor.
1. Use keyboard-driven web-browser, e.g. elinks.

Personally, I prefer the first one.
But because the connection to the vm
is a separate point, you know where to find guide.

The result of our work is a beautiful and sharp vision of the internet:

![Ups, the image is somewhere](./images/basics/essential_linux_skills/youtube_cli.png)

</details>

<details>
<summary>Services and processes</summary>

### Processes

Most commonly used commands for processes,
definitely are: ps, kill, nice, and taskset.

#### ***ps***

ps — show a snapshot of current list of processes
(especially their pid).

"ps a" — to list entire list of processes.

#### ***kill***

kill — send a signal to a process.

"kill -KILL \<PID>" — to kill the process
(similar to pkill).

"kill -STOP \<PID>" — to stop the process.

"kill -CONT \<PID>" — to continue the process.

#### ***nice***

nice — tool to change priority of process
(from -20 to 19)
(from max to min).

"nice --20 wget https://momcorp.com/playbot/hot-machines-without-secureboot.epub"
— this will execute this wget command (process) with the most high priority.

Or we can change nice index (usually in short - ni)
of the existant process — "sudo renice -n 5  -p 8721".

Some notes:

- nice without sudo can set max 0 as ni.
- without sudo renice can only change
priority of the process to the lower value.

#### ***taskset***

We can assign a specific process to a specific CPU. So..., let's try it, I guess?

"taskset -p \<PID>" — to show CPU affinity for the process.
E.g. return of the command 1f, that is equal to 00011111,
where the length of binary number if the number of the CPU,
and from the right to the left - attachment.
In this example process can be executed on:
CPU0, CPU1, CPU2, CPU3, CPU4.

To show the number of CPUs — "lscpu | grep ^CPU\\(s\\)".

"taskset -p 0x5 \<PID>" — assign CPU0, CPU2 to the process.

"taskset -c \<CPU list> \<PID>".

"taskset -c 0,2 \<PID>" — assign CPU0, CPU2 to the process

"taskset -c 0-2 \<PID>" — assign CPU0, CPU1, CPU2 to the process

### Services

Linux users should be aware of certain service operations, such as:

- Enable service — "systemctl enable \<Name of process>"
- Disable service — "systemctl disable \<Name of process>"
- Start — "systemctl start \<Name of process>"
- Stop — "systemctl stop \<Name of process>"
- Restart — "systemctl restart \<Name of process>"

</details>

<details>
<summary>Files and file systems</summary>

List of commands for this section: pwd, ls, cd, lsblk, mkfs.

#### ***pwd***

To show which directory you are currently in,
just type "pwd"

#### ***ls***

To show directory contents:

"ls" — shows all not hidden files and directories.

"ls -a" — shows all.

"ls -l" — like ls, but also shows size of the files,
their owners, permissions, last modification time.

#### ***cd***

To change your current possition — cd.

"cd \<Path to the destination>", path can be relative and global.

#### ***lsblk***

"lsblk -d" — show all drives.

"lsblk -d -o name,kname,fstype,size,type,rm,vendor,tran | grep -E 'usb|usb-c'" —
show only drives plugged by usb or usb-c.

#### ***mkfs***

"mkfs -t \<Filesystem type> \<Device name>" —
format the device with specific filesystem.

"mkfs.ext4 \<Device name>" —
format the device with specific ext4 filesystem.

</details>

<details>
<summary>Permissions</summary>

File permissions in Linux dictate who can access a file
and how they can interact with it. They are represented
by a three-character sequence, commonly referred to as the "rwx" mode.

1. Read (r): Grants the ability to read the contents of a file.
1. Write (w): Allows the user to modify or change the contents of a file or directory.
1. Execute (x): Enables a user to execute a file,
which typically means running a program or viewing the contents of a directory.

These permissions are applied to three categories of users:

1. Owner: The user who created or owns the file.
1. Group: The group to which the file belongs.
1. Others: All other users on the system.

"chmod \<options> \<permissions> \<file or directory>" — to change permissions.

"chmod 755 \<path to the file>" — change premissions to the file.

"chmod -R 777 \<path to the directory>" — change permissions to the directory
and its entire content.

</details>

<details>
<summary>Identity and Access Control</summary>

#### ***users***

Linux is a multi-user operating system,
meaning it can accommodate multiple users
with distinct identities and privileges.
Understanding the different user categories
and managing user accounts are essential aspects of Linux administration.

User Categories:

1. Root User: The ultimate administrative account with full control over the system.
1. System Users: Specialized accounts used by system services and applications.
1. Regular Users: Standard accounts granted to individuals for daily tasks.

Viewing All Users:

The "cat /etc/passwd" command displays a list of
all user accounts on the system. Each line contains information
about a single user, including their username, UID (user identifier),
GID (group identifier), home directory, and default shell.
To display only names, we can use: "awk -F':' '{ print $1}' /etc/passwd".

In the most cases, to see users, you can log in,
the command "getent passwd | awk -F: '$3 >= 1000 && $3 <= 60000 {print $1}'"
will work just fine.

Identifying Login Users:

The "who" command lists all users currently logged into the system.
Each line displays the username, terminal name, login time,
and remote host from which the user logged in.

Switching Users:

To switch between user accounts without logging out,
use the su command followed by the username you want to switch to.
For example, to switch to the user netpai, use:
"su - netpai".

#### ***groups***

Group Categories:

1. System Groups: Predefined groups used by system services and applications.
1. Primary Group: The default group to which a user belongs upon creation.
1. Secondary Groups: Additional groups a user can join for access control
and resource sharing.

Joining a Group:

To add a user to a group, use the "usermod" command followed by
the -g option for primary group or -G option for secondary groups:
"usermod \[-g|-G] \<group_name> \<username>"

Removing from a Group:

To remove a user from a group, use the "gpasswd" command followed by the -d option:
"gpasswd -d \<username> \<group_name>"

Group Types:

1. Closed Groups: Membership requires explicit addition by an administrator.
1. Open Groups: Users can join or leave freely.
1. Nested Groups: Groups can be members of other groups,
creating a hierarchical structure.

Listing Groups:
"cat /etc/group"

#### ***ownership***

Linux utilizes two primary ownership levels:

- File Owner: The individual user who created or
has been explicitly assigned ownership of the file or directory.
- File Group: The group to which the file or directory belongs.
Users within this group may have specific permissions for the file or directory.

Change ownership:

"chown [options] \<owner>:\<group> \<file or directory>"

"chown nerd:nerd .txt"

"chown -R nerd:nerd /home/nerd"

</details>

<details>
<summary>Metadata Management</summary>

#### ***size***

To check the size of a file or directory in Linux, you can use the
"du [options] \<file or directory>"

Options:

-h: Human-readable format (e.g., KB, MB, GB)

-s: Summarize the total size for each argument

Usage:

"du \<file>" — check the size of a file.

"du \<directory> — check the size of a directory.

"du -sh \<directory>" — check the size of a directory in a readable format.

"du -s \<directory>/*" — check the total size of all files in a directory.

"du -s \<directory>**" — check the size of all files in a directory and its subdirectories.

#### ***space***

"df -h" — memory usage for mounts.

"free - h" — ram usage.

du -sh $(find / -writable -user \<user_name>) — memory usage for the user

du -sh $(find / -writable -group \<group_name>) — memory usage for the group

#### ***date & time***

"date" — to show date.

"sudo date -s \<date>" — to set date.

</details>

<details>
<summary>File Interaction</summary>

#### ***read***

Go to the nano-vim section

#### ***search***

How original and no surprising at all, the command to search is called "find".

More precisely: "find \<path> \[options] \<criteria>"

"find \<path> -name "file"" — find files by name.

"find \<path> -type d" — find only directories

"find \<path> -size +1M" — find all files greater then 1Mb.

As criteria can be used regex.

"find . -iregex '\.\/[a-z]+.md'" - find all files in current
directory that end by .md and have only characters before.

#### ***copy***

"cp \[options] \<source> \<destination>"

"cp \<sFile> \<dFile>" — for files.

"cp -R \<sDirectory> \<dDirectory>" — for directories.

#### ***rename & replace***

To rename or replace you can use — "mv".

"mv [options] \<source> \<destination>"

"mv -p \<sFile> \<dFile>" — for files with preserving file attributes.

"mv -R \<sDirectory> \<dDirectory>" — for directories.

#### ***create***

Files:

"touch [options] \<list of names or pathes>" — for file creation.

"touch t1 t2 t3" — create 3 files with prefix "t" in the current directory.

Directories:

"mkdir [options] \<list of names or pathes>" — for directory creation.

"mkdir test" — create test directory in the current directory.

"mkdir -p ./test1/nested_test" — create nested_test directory in the current directory,
but also create all parent directories that does not exist.

#### ***info***

"file [options] \<file path or name>" — short information about file.

"file t1"

"stat [options] \<path or name>" — displays some useful information
about the object

"stat t2"

"stat test1"

#### ***delete***

"rm \[options] \<source>"

"rm \<sFile>" — for files.

"rm -R \<sDirectory>" — for directories.

</details>

</div>

</details>

<details>
<summary>System Administration</summary>

<div style="margin-left: 20px;">

<details>
<summary>Useful Linux system tools</summary>

#### ***top***

top — interective and more complex then ps manager of processes.

"top -u root" — show all processes attached to the root.

#### ***htop***

#### ***netstat***

#### ***Terminator***

#### ***tmux***

</details>

<details>
<summary>Console editors</summary>

#### ***vim***

Literally less complex version of neovim.

#### ***nano***

#### ***neovim***

</details>

<details>
<summary>sudo command</summary>

</details>

<details>
<summary>Users operations</summary>

#### ***creating users***

#### ***creating groups***

#### ***deleting users***

#### ***deleting groups***

#### ***managing users passwords***

</details>

<details>
<summary>Aliases</summary>

</details>

<details>
<summary>Package management</summary>

#### ***YUM***

#### ***RPM***

#### ***APT***

#### ***APT-GET***

#### ***DPKG***

#### ***DEB***

</details>

<details>
<summary>Compiling from source</summary>

</details>

<details>
<summary>Space management</summary>

</details>

<details>
<summary>Drives and partitions</summary>

</details>

<details>
<summary>Creating ext4 file system and permanently mounting (Tworzenie systemu plików ext4 i montowanie stałe)</summary>

</details>

<details>
<summary>Managing logical volumes</summary>

</details>

<details>
<summary>NFS service</summary>

#### ***server***

#### ***client***

#### ***fstab***

</details>

<details>
<summary>System monitoring</summary>

</details>

</div>

</details>

<details>
<summary>Networking</summary>

<div style="margin-left: 20px;">

<details>
<summary>Network configuration</summary>

</details>

<details>
<summary>SSH service</summary>

- [ ] client configuration
- [ ] server configuration
- [ ] tunneling
- [ ] SCP

</details>

<details>
<summary>Networks and firewalls</summary>

</details>

<details>
<summary>File Hosts and hostname</summary>

</details>

<details>
<summary>Configuring interfaces using nmtui</summary>

</details>

<details>
<summary>Firewall</summary>

</details>

<details>
<summary>Assigning multiple IP addresses to network interfaces</summary>

</details>

<details>
<summary>Monitoring traffic using tcpdump</summary>

</details>

</div>

</details>

<details>
<summary>Remote Access</summary>

<div style="margin-left: 20px;">

<details>
<summary>SSH</summary>

- [ ] putty
- [ ] mremoteNG
- [ ] MobaXtrem

</details>

<details>
<summary>VNC (optional)</summary>

</details>

</div>

</details>

## HeLp

<details>
<summary>HELP! | POMOCY! | ПАМАХИТЕ! | HILFE! | ヘルプ！ | ¡AYÚDAME MI AMIGO!</summary>

### HELP

If you are not familiar with the English.... hold on, why you even reading that then?

### POMOCY

Jeśli nie jesteś zaznajomiony z angielskim, cóż,
użyj [deepl], jest chyba wystarczająco dobry...?

### ПАМАХИТЕ

Они держат меня в этом межгалактическом подвале уже третий день
по каленадрю Юпитера, вызовите бригаду космического десанта [deepl],
они должны знать что делать в таких ситуациях, наверное ...?

### HILFE

Wenn Sie mit der englischen Sprache nicht vertraut sind, verwenden Sie [deepl],
das ist wahrscheinlich gut genug, nehme ich an...?

### ヘルプ

英語に馴染みがなければ、[deepl]を使えばいい。

### AYÚDAME MI AMIGO

¿Si no estás familiarizado con el inglés, bueno, utiliza [deepl],
supongo que te servirá...?

</details>

[deepl]: <https://www.deepl.com/>
[Ubuntu Server]: <https://ubuntu.com/download/server>
[pam_usb]: <https://github.com/mcdope/pam_usb>
