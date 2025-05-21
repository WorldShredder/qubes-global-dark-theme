# Qubes Global Dark Theme

This repository is dedicated to the configuration of a system-wide _dark-theme_ in [Qubes OS↗](https://qubes-os.org).

#### Comprehension
<details>
<summary><b>What is <code>/etc/skel</code></b></summary>

> `/etc/skel` is a directory which acts as a _skeleton_ home directories, e.g., `/home/user`, and is used by `adduser` to generate home directories for new users. That said, modifications to `/etc/skel` do not affect the home directories of existing users.

</details>



## Dom0 Configuration

_todo..._



## Debian 12 Configuration

### TemplateVM Config

> [!NOTE]
> Applying this configuration in a _TemplateVM_ will in-turn apply the configuration to _new_ _AppVMs_ based on the template. However, existing _AppVMs_ based on the template will **not** be affected; see [AppVM Config](#appvm-config) below.

> [!NOTE]
> The commands below are written for execution by user `user`. If, however, you are in a _minimal_ template with an unprivileged `user` account, remove all mentions of `sudo` in each command and execute as user `root`.

1. [**user**@**TemplateVM**]() Configure dark-theme for _GTK_ applications

    1. Create `gtk-3.0` directory in user skeleton

        ```bash
        sudo mkdir -p /etc/skel/.config/gtk-3.0
        ```

    2. Add dark-theme _CSS_ config `gtk-3.0`

        ```bash
        echo '@import url("resource:///org/gtk/libgtk/theme/Adwaita/gtk-contained-dark.css");' | sudo tee -a /etc/skel/.config/gtk-3.0/gtk.css &>/dev/null
        ```

2. [**user**@**TemplateVM**]() Configure dark-theme for _QT_ applications

    1. Install `qt5ct` package

        ```bash
        sudo apt install qt5ct
        ```

    2. Create `environment.d` drop-in directory

        ```bash
        sudo mkdir /etc/environment.d
        ```

    3. Create environment config which points _QT Platform Theme_ to `qt5ct`

        ```bash
        echo 'QT_QPA_PLATFORMTHEME=qt5ct' | sudo tee -a /etc/environment.d/100qt5ct-dark-theme.conf &>/dev/null
        ```

    4. Create `qt5ct` config directory in user skeleton

        ```bash
        sudo mkdir -p /etc/skel/.config/qt5ct
        ```

    5. Apply dark-theme to `qt5ct` configuration

        <details>
        <summary><b>Via Terminal</b></summary>

        1. Create `qt5ct` config in user skeleton

            ```bash
            cat << 'EOF' | sudo tee /etc/skel/.config/qt5ct/qt5ct.conf &>/dev/null
            [Appearance]
            color_scheme_path=/usr/share/qt5ct/colors/darker.conf
            custom_palette=true
            standard_dialogs=default
            style=Fusion
            EOF
            ```

        </details>

        <details>
        <summary><b>Via GUI</b></summary>

        1. Launch `qt5ct`

            ```bash
            setsid qt5ct &>/dev/null
            ```

        2. Set _QT_ color-scheme to `darker`

            _`Appearance` → `Palette` → `Custom` → `Color scheme` → `darker`_

        3. Click `OK`

        5. Copy `qt5ct` config to user skeleton

            ```bash
            sudo cp /home/user/.config/qt5ct/qt5ct.conf /etc/skel/.config/qt5ct/
            ```

        </details>

3. [**user**@**TemplateVM**]() Shutdown VM to apply changes

    ```bash
    sudo poweroff
    ```


### AppVM Config

_todo..._



## Fedora X Configuration

### TemplateVM Config

_todo..._

### AppVM Config

_todo..._
