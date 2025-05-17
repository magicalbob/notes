# Linux Package Managers

## dnf (Fedora, RHEL, CentOS Stream)

    list all installed packages:
    
        dnf list installed
    
    search for a package:
    
        dnf search package_name
    
    install a package:
    
        sudo dnf install package_name
    
    remove a package:
    
        sudo dnf remove package_name
    
    list available updates:
    
        dnf check-update
    
    update all packages:
    
        sudo dnf update
    
    list security updates:
    
        sudo dnf updateinfo list security
    
    apply security updates:
    
        sudo dnf update --security -y
    
    clean cache:
    
        sudo dnf clean all
    
    view package information:
    
        dnf info package_name
    
    list package dependencies:
    
        dnf repoquery --requires package_name
    
    list package groups:
    
        dnf group list

## yum (CentOS, older RHEL)

    list all installed packages:
    
        yum list installed
    
    search for a package:
    
        yum search package_name
    
    install a package:
    
        sudo yum install package_name
    
    remove a package:
    
        sudo yum remove package_name
    
    list available updates:
    
        yum check-update
    
    update all packages:
    
        sudo yum update
    
    list security updates:
    
        sudo yum updateinfo list security
    
    apply security updates:
    
        sudo yum update --security -y
    
    clean cache:
    
        sudo yum clean all
    
    enable/disable repositories:
    
        sudo yum-config-manager --enable repo_name
        sudo yum-config-manager --disable repo_name

## apt/apt-get (Debian, Ubuntu)

    update package lists:
    
        sudo apt update
    
    list available upgrades:
    
        apt list --upgradable
    
    upgrade all packages:
    
        sudo apt upgrade
    
    install a package:
    
        sudo apt install package_name
    
    remove a package:
    
        sudo apt remove package_name
    
    completely remove a package and its configuration:
    
        sudo apt purge package_name
    
    search for a package:
    
        apt search package_name
    
    show package information:
    
        apt show package_name
    
    clean cache:
    
        sudo apt clean
        sudo apt autoclean
    
    remove unused dependencies:
    
        sudo apt autoremove
    
    add a repository:
    
        sudo add-apt-repository ppa:repository_name

## apk (Alpine Linux)

    update package lists:
    
        apk update
    
    install a package:
    
        apk add package_name
    
    remove a package:
    
        apk del package_name
    
    search for a package:
    
        apk search package_name
    
    list installed packages:
    
        apk list --installed
    
    upgrade all packages:
    
        apk upgrade
    
    clean cache:
    
        apk cache clean
    
    add repository:
    
        echo "http://repository_url" >> /etc/apk/repositories
        apk update

## pacman (Arch Linux)

    update package database:
    
        sudo pacman -Sy
    
    install a package:
    
        sudo pacman -S package_name
    
    remove a package:
    
        sudo pacman -R package_name
    
    remove a package and its dependencies:
    
        sudo pacman -Rs package_name
    
    update all packages:
    
        sudo pacman -Syu
    
    search for a package:
    
        pacman -Ss package_name
    
    list installed packages:
    
        pacman -Q
    
    clean package cache:
    
        sudo pacman -Sc

## brew (macOS)

    update brew:
    
        brew update
    
    install a package:
    
        brew install package_name
    
    uninstall a package:
    
        brew uninstall package_name
    
    search packages:
    
        brew search package_name
    
    list installed packages:
    
        brew list
    
    upgrade all packages:
    
        brew upgrade
    
    get information about a package:
    
        brew info package_name
    
    clean up old versions:
    
        brew cleanup
