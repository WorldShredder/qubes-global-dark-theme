# Qubes Global Dark Theme

This repository is dedicated to the configuration of a system-wide _dark-theme_ in [Qubes OS↗](https://qubes-os.org).

#### Comprehension
<details>
<summary><b>What is <code>/etc/skel</code></b></summary>

> `/etc/skel` is a directory which acts as a _skeleton_ for user home directories, e.g., `/home/user`, and is used by `adduser` to generate home directories for new users. That said, modifications to `/etc/skel` do not affect the home directories of existing users.

</details>



## Dom0 Configuration

_todo..._



## Debian 12 Configuration

### TemplateVM Config

> [!NOTE]
> Applying this configuration in a _TemplateVM_ will in-turn apply the configuration to **new** _AppVMs_ based on the template. However, **existing** _AppVMs_ based on the template will _not_ be affected; see [AppVM Config](#appvm-config) below.

> [!NOTE]
> The commands below are written for execution by user `user`. If, however, you are in a _minimal_ template with an unprivileged `user` account _(e.g., debian-12-minimal)_, remove all mentions of `sudo` in each command and execute as user `root`.

1. [**user**@**TemplateVM**]() Configure dark-theme for _GTK_ applications <a name="deb-tvm-1"></a>

    1. Create `gtk-3.0` directory in user skeleton

        ```bash
        sudo mkdir -p /etc/skel/.config/gtk-3.0
        ```

    2. Create dark-theme _CSS_ config `gtk.css`

        ```bash
        echo '@import url("resource:///org/gtk/libgtk/theme/Adwaita/gtk-contained-dark.css");' | sudo tee -a /etc/skel/.config/gtk-3.0/gtk.css &>/dev/null
        ```

2. [**user**@**TemplateVM**]() Configure dark-theme for _QT_ applications <a name="deb-tvm-2"></a>

    1. Install `qt5ct` package <a name="deb-tvm-2-1"></a>

        ```bash
        sudo apt install -y --no-install-recommends qt5ct
        ```

    2. Create `environment.d` drop-in directory <a name="deb-tvm-2-2"></a>

        ```bash
        sudo mkdir -p /etc/environment.d
        ```

    3. Create environment config which points _QT Platform Theme_ to `qt5ct` <a name="deb-tvm-2-3"></a>

        ```bash
        cat << 'EOF' | sudo tee -a /etc/environment.d/100qt5ct-dark-theme.conf &>/dev/null
        QT_QPA_PLATFORMTHEME=qt5ct
        QT_SCALE_FACTOR=1
        EOF
        ```

    4. Create `qt5ct` config directory in user skeleton

        ```bash
        sudo mkdir -p /etc/skel/.config/qt5ct
        ```

    5. Apply dark-theme to `qt5ct` configuration

        <details>
        <summary><b>Via Terminal</b></summary>

        > 1. Create `qt5ct` config in user skeleton
        >
        >    ```bash
        >    cat << 'EOF' | sudo tee /etc/skel/.config/qt5ct/qt5ct.conf &>/dev/null
        >    [Appearance]
        >    color_scheme_path=/usr/share/qt5ct/colors/darker.conf
        >    custom_palette=true
        >    standard_dialogs=default
        >    style=Fusion
        >    EOF
        >    ```

        </details>

        <details>
        <summary><b>Via GUI</b></summary>

        > 1. Launch `qt5ct`
        >
        >     ```bash
        >     setsid qt5ct &>/dev/null
        >     ```
        >
        > 2. Set _QT_ color-scheme to `darker`
        > 
        >     _`Appearance` → `Palette` → `Custom` → `Color scheme` → `darker`_
        >
        > 3. Click `OK`
        >
        > 5. Copy `qt5ct` config to user skeleton
        >
        >     ```bash
        >     sudo cp /home/user/.config/qt5ct/qt5ct.conf /etc/skel/.config/qt5ct/
        >     ```

        </details>

3. [**user**@**TemplateVM**]() Shutdown the _TemplateVM_ to apply changes <a name="deb-tvm-3"></a>

    ```bash
    sudo poweroff
    ```


### AppVM Config

> [!IMPORTANT]
> You must execute `sudo apt install -y --no-install-recommends qt5ct` in the _TemplateVM_ of your _AppVM_ before starting the _AppVM_. This is a required package for _QT_-based applications _(KeePassXC, Telegram, VLC)_.
>
> :exclamation: Shutdown the _TemplateVM_ after installing _qt5ct_: `sudo poweroff`

1. [**user**@**AppVM**]() Configure dark-theme for _GTK_ applications <a name="deb-avm-1"></a>

    1. Create `gtk-3.0` directory in user home

        ```bash
        mkdir -p /home/user/.config/gtk-3.0
        ```

    2. Create dark-theme _CSS_ config `gtk.css`

        ```bash
        echo '@import url("resource:///org/gtk/libgtk/theme/Adwaita/gtk-contained-dark.css");' > /home/user/.config/gtk-3.0/gtk.css
        ```

2. [**user**@**AppVM**]() Configure dark-theme for _QT_ applications <a name="deb-avm-2"></a>

    1. Point _QT Platform Theme_ to `qt5ct` in the `~/.profile` script

        ```bash
        cat << 'EOF' | tee -a ~/.profile &>/dev/null
        QT_QPA_PLATFORMTHEME=qt5ct
        QT_SCALE_FACTOR=1
        EOF
        ```

    2. Create `qt5ct` config directory in `.config`

        ```bash
        mkdir -p ~/.config/qt5ct
        ```

    3. Apply dark-theme to `qt5ct` configuration

        <details>
        <summary><b>Via Terminal</b></summary>

        > 1. Create `qt5ct` config in user skeleton
        >
        >    ```bash
        >    cat << 'EOF' > ~/.config/qt5ct/qt5ct.conf &>/dev/null
        >    [Appearance]
        >    color_scheme_path=/usr/share/qt5ct/colors/darker.conf
        >    custom_palette=true
        >    standard_dialogs=default
        >    style=Fusion
        >    EOF
        >    ```

        </details>

        <details>
        <summary><b>Via GUI</b></summary>

        > 1. Launch `qt5ct`
        >
        >     ```bash
        >     setsid qt5ct &>/dev/null
        >     ```
        >
        > 2. Set _QT_ color-scheme to `darker`
        >
        >     _`Appearance` → `Palette` → `Custom` → `Color scheme` → `darker`_
        >
        > 3. Click `OK`

        </details>

3. [**user**@**AppVM**]() Shutdown the _AppVM_ to apply changes <a name="deb-avm-3"></a>

    > <picture>
    >   <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Mqxx/GitHub-Markdown/main/blockquotes/badge/light-theme/info.svg">
    >   <img alt="Info" src="https://raw.githubusercontent.com/Mqxx/GitHub-Markdown/main/blockquotes/badge/dark-theme/info.svg">
    > </picture><br>
    >
    > It is good practice to erase the _bash history (command history)_ of _TemplateVMs_ and _AppVMs_ configured as _disposable templates_. How this is performed depends on the _shell_ type.
    >
    > <details>
    > <summary><b>What is my shell type?</b></summary>
    >
    > > Execute `echo $0` or `echo $SHELL` in your terminal to discover the current shell type.
    >
    > </details>

    - **Clean Shutdown** _(recommended for Disposable Templates)_

        <details>
        <summary><b>Zsh Shell <em>(e.g., Whonix Workstation)</em></b></summary>

        > <picture>
        >   <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Mqxx/GitHub-Markdown/main/blockquotes/badge/light-theme/note.svg">
        >   <img alt="Note" src="https://raw.githubusercontent.com/Mqxx/GitHub-Markdown/main/blockquotes/badge/dark-theme/note.svg">
        > </picture><br>
        >
        > This command appends your `.zshrc` file with a command which deletes the _command history file_ `$HISTFILE`. You only need to execute the below command once per _VM_; subsequent sessions can safely shutdown by simply executing `sudo poweroff`.

        ```bash
        echo 'rm -f "$HISTFILE"' >> ~/.zshrc && sudo poweroff
        ```

        </details>

        <details>
        <summary><b>Bash Shell <em>(e.g., Debian)</em></b></summary>

        ```bash
        cat /dev/null > ~/.bash_history && history -c && sudo poweroff
        ```

        </details>
        
    
    - **Dirty Shutdown**
    
        ```bash
        sudo poweroff
        ```



## Fedora X Configuration

### TemplateVM Config

_todo..._

### AppVM Config

_todo..._
