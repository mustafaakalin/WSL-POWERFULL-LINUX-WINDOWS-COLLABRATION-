# WSL “POWERFULL LINUX , WINDOWS COLLABRATION”

# WSL2

you can linux command in windows : example : `wsl cat /etc/passwd`

you can windows command in linux : exmple `explorer .`

## SETUP/INITIALIZE LINUX DISTROS

distro options command

```
wsl.exe --list -o
```

example output:

```
The following is a list of valid distributions that can be installed.
Install using 'wsl.exe --install <Distro>'.

NAME                            FRIENDLY NAME
AlmaLinux-8                     AlmaLinux OS 8
AlmaLinux-9                     AlmaLinux OS 9
```

## DEBIAN

### UBUNTU

```
sudo apt update \
apt list --upgradable \
sudo apt full-upgrade -y
```

## ARCH

note: `always believe official docs , resources, sources etc.`

1. official arch wiki url : https://wiki.archlinux.org/title/Install_Arch_Linux_on_WSL
2. https://gitlab.archlinux.org/archlinux/archlinux-wsl
3.  [MANUAL install] https://gitlab.archlinux.org/archlinux/archlinux-wsl/-/releases/permalink/latest
4.  [MANUAL install]  download only .wsl extension file exmpl: `archlinux-2025.03.04.121374.wsl`
5. final : Download the [latest Arch Linux ".wsl" image](https://gitlab.archlinux.org/archlinux/archlinux-wsl/-/releases/permalink/latest) and double-click on it to start the installation.

after run update/upgrade

```bash
pacman -Syu
```

how to run command:

```powershell
wsl.exe -d archlinux
```

1. create user https://wiki.archlinux.org/title/Users_and_groups#Example_adding_a_user exmpl: greenwarrior

```bash
useradd -m greenwarrior
```

1.  [OPTIONAL] ADD ADDITIONAL REPOS (blackarch, and garuda [chaotic.cx](http://chaotic.cx) [aur](https://aur.archlinux.org/))

**blackarch** https://www.blackarch.org/downloads.html 

REMINDER: “Verify the SHA1 sum” section is another version of [strap.sh](https://blackarch.org/strap.sh) file if changed, changes: you can comment line with first `#` (bash shell comment char : #)

```bash
# Run https://blackarch.org/strap.sh as root and follow the instructions.
curl -O https://blackarch.org/strap.sh

# Verify the SHA1 sum
echo bbf0a0b838aed0ec05fff2d375dd17591cbdf8aa strap.sh | sha1sum -c

# Set execute bit
chmod +x strap.sh

# Run strap.sh
./strap.sh

# Enable multilib following https://wiki.archlinux.org/index.php/Official_repositories#Enabling_multilib and run:
pacman -Syu
```

[**chaotic](https://aur.chaotic.cx/about) aur** 

- Chaotic-AUR is a repository that provides a large number of packages for Arch Linux.
- The repository is maintained by a team of volunteers who work to provide pre-compiled packages for their users.

```bash
pacman-key --recv-key 3056513887B78AEB --keyserver keyserver.ubuntu.com
pacman-key --lsign-key 3056513887B78AEB
```

```bash
pacman -U 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-keyring.pkg.tar.zst'
pacman -U 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-mirrorlist.pkg.tar.zst'
```

```bash
echo "[chaotic-aur]
Include = /etc/pacman.d/chaotic-mirrorlist" >> /etc/pacman.conf
```

```bash
pacman -Syu
```

- you can optional install [**firedragon**](https://firedragon.garudalinux.org/team) (i suggest for privacy first browser work, firefox fork project, developed by [garudalinux team](https://firedragon.garudalinux.org/team))

```bash
pacman -S firedragon openjdk-src
```

1. SET ROOT PASSWORD / SET USER (greeenwarrior) PASSWORD

for root user(in root user shell, **open new archlinux terminal on windows terminal**!!!):

```bash
passwd root
```

for created user (greenwarrior) (in root shell or greenwarrior shell chsh -l , chsh /usr/bin/bash):

```bash
passwd greenwarior # $USER var usable in created user shell! exmp: passwd $USER
```

1. SET DEFAULT LOGIN USER CREATED USER (greenwarrior) (for more secure, and no damage system maybe😀 maybe)

```bash
echo "[user]
default=greenwarrior" >> /etc/wsl.conf
```

**INSTALL SUDO (for [SUDO](https://wiki.archlinux.org/title/Sudo)  , other required packages )**

```bash
pacman -S sudo debugedit fakeroot --noconfirm # required for yay : debugedit fakeroot
```

ADD SUDOERS GROUP CREATED USER command (greenwarrior)

```bash
sed -i '/^root ALL=(ALL:ALL) ALL/a bluework ALL=(ALL:ALL) ALL' /etc/sudoers
```

OPEN NEW **WINDOWS TERMINAL** TAB

- The change will apply at the next session. To terminate your current session, run the following command in a PowerShell prompt:

```bash
wsl --terminate archlinux
```

OPEN NEW archlinux TAB

- list of installed shell’s

```bash
chsh -l
```

```bash
sudo chsh bluework -s /usr/bin/fish
```

 [OPTIONAL:FISHSHELL] bonus part/ OPTIONAL 

install fish shell for easy, little(because not mirrored bash, bash is the so popular shell, some sh,bash script file not correctly work, this is little problem, change shell and fix it, example: `bash` command use!!) good shell :DD

1. `su -m greenwarrior` (if on root shell !)

```bash
sudo pacman -S --needed --noconfirm git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

# Paket yöneticisi güncelleme
sudo pacman -Syu --noconfirm

# Gerekli paketlerin yüklenmesi
sudo pacman -S --needed --noconfirm \
    yay fish bat less qtile starship find-the-command eza ugrep \
    hwinfo reflector fastfetch expac meld nmap #nc

# YAY
yay -S --noconfirm paru find-the-command
```

1. go https://gitlab.com/garuda-linux/pkgbuilds/-/blob/main/garuda-fish-config/config.fish?ref_type=heads
2. `Copy file contents` button , right top “Copied Content”
3. add your `~/greenwarrior/.config/fish/config.fish` fish config file , garuda fish shell config, 

shortcut for add (remove file , and create file on bash shell)

**remove , and curl**

```bash
rm -f /home/greenwarrior/.config/fish/config.fish && curl  -o "/home/greenwarrior/.config/fish/config.fish" https://gitlab.com/garuda-linux/pkgbuilds/-/raw/main/garuda-fish-config/config.fish
```

[OPTIONAL:[LUNARVIM](https://www.lunarvim.org/docs/community)] [LUNARVIM](https://www.lunarvim.org/docs/installation) INSTALL

```bash
#editor setup
sudo pacman -S vim neovim nano git make python-pip python npm nodejs cargo ripgrep lazygit
```

```bash
# fish shell syntax
bash -c "bash <(curl -s https://raw.githubusercontent.com/lunarvim/lunarvim/master/utils/installer/install.sh)"
```

[OPTIONAL:NERDFONT] [NERD FONT](https://www.nerdfonts.com/font-downloads) [INSTALL](https://github.com/getnf/getnf)

- https://github.com/getnf/getnf

```bash
sudo pacman -S fzf --noconfirm
curl -fsSL https://raw.githubusercontent.com/getnf/getnf/main/install.sh | bash
getnf -f # JetBrainsMono, Hack, Firacode good font 
```

**[OPTIONAL:DEVELOPER]** dev tools **`pacman`** , **`yay`**

```bash
sudo pacman -S git github-cli 
#yay -S visual-studio-code-insiders-bin
#❯ code-insiders
#To use Visual Studio Code - Insiders with the Windows Subsystem for Linux, please install Visual Studio Code - Insiders in Windows and uninstall the Linux version in WSL. You can then use the `code-insiders` command in a WSL terminal just as you would in a normal command prompt.
#Do you want to continue anyway? [y/N] 
```

- **WIN TERMINAL `winget`**

```bash
winget install Microsoft.VisualStudioCode.Insiders 
# https://code.visualstudio.com/docs/remote/wsl
# wsl addon : https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl
wsl --set-default archlinux
```

**[OPTIONAL:DE] HYPRLAND # Desktop Environment**

```bash
yay -S hyprland-git hyprland-meta-git
# HYPRLAND install from the AUR
# install the hyprland-meta package to automatically fetch and compile the latest git versions of all components within the hypr* ecosystem.
#error: failed to commit transaction (failed to retrieve some files)
#Errors occurred, no packages were upgraded.
# -> error installing repo packages
#error installing repo packages
#if http 400, http 404 error code
#cat /etc/pacman.d/mirrorlist
#cat /etc/pacman.d/chaotic-mirrorlist
# OPTIONS COUNTRIES: Brazil, Bulgaria, Canada, Chile, Germany, France, Greece, India, Korea, Spain, United States 
# sudo reflector --country "United States" --latest 15 --age 2 --fastest 20 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
# recommended hypr config : https://github.com/JaKooLit/Arch-Hyprland
```

**[OPTIONAL:GUI_LINUX] KDE PLASMA DE # Desktop Environment**

- META PACKAGES

```bash
sudo pacman -S plasma kde-graphics-meta kde-applications-meta kde-education-meta kde-cli-tools kde-multimedia-meta kde-network-meta kde-office-meta kde-pim-meta kde-sdk-meta kde-system-meta kde-utilities-meta kde-gtk-config
#Total Download Size:   1139.12 MiB
#Total Installed Size:  3724.72 MiB
```

**[OPTIONAL:KEYBOARD MAP] , LOCALE**

```bash
###############!!!!!!!!!!!!IMPORTANT uncomment required languages
#vim /etc/locale.gen
sudo vim /etc/locale.gen
sudo locale-gen # update

############### Set the Keyboard Layout perm
#localectl list-keymaps | grep tr
sudo localectl set-keymap trq # if another lang :use <TAB>
localectl status
############### Set Keyboard Layout temporarely Export Locale Variables (Temporary)
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
############### Set the System Locale
sudo nano /etc/locale.conf
#    Add the following line:
LANG=en_US.UTF-8
##################### Generate the Required Locales
# sudo nano /etc/locale.gen
en_US.UTF-8 UTF-8
tr_TR.UTF-8 UTF-8
sudo locale-gen
```

**[OPTIONAL:GUI_LINUX] WSL D.E. CONNECT ON THE WINDOWS**

- **TIGERVNC SERVER**
- https://github.com/TigerVNC/tigervnc

```bash
sudo pacman -S tigervnc wayland # tigervnc inside built-in vncviewer

#❯ vnc<TAB>
#vncconfig          (command)  vncserver         (command)  vncviewer  (command)
#vncpasswd          (command)  vncsession        (command)
#vncpcap2john  (command link)  vncsession-start  (command)

sudo pacman -S xorg-xkbcomp xorgproto xkeyboard-config
sudo usermod -aG video $USER # for GPU access on guest 
# then, logout and log back in.
vncpasswd
#Password should not be greater than 8 characters
#Because only 8 valid characters are used - try again
#Password:
## example pw: ^W77abH
mkdir -p ~/.vnc
         nano ~/.vnc/xstartup
# add to xstartup file inside:
#!/bin/bash
        unset SESSION_MANAGER
        unset DBUS_SESSION_BUS_ADDRESS
        [ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
        [ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
        xsetroot -solid grey
        vncconfig1 -iconic &
        #x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
       #x-window-manager &
        startplasma-x11 & #For KDE Plasma
        #startxfce4 & #For XFCE
        #gnome-session & #For Gnome
       
        
chmod +x ~/.vnc/xstartup        
        
############# sudo pacman -S twm # or icewm
############# sudo pacman -S gnome-session        
# journalctl -xe        
# vncserver
vncserver :1
# VNC authentication enabled, but no password file created.
# startplasma-wayland # dont recommended , use weston , wayvnc for wayland compositor

# RFB, Remote Firewall Block , close win defender firewall : 
# WAY : systemTray Windefender>Firewall&network>Advanced settings>Properties
# edit , recommend "Private profile" use
# Windows + I
# Go to Network & Internet
# -Click on "Network & Internet" and select either "Wi-Fi" or "Ethernet," depending on your connection.
# Change Network Profile: 
# -    Under your active network, look for "Network profile" and select Private. This will set your network to private, enabling the Private Profile in Windows Firewall.
# Verify in Firewall Settings: 
#     Open the Control Panel, go to "Windows Defender Firewall," and ensure the Private Profile is active under "Firewall & network protection."

# restart : open admin terminal : net stop mpssvc & net start mpssvc : pwsh: Restart-Service -Name mpssvc -Force
# close "Microsoft Defender Firewall"
# vEthernet (WSL (Hyper-V firewall)) # DISABLE  

## CONNECT
ip a show ## UP UPUPUPUPUP

## "Remote Desktop Connection" open
# Computer : 192.168.xx.x2x:5901 #

## DOWNLOAD TIGERVNC ON HOST
# tigervnc-1.15.0\tigervnc-1.15.0\win\winvnc
	# winvnc4.exe.manifest64 # REMOVE '.manifest64' and RUN

```

RDP “Remote Desktop Connect” in windows built-in vncviewer program

- win key + rdp <ENTER>

> Hint: You are currently not seeing messages from other users and the system.
      Users in groups 'adm', 'systemd-journal', 'wheel' can see all messages.
      Pass -q to turn off this notice.
> 

```bash
sudo usermod -aG adm,systemd-journal,wheel blue # -aG append group
```

LINUX:

```bash
ip a show
```

WINDOWS:

```bash
ipconfig
```

[OPTIONAL:PACKAGES] USEFULL PACKAGES

```bash
# archlinux-java status 
# sudo archlinux-java set <java-version>
sudo pacman -S openjdk-src tree --no-confirm 
```

[OPTIONAL:EXPO-REACT-NATIVE] EXPO REACT-NATIVE TOOLS, ANDROID STUDIO

```bash
sudo pacman -S nodejs npm pnpm yarn 
yay -S android-studio # chaotic-aur/android-studio
sudo npm install -g eas-cli
```

[OPTIONAL:FOLDER_STRACTURES] SETUP FOR DEV AND SEC RESEARCHER

```bash
############### DEV (BASH)
su username # not root user!!! only gid(1000) user for damage preventeation , you know
mkdir -p $HOME/Documents/projects
mkdir -p $HOME/Documents/projects/javascript/expo-react-native
mkdir -p $HOME/Documents/projects/javascript/expo-react-native

```
