# Chapter 03: Basic File Management — RHCSA 9 Study Guide

Comprehensive revision notes based on *RHCSA 9 (Red Hat Certified System Administrator)* by Asghar Ghori.

---

## 1. Linux File Types & Identification

Linux does not rely on file extensions to determine file types. Instead, it identifies files by their underlying structures and metadata.

### The 7 File Types in RHEL
| Type Indicator (`ls -l`) | File Type | Description |
|:---:|:---|:---|
| `-` | **Regular file** | ASCII text files, shell scripts, binaries, data files. |
| `d` | **Directory** | Logical containers storing filenames and inode pointers. |
| `b` | **Block device** | Hardware devices transferring data in fixed blocks (e.g., HDDs, SSDs: `/dev/sda`). |
| `c` | **Character device** | Hardware devices transferring raw data character-by-character (e.g., terminals: `/dev/console`). |
| `l` | **Symbolic link** | Soft shortcut pointing to another file/directory path. |
| `p` | **Named pipe (FIFO)** | Used for inter-process communication (IPC). |
| `s` | **Socket** | Used for network and local inter-process communication. |

### Major and Minor Numbers
For device files (`/dev`):
* **Major Number**: Identifies the device driver in the kernel (e.g., `8` for SATA/SCSI disks, `253` for device mapper / LVM).
* **Minor Number**: Identifies the specific unique device or partition controlled by that driver (e.g., `sda1`, `sda2`).

### File Identification Commands
* `ls -l <file>`: Displays the file type indicator as the first character of permissions.
* `file <file>`: Inspects contents and returns specific data format (e.g., `ASCII text`, `ELF 64-bit executable`).
* `stat <file>`: Reports detailed metadata (file type, size, blocks, I/O block, inode, links, UID/GID, Access/Modify/Change/Birth timestamps).

---

## 2. Compression and Archiving

### Compression Utilities: `gzip` vs `bzip2`
* Both utilities replace original uncompressed files with compressed versions upon execution.
* **`gzip` / `gunzip`**: Uses `.gz` extension. Faster compression/decompression speeds, moderate compression ratio.
  * `gzip <file>` &rarr; creates `<file>.gz`
  * `gzip -l <file>.gz` &rarr; displays compression statistics and original name.
  * `gunzip <file>.gz` &rarr; restores original uncompressed file.
* **`bzip2` / `bunzip2`**: Uses `.bz2` extension. Higher compression ratio (smaller file size) but slower processing time.
  * `bzip2 <file>` &rarr; creates `<file>.bz2`
  * `bunzip2 <file>.bz2` &rarr; restores original uncompressed file.

### Archiving with `tar` (Tape Archive)
`tar` packages multiple files and directory structures into a single file (*tarball*), preserving file metadata and hierarchy.

#### Essential `tar` Command Flags
| Flag | Action |
|:---:|:---|
| `-c` | Create a new archive |
| `-f` | Specify the archive filename (must be followed immediately by the filename) |
| `-v` | Verbose mode (list files processed) |
| `-x` | Extract files from an archive |
| `-t` | List table of contents of an archive |
| `-r` | Append files to an uncompressed archive |
| `-u` | Update archive: append newer files only (uncompressed archives only) |
| `-p` | Preserve original file permissions (default for `root`) |
| `-C <dir>` | Extract to a specified target directory |
| `-z` | Filter through `gzip` (create/extract `.tar.gz` or `.tgz`) |
| `-j` | Filter through `bzip2` (create/extract `.tar.bz2`) |
| `-P` | Retain leading `/` (absolute paths) during creation (default strips `/`) |
| `--selinux` | Preserve SELinux security contexts |

