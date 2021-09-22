# thinkorswim™ Linux Container

**Disclaimer**: *thinkorswim* is a proprietary trading platform created and operated by TD Ameritrade.

This repository contains scripts to create a container environment that the *thinkorswim* product can run inside safely. It extracts a Zulu OpenJDK image to a local directory and sets it up a container environment with the extracted image as its root. A desktop file is installed so that it will appear in the application list for the desktop environment.

## Requirements

- Linux 🐧
- [buildkit](https://github.com/moby/buildkit)
- [ansible](https://github.com/ansible/ansible)
- [bubblewrap](https://github.com/containers/bubblewrap)

## Procedure

1. start **buildkitd**
   
   ```bash
   rootlesskit --net=slirp4netns --copy-up=/etc --disable-host-loopback buildkitd
   ```

2. run the **ansible** playbook
   
   - take a look at `ansible/playbook.yaml` to see what it is going to do
   - pay special attention to the `vars` section
   - the playbook will pause at a certain point to wait for installation to complete
     - **this is a manual action**
     - proceed to step 3 and execute that command
   - set your local time zone by adding `-e timezone=TIMEZONE` to the invocation (defaults to UTC)
   
   ```bash
   ansible-playbook ansible/playbook.yaml -e timezone=America/Chicago
   ```

3. run the *thinkorswim* **installer**
   
   - the `/home/thinkorswim/...` path in _inside_ the container, do not change it
   - if everything is properly configured, an installer window will pop up
   - choose the option to install for the **current user**, not system-wide
   - continue the ansible playbook once installation is complete
   
   ```bash
   ${HOME}/.local/share/thinkorswim/container.sh /home/thinkorswim/thinkorswim_installer.sh
   ```

4. (optional) stop **buildkitd**
   
   - if you manually started it before

5. launch ***thinkorswim***
   
   - at this point it should be available in your applications list in you desktop environment
   
   - to launch from the terminal, you can run the following
     
     ```bash
     gtk-launch thinkorswim
     ```
