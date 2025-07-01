# AIC8800 Linux Driver

This repository provides the source code for the AIC8800 Wi-Fi driver, modified to ensure compatibility with Linux kernels version 6.x and newer. The primary purpose of this fork is to address compilation errors caused by API changes in the kernel's `cfg80211` subsystem.

## The Problem

The official driver, often distributed as `aic8800fdrvpackage.deb`, fails to compile on recent Linux distributions (e.g., Ubuntu 24.04 with Kernel 6.11) due to breaking changes in the Linux kernel's `cfg80211` wireless subsystem API.

Users will typically encounter compilation errors similar to these:

```
error: too many arguments to function ‘cfg80211_ch_switch_notify’
...
error: too many arguments to function ‘cfg80211_ch_switch_started_notify’
```

This occurs because the function signatures for these kernel APIs have changed, and the driver's source code has not been updated to reflect this.

## The Solution

This repository provides the complete, patched driver source in a structure that can be directly built into a `.deb` package. The function calls in `AIC8800/drivers/aic8800/aic8800_fdrv/rwnx_main.c` have been modified to match the updated kernel APIs.

### Tested Environment
*   **Kernel Version**: Successfully tested on `Linux Kernel 6.11.0-28-generic`
*   **Distribution**: Ubuntu 24.04 LTS
*   **Target Chip**: AIC8800 (USB Wi-Fi Adapter)

This patch should also work on other 6.x kernels that have the same updated `cfg80211` function signatures.

---

## Installation Guide

The **recommended** way to install this patched driver is to build a `.deb` package directly from this repository. This ensures a clean installation and removal process through your system's package manager.

### Prerequisites

First, ensure you have the necessary tools for building packages and kernel modules.

```bash
sudo apt update
sudo apt install build-essential linux-headers-$(uname -r) git dpkg-deb
```

### Building and Installing the `.deb` Package

1.  **Clone this repository:**
    ```bash
    git clone https://github.com/hellofuxu/aic8800-linux-driver.git
    ```

2.  **Build the `.deb` package:**
    This command takes the cloned directory (`aic8800-linux-driver`) and packages its contents into a `.deb` file. The command should be run from the parent directory where you cloned the repo.
    ```bash
    dpkg-deb --build aic8800-linux-driver
    ```
    After this step, a new file named `aic8800-linux-driver.deb` will be created.

3.  **Install the package:**
    You can now install the fixed driver using your newly created `.deb` file.
    ```bash
    sudo dpkg -i aic8800-linux-driver.deb
    ```
    The driver should now be successfully compiled by the package's post-installation script and loaded automatically.

---

## Alternative Method: Building from Source

For advanced users, or for environments where a `.deb` package is not suitable, it is possible to compile and install the driver directly from the source code provided in this repository.

The necessary `Makefile`(s) are included in the source directories. You will need to inspect them to understand the specific build targets and the required manual installation process. This method is not recommended for most users as it bypasses the system's package management.

## Troubleshooting

*   **Secure Boot:** If your system has Secure Boot enabled, it may block the loading of this unsigned third-party kernel module. You may need to sign the module yourself or disable Secure Boot in your system's UEFI/BIOS settings.
*   **Kernel Headers:** Ensure the `linux-headers-$(uname -r)` package is installed and perfectly matches your running kernel version. If you recently updated your kernel, reboot your machine before attempting to build the driver.

## Disclaimer

This is an unofficial community patch. It is provided "as is" without any warranty. The original driver and its source code belong to their respective copyright holders. Use at your own risk.

## Contributing

Feel free to open an issue if you encounter problems or submit a pull request if you have improvements to the patch.