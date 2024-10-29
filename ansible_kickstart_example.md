


# Playbook Overview

## The playbook handles:
* Basic system configuration (SELinux, firewall, timezone, language)
* Package installation
* Service management
* Banner setup
* Symbolic links creation
* UID/GID allocation policy

## Notable differences from the kickstart file:
* Disk partitioning is not included (this would typically be handled during initial installation)
* Some of the more complex custom scripts (`zaiwen-auto-rh6cln_secure*.sh`) would need to be converted separately
* Package groups are simplified (you may want to expand these based on your needs)

## Things you might want to add:
* Disk partitioning tasks if needed post-installation
* More detailed package group installations
* Additional security configurations
* Network configuration tasks
* Custom script content converted to Ansible tasks







```


---
- name: Configure RHEL 9.3 System
  hosts: all
  become: true
  vars:
    timezone: America/New_York
    root_password: "$6$O6gswNOGOFu7FSDa$ApOOS0o0oEatWkr6jPu6X.2MIc.0bC/im4GrfeIiYbhvRXZQZJjyo.Z1ihSkQpF//CozMi9xJDQgBD5HMqdph0"
    min_disk_size_gb: 80

  tasks:
    - name: Set root password
      user:
        name: root
        password: "{{ root_password }}"
        
    - name: Configure SELinux
      selinux:
        state: permissive

    - name: Disable firewall
      service:
        name: firewalld
        state: stopped
        enabled: false

    - name: Set system timezone
      timezone:
        name: "{{ timezone }}"

    - name: Configure keyboard layout
      command: localectl set-keymap us

    - name: Set system language
      command: localectl set-locale LANG=en_US.UTF-8

    - name: Configure bootloader
      lineinfile:
        path: /etc/default/grub
        regexp: '^GRUB_CMDLINE_LINUX='
        line: 'GRUB_CMDLINE_LINUX="rhgb quiet crashkernel=1G-4G:192M,4G-64G:256M,64G-:512M"'
      notify: Update grub

    - name: Install required packages
      dnf:
        name:
          - "@workstation-product-environment"
          - "@backup-client"
          - "@base-x"
          - "@development"
          - "@fonts"
          - "@gnome-desktop"
          - "@network-server"
          - "@office-suite"
          - "@scientific"
          - "@virtualization-client"
          - "@web-server"
          - chrony
          - emacs-nox
          - vim-X11
          - wget
          - zsh
          - tcsh
          - ksh
        state: present

    - name: Configure services
      service:
        name: "{{ item.name }}"
        state: "{{ item.state }}"
        enabled: "{{ item.enabled }}"
      loop:
        - { name: 'avahi-daemon', state: 'stopped', enabled: false }
        - { name: 'abrt-ccpp', state: 'stopped', enabled: false }
        - { name: 'nfs-lock', state: 'started', enabled: true }
        - { name: 'autofs', state: 'started', enabled: true }
        - { name: 'NetworkManager', state: 'started', enabled: true }
        - { name: 'cockpit.socket', state: 'started', enabled: true }
        - { name: 'fapolicyd', state: 'started', enabled: true }
        - { name: 'postfix', state: 'started', enabled: true }

    - name: Create BNL banner
      copy:
        dest: /etc/motd
        content: |
          NOTICE TO USERS
          
          This is a Federal computer system (and/or it is directly
          connected to a BNL local network system) and is the property
          of the United States Government. It is for authorized use
          only. Users (authorized or unauthorized) have no explicit
          or implicit expectation of privacy.
          
          Any or all uses of this system and all files on this system
          may be intercepted, monitored, recorded, copied, audited,
          inspected, and disclosed to authorized site, Department of
          Energy, and law enforcement personnel, as well as authorized
          officials of other agencies, both domestic and foreign.

    - name: Create symbolic links
      file:
        src: /cfs/ad
        dest: "{{ item }}"
        state: link
      loop:
        - /home/cfsad
        - /home/cfsd
        - /home/cfsa

    - name: Modify login.defs for UID/GID allocation
      lineinfile:
        path: /etc/login.defs
        regexp: "^{{ item }}.*"
        line: "{{ item }} 499"
      loop:
        - "SYS_UID_MAX"
        - "SYS_GID_MAX"

  handlers:
    - name: Update grub
      command: grub2-mkconfig -o /boot/grub2/grub.cfg

```
