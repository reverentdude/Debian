## Steps to do after installing Debian


- #### Add your username to sudo group
    * Become root
        ```
        su - root
        ```
    * provide root password
    * Replace "dev" with your username in below commands & execute
        ```
        usermod -aG sudo dev
        ```
        * Check if your username is added  to sudo
        ```
        groups dev
        ```
        * you should see sudo in available groups of your username as shown below
            > dev : dev cdrom floppy sudo audio dip video plugdev users netdev bluetooth lpadmin scanner
    * exit root session
        ```
        exit
        ```

- #### Check for updates and install if any
    ```
    sudo apt update && sudo apt dist-upgrade
    ```

- #### Setup flatpak
    * Note: "gnome-software-plugin-flatpak" is only intended for Gnome DE, if you are not using Gnome ou can skip it
    ```
    sudo apt install flatpak gnome-software-plugin-flatpak
    ```
    ```
    sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
    ```

- #### Setup updated libreoffice
    * Uninstall all libreoffice related installed files from Software & execute below mentioned command to clean leftover stuff
        ```
        sudo apt remove libreoffice-common libreoffice-core libreoffice-gnome libreoffice-gtk3 libreoffice-help-common libreoffice-help-en-us libreoffice-style-colibre libreoffice-style-elementary
        ```
    * install libreoffice from software through flatpak

- #### Enable Non-Free Software and Firmware
    * Enable below mentioned options from  Software & Updates
        * Non-DFSG-compatible Firmware for Hardware Support (non-free-firmware)
        * Non-DFSG-compatible Software (non-free)

- #### Add multimedia codecs & Install VLC
    ```
    sudo apt install libavcodec-extra vlc
    ```

- #### Install Snaptic package manager
    ```
    sudo apt install synaptic
    ```

- #### Install Nvidia Driver
    ```
    sudo apt install nvidia-driver
    ```

- #### Add the "back ports" repository to install upcoming updates
    * Create a file named backports.list
        ```
        sudo vim /etc/apt/sources.list.d/backports.list
        ```
    * Add below mentioned content to backports.list by replacing "bookworm" with respective codenname
        > deb http://deb.debian.org/debian bookworm-backports main
    * Perform an update
        ```
        sudo apt -y update && sudo apt -y upgrade
        ```
    * Use below mentioned command format to install any package through backports by replacing "bookworm" and "package" with actual codename and actual package name
        ```
        sudo apt install -t bookworm-backports package
        ```

- #### Setup Qemu
    * Install required packages
        ```
        sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager -y
        ```

- #### Setup Docker
    * Install required packages
        ```
        sudo apt install lsb-release gnupg2 apt-transport-https ca-certificates curl software-properties-common -y
        ```
    * Import the GPG keys for the Docker repository
        ```
        curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/debian.gpg
        ```
    * Add the Docker stable repository
        ```
        sudo add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
        ```
    * Perform an update
        ```
        sudo apt -y update && sudo apt -y upgrade
        ```
    * Install Docker CE
        ```
        sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
        ```
    * Add your username to the Docker group to be able to run Docker commands without using sudo
        ```
        sudo usermod -aG docker $USER
        ```
    * log in to docker group
        ```
        newgrp docker
        ```
    * Enable docker service
        ```
        sudo systemctl enable docker
        ```
    * Start docker service
        ```
        sudo systemctl start docker
        ```
    * Check if the service is running
        ```
        systemctl status docker
        ```
    * Check the installed version
        ```
        docker version
        ```
        * Steps for uninstallation of Docker
            ```
            sudo apt remove docker-ce docker-ce-cli containerd.io
            ```
            ```
            sudo rm -rf /var/lib/docker
            ```
            ```
            sudo rm -rf /var/lib/containerd
            ```

- #### Install Nvidia Container Toolkit
    * Add requirements
        ```
        sudo bash -c "curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey >> /etc/apt/keyrings/nvidia-docker.key"
        ```
        ```
        sudo bash -c "curl -s -L https://nvidia.github.io/nvidia-docker/debian11/nvidia-docker.list > /etc/apt/sources.list.d/nvidia-docker.list"
        ```
        ```
        sudo sed -i -e "s/^deb/deb \[signed-by=\/etc\/apt\/keyrings\/nvidia-docker.key\]/g" /etc/apt/sources.list.d/nvidia-docker.list
        ```
    * Perform an update
        ```
        sudo apt -y update && sudo apt -y upgrade
        ```
    * Install toolkit
        ```
        sudo apt -y install nvidia-container-toolkit
        ```
    * Restart docker service
        ```
        sudo systemctl restart docker 
        ```
    * Try to pull cuda 12.1 image & run [nvidia-smi] from Containers.
        ```
        docker run --gpus all nvidia/cuda:12.1.1-runtime-ubuntu22.04 nvidia-smi
        ```

- #### Install required fonts
    * Activate the ‘contrib’ and ‘non-free’ repositories
        ````
        sudo apt-add-repository contrib non-free -y
        ````
        * If the above command doesn't work try it again after executing below mentioned command
            ```
            sudo apt install software-properties-common -y
            ```
    * Initiate the font installation
        ```
        sudo apt install fonts-ubuntu fonts-liberation2 ttf-mscorefonts-installer 
        ```
    * Create the directory to host the config file
        ```
        mkdir -p ~/.config/fontconfig/
        ```
    * Place the following configuration under ~/.config/fontconfig/fonts.conf
        ```
        sudo vim ~/.config/fontconfig/fonts.conf
        ```
        > 
          <?xml version="1.0"?>
            <!DOCTYPE fontconfig SYSTEM "fonts.dtd">
            <fontconfig>
            <alias>
                <family>serif</family>
                <prefer>
                <family>Liberation Serif</family>
                </prefer>
            </alias>
            <alias>
                <family>sans-serif</family>
                <prefer>
                <family>Ubuntu</family>
                </prefer>
            </alias>
            <alias>
                <family>monospace</family>
                <prefer>
                <family>Ubuntu Mono</family>
                </prefer>
            </alias>
            </fontconfig>
    * Go to your web browser settings and under font customization pick:
        * sans-serif (or sans) as default font
        * sans-serif (or sans) as sans-serif font
        * serif as serif font

- #### Setup git
    * Install git
        ```
        sudo apt install git
        ```
    * Create a .ssh directory
        ```
        mkdir $HOME/.ssh
        ```
    * generate a key by replacing user@gmail.com with your mail in below command
        ```
        ssh-keygen -t rsa -b 4096 -C user@gmail.com
        ```
    * evaluate it
        ```
        eval "$(ssh-agent -s)"
        ```
    * Get your key
        ```
        cat ~/.ssh/id_rsa.pub
        ```
    * After this, go to your GitHub profile, to the settings tab, option SSH and GPG Keys,New SSH Key, name it as you like and there paste your key for authentication, similarly add same key or a new key for signing
