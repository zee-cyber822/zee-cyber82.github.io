Name: Fouz Alfouzan 
Class: system administra6ons 
Date: November 2nd 2025 
1) host, VM, and OS facts 
the VM is arch Linux ARM running in VM Ware and the system clock is synced 
The commands I ran 
Hostnamectl 
timedatectl status 
The notes I observed 
-	Virtualization: VMware 
-	OS: Arch Linux ARM 
-	Clock synchronized: yes (NTP active) 
  
2) Desktop, display manager, and network 
I installed LXQT + SDDM + networkmanager, enable services and boot to a GUI login. 
Packages I installed: 
sudo pacman -S --needed --noconfirm \  xorg-server lxqt openbox sddm networkmanager xdg-user-dirs \  openssh zsh zsh-comple6ons zsh-autosugges6ons zsh-syntax-highligh6ng op6onal tools i installed: 
sudo pacman -S nano git htop zip unzip curl wget Enabled services and set to graphical target: sudo systemctl enable --now sddm NetworkManager sshd sudo systemctl set-default graphical.target Status checks: systemctl status sddm --no-pager systemctl status NetworkManager --no-pager systemctl is-enabled sddm NetworkManager sshd systemctl get-default ip -br a
  
 
 
3) User accounts, shell, and sudo 
I gave the wheel group sudo rights and uncommented the wheel line: 
sudo sed -i 's/^#\s*%wheel ALL=(ALL:ALL) ALL/%wheel ALL=(ALL:ALL) ALL/' /etc/sudoers and checked manually just to make sure and And then added users 2 wheel and set default shell to ZSH: 
sudo useradd -m -G wheel -s /usr/bin/zsh fouz sudo useradd -m -G wheel -s /usr/bin/zsh codi sudo passwd fouz sudo passwd codi 
And when I thought they already existed I just changed the shell: 
sudo chsh -s /usr/bin/zsh fouz sudo chsh -s /usr/bin/zsh codi 
And then I verified both of Fouz and Codi: 
whoami echo $SHELL groups sudo -l 
 
 
4) ZSH configura6on quality of life Here is my commands that I used history & comple6on: 
autoload -Uz compinit && compinit plugins (installed from pacman): 
source /usr/share/zsh/plugins/zsh-autosugges6ons/zsh-autosugges6ons.zsh source /usr/share/zsh/plugins/zsh-syntax-highligh6ng/zsh-syntax-highligh6ng.zsh aliases: 
alias ll='ls -alF' alias la='ls -A' alias ls='ls --color=auto' alias grep='grep --color=auto' alias update='sudo pacman -Syu' alias sctl='sudo systemctl' simple prompt: 
PROMPT='%F{cyan}%n@%m%f %F{yellow}%~%f %# ' 
And then I copied the same configura6on to Codi that was ran as Zee sudo install -o codi -g codi -m 644 ~/.zshrc ~codi/.zshrc 
  
5) Reboot and login 
I rebooted and logged out to the SDDM greeter and logged in with Fouz and also tested Codi 
  
6) Issues I ran into visudo: specified editor (vi) doesn’t exist  so i ran sudo EDITOR=nano visudo to open with nano network not showing an IP: 
Verified NetworkManager was enabled/running and the VM network was NAT Commands: systemctl status NetworkManager, ip -br a 