#### Common `tar` Command Recipes
```bash
# Create gzip-compressed tarball
tar -czvf /tmp/backup.tar.gz /etc

# Create bzip2-compressed tarball
tar -cjvf /tmp/backup.tar.bz2 /home

# View contents without extracting
tar -tvf /tmp/backup.tar.gz

# Extract to current directory
tar -xvf /tmp/backup.tar.gz

# Extract specific file to current directory
tar -xvf /tmp/backup.tar.gz etc/passwd

# Extract to a different destination directory
tar -xvf /tmp/backup.tar.gz -C /opt/restore
```

---

## 3. Text Editing with `vim`

`vim` operates across three core operational modes:
1. **Command Mode (Escape Mode)**: Default starting mode. Used for navigation, cutting, copying, pasting, and searching.
2. **Input / Insert Mode**: Used for typing text into the active buffer.
3. **Last Line Mode (Extended Mode)**: Invoked by pressing `:` from Command Mode; used for file management, configurations, batch operations, regex replacements.

### Switching into Insert Mode
| Command | Insertion Point |
|:---:|:---|
| `i` | Before current cursor position |
| `I` | At the beginning of current line |
| `a` | After current cursor position |
| `A` | At the end of current line |
| `o` | Open a new line below current line |
| `O` | Open a new line above current line |
| `Esc` | Return to Command Mode |

### Cursor Navigation (Command Mode)
* `h`, `j`, `k`, `l`: Left, Down, Up, Right.
* `w` / `b`: Move forward / backward one word.
* `e`: Jump to end of current/next word.
* `0` / `$`: Jump to beginning / end of current line.
* `[[` / `]]`: Jump to first / last line of file.
* `Ctrl + f` / `Ctrl + b`: Scroll forward / backward full screen.
* *Multiplier prefix*: Any motion can take a count (e.g., `5j` moves down 5 lines, `3w` moves forward 3 words).

### Editing, Deletion, and Manipulation
* `x` / `X`: Delete character at cursor / delete character before cursor.
* `dw`: Delete word from cursor to end of word.
* `dd` / `5dd`: Delete current line / delete 5 lines.
* `D`: Delete from cursor to end of line.
* `:6,12d`: Delete lines 6 through 12.
* `u` / `U`: Undo last command / undo all recent changes on current line.
* `.`: Repeat the last command.
* `~`: Toggle letter case (lower &harr; upper).
* `xp`: Swap current character with character to the right.
* `J`: Join next line with current line.
* `r<char>` / `R`: Replace single character / enter replace (overwrite) mode.
* `cw` / `cc` / `C`: Change word / change entire line / change to end of line (enters insert mode).

### Copy, Cut, and Paste
* `yy` / `3yy`: Yank (copy) current line / yank 3 lines.
* `yw` / `yl`: Yank word / yank letter.
* `p` / `P`: Paste buffer below current line / paste above current line.
* `:1,3co6`: Copy lines 1-3 and insert after line 6.
* `:4,6m9`: Move lines 4-6 and insert after line 9.

### Search and Replace
* `/pattern`: Forward search (`n` next occurrence, `N` previous occurrence).
* `?pattern`: Backward search (`n` next backwards, `N` previous).
* `:%s/old/new`: Replace first occurrence of `old` with `new` on each line.
* `:%s/old/new/g`: Replace all occurrences of `old` with `new` globally.

### Saving, Quitting, and Settings
* `:set nu` / `:set nonu`: Toggle line numbers.
* `:w` / `:w <filename>`: Write buffer / write to specific file.
* `:w!`: Force write (override read-only if owned).
* `:wq` / `:x`: Save changes and quit.
* `:q` / `:q!`: Quit without saving / force quit discarding all changes.

---

## 4. File and Directory Operations

### File Creation & Timestamps
* `touch <file>`: Creates empty 0-byte file if nonexistent; updates access/modification timestamps if exists.
  * `-d <YYYY-MM-DD>`: Set specific date/time.
  * `-m`: Update modification time only.
  * `-a`: Update access time only.
  * `-r <ref_file>`: Clone timestamps from reference file.
* `cat > <file>`: Quick file creation using standard input redirection (press `Ctrl + d` to save and exit).

