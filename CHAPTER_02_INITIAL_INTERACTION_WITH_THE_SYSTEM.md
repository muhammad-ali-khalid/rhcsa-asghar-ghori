# RHCSA Chapter 02: Initial Interaction with the System

This detailed reference guide covers the Linux graphical environment, the directory structure, essential system commands, and the native help tools available in Red Hat Enterprise Linux (RHEL) 9.

## 1. Linux Graphical Environment
While command-line text interfaces are standard for administration, RHEL provides a robust graphical GUI for convenience.

*   **Wayland:** The default, advanced client/server display protocol in RHEL 9 that replaces the legacy X Window System, offering superior graphics capabilities and performance.
*   **GNOME Display Manager (GDM):** The default login manager that handles the graphical login screen. It displays users, system date/time, network connectivity, power, and accessibility options.
*   **GNOME Desktop Environment:** The default GUI once logged in. It features an **Activities** icon (top left) for searching and launching programs like Firefox, file manager, terminal, and the Settings app (for Wi-Fi, Bluetooth, background, etc.).

---

## 2. Linux Directory Structure and File Systems
RHEL follows the **Filesystem Hierarchy Standard (FHS)**. The file system is an inverted tree starting at the root directory (`/`). 

### File System Categories
1.  **Disk-based:** Persistent data created on physical media (e.g., hard drives). Examples include the root (`/`) and boot (`/boot`) file systems.
2.  **Network-based:** Disk-based systems shared persistently over a network.
3.  **Memory-based (Virtual):** Created at startup and destroyed at shutdown. Data is lost on reboot.

### Key Top-Level Directories
*   `/boot`: (Disk) Contains the Linux kernel, boot support files, and boot configuration files.
*   `/dev`: (Virtual) Managed by the `udevd` service. Stores device nodes for hardware. 
    *   **Character devices:** Accessed serially (keyboards, mice, consoles).
    *   **Block devices:** Accessed in parallel/blocks (hard disks, optical drives).
*   `/etc`: (Disk) System configuration files.
*   `/home`: (Disk) Standard user home directories and personal files.
*   `/root`: (Disk) The default home directory for the root superuser.
*   `/opt`: (Disk) Used for optional, third-party software installations.
*   `/proc`: (Virtual) **Procfs**. Zero-length pseudo-files mapping to memory. Details the running kernel's state, CPU, memory, disks, and processes.
*   `/run`: (Virtual) Runtime data for running processes. `/run/media` auto-mounts external USB/CD drives. 
*   `/sys`: (Virtual) System hardware, drivers, and kernel features configuration.
*   `/tmp`: (Disk) Temporary files. Survives reboots but auto-deletes files inactive for **10 days**.
*   `/var`: (Disk) Variable, dynamic data that frequently changes.
    *   `/var/log`: System, boot, and user logs.
    *   `/var/opt`: Logs for third-party software in `/opt`.
    *   `/var/spool`: Queues for print, cron, and mail jobs.
    *   `/var/tmp`: Large/long-term temporary files. Auto-deletes files inactive for **30 days**.
*   `/usr`: (Disk) UNIX System Resources. 
    *   `/usr/bin`: General user commands/binaries.
    *   `/usr/sbin`: System administration commands (root privileges).
    *   `/usr/lib` & `/usr/lib64`: Shared library routines and system init programs.
    *   `/usr/include`: C language header files.
    *   `/usr/local`: Admin repository for custom/downloaded tools.
    *   `/usr/share`: Shared data, manual pages, and documentation.
    *   `/usr/src`: Source code.

---

## 3. Essential System Commands
Command syntax generally follows: `command option(s) argument(s)`. Options can be short (`-a`) or long (`--all`).

### Navigating and Listing Files
*   **`pwd`**: Prints the absolute working directory.
*   **`cd`**: Changes directory. Supports absolute (`/etc/sysconfig`) and relative (`../usr`) paths. `cd ~` goes home, `cd -` returns to the previous directory.
*   **`tree`**: Displays directory hierarchy. Options: `-a` (hidden), `-d` (directories only), `-h` (human-readable sizes), `-f` (full paths), `-p` (permissions).
*   **`ls`**: Lists files/directories. `ll` is a shortcut for `ls -l`.
    *   `-a`: Show hidden files (starting with `.`).
    *   `-l`: Long listing (permissions, links, owner, group, size, date/time, name).
    *   `-ld`: Long listing of a directory itself, not its contents.
    *   `-lh`: Long listing with human-readable sizes (K, M, G).
    *   `-lt` / `-ltr`: Sort by time (newest first / oldest first).
    *   `-R`: Recursive listing of subdirectories.

### System Information
*   **`tty`**: Identifies your active terminal session device (e.g., `/dev/pts/0`).
*   **`uptime`**: Shows current time, system uptime, logged-in users, and CPU load averages (1, 5, 15 minutes). A load average > 1.00 indicates over 100% load.
*   **`clear`**: Clears the terminal screen (Shortcut: `Ctrl+l`).
*   **`uname`**: Displays system/OS info. 
    *   `-a` (all), `-s` (kernel name), `-n` (hostname), `-r` (kernel release), `-v` (build date), `-m` (hardware architecture), `-p` (processor type), `-o` (OS name).
*   **`lscpu`**: Displays detailed CPU architecture, modes (32/64-bit), cores, sockets, threads, and cache memory (L1, L2, L3).

### Locating Commands
*   **`which`, `whereis`, `type`**: These commands determine the absolute path of an executable (e.g., `which uptime` returns `/usr/bin/uptime`).

---

## 4. Getting Help
RHEL provides robust native documentation tools for commands and configuration files.

### Manual Pages (`man`)
The `man` command opens online documentation. Manuals are categorized into 9 sections (e.g., Section 1 for user commands, Section 5 for configuration file formats, Section 8 for admin commands). Specify a section to narrow the search (e.g., `man 5 passwd`).
*   **Navigation:** 
    *   `Down/Up Arrows`: Move line by line.
    *   `Spacebar` / `f` / `PgDn`: Move forward one page.
    *   `b` / `PgUp`: Move backward one page.
    *   `g` / `G`: Go to the top / bottom of the document.
    *   `/pattern` / `?pattern`: Search forward / backward for a keyword. (`n` for next match, `N` for previous).
    *   `q`: Quit.

### Searching and Quick Help
*   **`mandb`**: Builds/updates the index database required for manual page keyword searches.
*   **`apropos` or `man -k`**: Searches all manual page names and descriptions for a keyword.
*   **`whatis` or `man -f`**: Returns a brief, one-line description of a command or file.
*   **`--help` or `-?`**: Adding these flags directly to a command prints a brief syntax and option summary.

### Additional Documentation
*   **`/usr/share/doc`**: Contains general text documentation (READMEs, ChangeLogs, AUTHORS) for installed packages.
*   **Red Hat Portal:** Comprehensive official documentation (HTML, PDF, EPUB) is available at `docs.redhat.com`.