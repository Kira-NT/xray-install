# xray-install

[![Version](https://img.shields.io/github/v/release/Kira-NT/xray-install?sort=date&label=version)](https://github.com/Kira-NT/xray-install/releases/latest)
[![License](https://img.shields.io/github/license/Kira-NT/xray-install?cacheSeconds=36000)](LICENSE.md)

An installer script for [Xray](https://github.com/XTLS/Xray-core), best suited for systemd-based systems.

---

## Quick Start

1) First, decide whether you want to run the script as root or as your regular user. If you are installing Xray on a server or want to use a transparent proxy setup, you **must** run it as root *(e.g., `sudo -i`)*.

2) Then, download the installer script:
    ```sh
    chmod 755 "$(curl 'https://github.com/Kira-NT/xray-install/blob/main/xray-install?raw=true' -fsSLOJw '%{filename_effective}' --create-dirs --output-dir "$([ "$(id -u)" = 0 ] && echo /usr/local/bin || echo "${HOME}/.local/bin")")"
    ```

3) Run the installer.
    - For a regular setup:
        ```sh
        # --channel should be one of: release, prerelease
        # --geodata-type should be one of: cn, ir, ir-lite, ru, ru-lite, none
        xray-install --channel release --geodata-type cn --autorun --update weekly
        ```
    - For a transparent proxy setup:
        ```sh
        # --channel should be one of: release, prerelease
        # --geodata-type should be one of: cn, ir, ir-lite, ru, ru-lite, none
        xray-install --channel release --geodata-type cn --tproxy-port 4589 --autorun --update weekly
        ```

4) Update the configuration template provided by the installer with your inbounds, outbounds, and other necessary settings.

5) Restart the service via:
    - `systemctl restart xray`, if the script was run as root.
    - `systemctl --user restart xray`, if the script was run as a non-privileged user.

6) Enjoy!

> [!WARNING]
> If you enable automated updates for the `xray` executable, `geoip.dat`, and `geosite.dat` via `--update <timer>`, do **not** delete `xray-install` from your system, as this script is what's responsible for performing those updates.

> [!TIP]
> If you need a proxy in order to be able to download Xray to begin with, you can specify it using the `--proxy <proxy>` flag.<br>
> `xray-install` will remember the value you provided and reuse it for future automated updates.
>
> Once your local Xray instance is up and running, you can configure future automated updates to use it as a proxy by re-running the installer script with the SOCKS5 inbound from your config as `--proxy`.

> [!TIP]
> You can add any non-privileged user to the newly created `xray` group *(`usermod -aG xray username`)* to allow them to manage the system-wide Xray installation without needing to enter a password, which may be useful for scripting.

----

## Usage

```
Usage: xray-install [-p <proxy>] [-x <url>] [-C <release|prerelease>] [-b <filename>]
       [-i <url>] [-s <url>] [-t <cn|ir|ir-lite|ru|ru-lite|none>] [-d <directory>]
       [-l <directory>] [-c <filename>] [-P <port>] [-a] [-u <timer>] [-r <command>] [--uninstall]

Downloads and configures Xray.

Examples:
  xray-install --geodata-type none
  xray-install --geodata-type cn --autorun --update weekly
  xray-install -p socks5://127.0.0.1:1080 -P 4242 -t cn --autorun --update weekly
  xray-install --uninstall

Options:
  -h, --help
      Display this help page.

  -p, --proxy <proxy>
      Specify a proxy server to use.

  -x, --xray-url <url>
      Specify a URL to download the Xray executable.

  -C, --xray-channel, --channel <release|prerelease>
      Specify a channel to download the Xray executable from.

  -b, --binary, --xray-path <filename>
      Specify a filename for the Xray executable.

  -i, --geoip-url <url>
      Specify a URL to download geoip.dat.

  -s, --geosite-url <url>
      Specify a URL to download geosite.dat.

  -t, --geodata-type <cn|ir|ir-lite|ru|ru-lite|none>
      Select a predefined pair of geoip.dat and geosite.dat files to download.

  -d, --data-path, --geodata-path <directory>
      Specify a directory where geoip.dat and geosite.dat should be saved.

  -l, --log-path <directory>
      Specify a directory where log files should be stored.

  -c, --config-path <filename>
      Specify a filename for the configuration file.

  -P, --tproxy-port <port>
      Specify a port for configuring a transparent proxy.

  -a, --autorun
      Create a service that automatically starts Xray on boot.

  -u, --update <timer>
      Create a service that automatically updates Xray and/or its associated files.
      https://man.archlinux.org/man/systemd.time.7

  -r, --reload-command <command>
      Specify a command to execute when Xray or any of its associated files are updated.

  --uninstall
      Uninstall Xray and all associated files.
```

----

## Useful Resources

- [Absolute Beginner's Guide by Project X](https://xtls.github.io/en/document/level-0/)
- [XTLS/Xray-examples](https://github.com/XTLS/Xray-examples)
- [chika0801/Xray-examples](https://github.com/chika0801/Xray-examples)
- [DanielLavrushin/GeodatExplorer](https://github.com/DanielLavrushin/GeodatExplorer)

----

## License

Licensed under the terms of the [MIT License](LICENSE.md).