### Directory Creation
* `mkdir <dirname>`: Create directory.
* `mkdir -p <dir1/dir2/dir3>`: Create parent directories recursively without throwing errors.
* `mkdir -v`: Verbose output.

### Viewing Content
* `cat <file>`: Output entire file (`-n` adds line numbers).
* `more <file>` / `less <file>`: Paged file viewers. `less` is superior (does not load full file into memory before opening, supports both `/` forward and `?` backward search).
* `head -n <N> <file>`: View first $N$ lines (default 10).
* `tail -n <N> <file>`: View last $N$ lines (default 10).
* `tail -f <file>`: Follow mode to watch live append operations (e.g., `/var/log/messages`).
* `wc <file>`: Word count.
  * `-l`: Lines
  * `-w`: Words
  * `-c`: Bytes
  * `-m`: Characters

### Copying, Moving, Renaming, and Deleting
* `cp <src> <dest>`: Copy file.
  * `-i`: Interactive mode (prompt before overwriting; aliased by default for root).
  * `-r` or `-R`: Copy directories recursively.
  * `-p`: Preserve file attributes (permissions, ownership, timestamps).
* `mv <src> <dest>`: Move or rename file/directory.
  * Inside same filesystem: Updates directory entry metadata only (very fast).
  * Across filesystems: Physically copies data to new filesystem and unlinks source.
* `rm <file>`: Remove file.
  * `-i`: Interactive confirmation.
  * `-r` / `-R`: Recursive removal (directories).
  * `-f`: Force removal without prompt, ignore nonexistent files.
  * `-d`: Remove empty directory.
* `rmdir <dir>`: Remove empty directory only.
* **Escaping Wildcards**: To delete literal wildcard filenames (e.g., file named `*`), escape with backslash: `rm /\*`.

---

## 5. File Linking: Hard Links vs Soft Links

### Inode & File Metadata Concepts
* **Inode (Index Node)**: Fixed 128-byte data structure storing all file metadata:
  * File type, size, permissions, UID/GID, timestamps, link count, block pointers.
  * **Crucial Rule**: The inode does **not** store the filename. Filename-to-inode mappings are stored in the directory data block.
* **Link**: An entry associating a human-readable filename with an inode number.

### Hard Links
* Direct pointer to the existing inode.
* Target and link share identical inode number, permissions, ownership, timestamps, and data blocks.
* Link count in `ls -l` increments with each hard link created; decrements when a name is removed.
* File data persists as long as link count > 0.
* **Restrictions**:
  1. Cannot span across different filesystems / partitions.
  2. Cannot link directories (prevents infinite directory recursion loops).
* **Command**: `ln <target> <hardlink>`

### Soft Links (Symbolic Links / Symlinks)
* Independent pointer file containing the pathname (target string) of another file/directory.
* Has its own unique inode number and independent file permissions (typically `lrwxrwxrwx`).
* Size of the symlink equals the character length of the target path.
* Link count of target does not increment.
* **Capabilities**:
  1. Can span across different filesystems.
  2. Can point to directories (e.g., `/bin -> usr/bin`, `/lib64 -> usr/lib64`).
* If source file is deleted, the symlink becomes **dangling / broken**.
* **Command**: `ln -s <target> <symlink>`

### Comparison Matrix: Copy vs Hard Link vs Soft Link
| Feature | `cp` (Copy) | Hard Link (`ln`) | Soft Link (`ln -s`) |
|:---|:---|:---|:---|
| **Data Duplication** | Duplicates actual data | Shared data | Stores path string only |
| **Inode Number** | New unique inode | Identical inode | New unique inode |
| **Cross-Filesystem** | Yes | **No** | Yes |
| **Supports Directories**| Yes (`-r`) | **No** | Yes |
| **Original File Deleted**| Copy unaffected | Data intact (link count - 1)| Link breaks (dangling) |
| **Attribute Sync** | Independent | Synchronized in real time | Independent |