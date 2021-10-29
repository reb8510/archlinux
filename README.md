## Doc for Arch Linux by Reagan Becnel
**Set the consule keyboard layout**
<p>The default consule is the US keyboard, so the layout should be...</p>
<pre><code> ls /usr/share/kbd/keymaps/**/*.map.gz </code></pre>

**Connect to the internet**
<p>Set up the internet connection.</p>
<pre><code> ip link </pre></code>
<p>Connect to the Wi-Fi.</p>
<pre><code>iwctl</pre></code>
<p>Verify the connection works.</p>
<pre><code>ping archlinux.org</pre></code>

**Check the System Clock**
<p>Add the correct time zone system clock.</p>
<pre><code>timedatectl set-ntp true</pre></code>

**Check for UEFI System**
<p>Check to see if you have a UEFI system.</p>
<pre><code>ls /sys/firmware/efi/efivars </pre></code>
<p>A directory exists, therefore I had a UEFI enabled system.</p>

**Partition the Disks**
<p>Show the hard discs attached to the system.</p>
<pre><code>fdisk -l</pre></code>
<p>Partition the first table.</p>
<pre><code>fdisk /dev/sda</pre></code>
<p>Once the table has been created, you will have created an EFI partition with 500M.</p>
<p>Partition the second table.</p>
<p>The table has been created, you will change the type of partition from 'Linux filesystem' to 'Linux LVM' by changing the type of partition to 30. You will give the table the rest of the space, which turns out to be 19.5GiB.</p>

**Create Filesystem for the UEFI System**
<p>Create a fat32 filesystem.</p>
<pre><code>mkfs.fat -F32 /dev/sda1</pre></code>
<p>Create a swap file system.</p>
<pre><code>mkswap /dev/sda2 </pre></code>
<p>Create and mount the Ext4 filesystem on the root partition.</p>
<pre><code>mkfs.ext4 /dev/sda3
mount /dev/sda3 /mnt</pre></code>

**Install Packages**

<p>Install the base package, Linux kernal, and fireware.</p>
<pre><code>pacstrap /mnt base linux linux-firmware</pre></code>

**Generate Filesystem table**
<pre><code>genfstab -U /mnt >> /mnt/etc/stab</pre></code>
Change root into the new system. 
<pre><code>arch-choroot /mnt</pre></code>

**Set the Local**
<p>Set and edit the locale.gen file.
<pre><code>nano /etc/locale.gen </pre></code>
 <p>Once in the file, uncomment the en_US.UTF-8 UTF-8.</p>

 **Network Configuration**
 <p>Create the hostname file.</p>
 <pre><code>/etc/hostname/archReagan</pre></code>
 <p>Set the root password</p>
 <pre><code>passwd</pre></code>
 <p>set local host in the /etc/host</p>
 <pre><code>Add: 127.0.0.1 localhost 
::1 localhost
127.0.1.1 archReagan.localdomain</pre></code>

**Desktop Environment**
<p>Create Codi and Sal as users.</p>
<pre><code>user add -m codi
passwd codi
user add -m sal
passwd sal</pre></code>
<p>Make temp password for Codi and Sal 'GraceHopper1906'</p>

<p>Install Sudo and Grub</p>
<pre><code>pacman -S sudo
pacman -S grub</pre></code>
<p>Install the EFI Boot Manager</p>
<pre><code>efibootmgr dosfstools os-prober motools</pre></code>
<p>Create directory in the EFI</p>
<pre><code>mkdir /boot/EFI</pre></code>
<p>Mount the sda</p>
<pre><code>mount /dev/sda1/boot/EFI</pre></code>
<p>Configure the GRUB</p>
<pre><code>grub-install --target=x86_64-efi --boot loader-id=grub_uefi --recheck</pre></code>
<p>Download the network manager</p>
<pre><code>pacman -S network manager</pre></code>

**Install Graphics Driver**
<p>Install xorg graphics server</p>
<pre><code>pacman -S xorg-server xorg-apps xorg-xinit</pre></code>
<p>Install graphics driver</p>
<pre><code>pacman -S xf86-video-vesa</pre></code>
<p>Install display manager</p>
<pre><code>pacman -S sddm</pre></code>
<p>Install KDE desktop environment</p>
<pre><code>pacman -S plasma kde-applications</pre></code>
<p>configure graphical boot</p>
<pre><code>systemctl enable sddm</pre></code>
<p>configure network manager</p>
<pre><code>pacman -S networkmanager
systemctl enable NetworkManager</pre></code>

**Exit**
<p>after you should exit and then reboot and have your desktop environment.</p>
