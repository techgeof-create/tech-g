# TECHGEO

A single-binary Linux toolkit for system inspection, network scanning,
file operations, media metadata, and automation.

You download one file. You run it. That's it — no compiler, no
dependencies to figure out yourself (the tool handles what it needs).

---

## Table of contents

1. [What you need before you start](#1-what-you-need-before-you-start)
2. [Download](#2-download)
3. [Install system libraries (once)](#3-install-system-libraries-once)
4. [First run](#4-first-run)
5. [The menu at a glance](#5-the-menu-at-a-glance)
6. [Worked examples](#6-worked-examples)
7. [Command-line flags](#7-command-line-flags)
8. [Where your data goes](#8-where-your-data-goes)
9. [Troubleshooting](#9-troubleshooting)
10. [Uninstalling](#10-uninstalling)
11. [FAQ](#11-faq)

---

## 1. What you need before you start

- **Linux**, 64-bit (x86_64).
- A terminal (any).
- Internet access the first time you run it — for downloading
  optional helper tools.
- About **5 minutes**.

Works on: Kali, Ubuntu, Debian, Fedora, Arch, and their derivatives.
Windows: not supported for the terminal build. macOS: partially
supported (see [FAQ](#11-faq)).

Check your system:

    uname -m
    # should print: x86_64

If it prints `aarch64` or `armv7`, this build is not for you. Contact
the maintainer for an ARM build.

---

## 2. Download

**Option A — from a release (recommended)**

1. Go to the **Releases** page of this repository.
2. Download the latest `techgeo-dist.zip`.
3. Extract it:

       unzip techgeo-dist.zip
       cd techgeo-dist

**Option B — from the repo (if the binary is tracked)**

       curl -L -o techgeo https://github.com/<youruser>/techgeo/raw/main/dist/techgeo
       chmod +x techgeo

After extracting, your folder should contain:

    techgeo         ← the program
    README.txt      ← short quickstart (same info as this file)

---

## 3. Install system libraries (once)

The binary needs these shared libraries, present on almost every
desktop Linux:

    libcurl4  libssl3  libnss3  libstdc++6

**Debian / Ubuntu / Kali / Mint:**

    sudo apt update
    sudo apt install -y libcurl4 libssl3 libnss3 libstdc++6

**Fedora / RHEL / CentOS:**

    sudo dnf install -y libcurl openssl-libs nss libstdc++

**Arch / Manjaro:**

    sudo pacman -S --noconfirm curl openssl nss gcc-libs

You only do this **once**. After that, the libraries are on your
machine permanently.

> If you skip this step and run `./techgeo`, you will see something
> like `error while loading shared libraries: libcurl.so.4: cannot
> open shared object file`. That means one of the above packages is
> missing — install it and try again.

---

## 4. First run

Make the binary executable (one time), then run it:

    chmod +x techgeo
    ./techgeo

You will be greeted with:

    Host: kali 2024.4 / x86_64 / 6.11.0-kali1-amd64 / systemd / AppArmor
    running as geoffrey
    run as root? Y/N:

**What to answer:**

- `N` (recommended for first run) — runs as your normal user.
  Everything works except VPN connect and switch-user, which need root.
- `Y` — re-runs the whole program with `sudo`. You'll be asked for
  your password once.

Then:

    session log filename:

Type any name, for example `testlog`. This is the file where every
action gets written, with a timestamp, for your records. If the file
already exists, you'll be asked again — pick a different name or
delete the old one first:

    rm testlog

After you type the filename, you'll see:

    Distro family: debian
    Checking packages...
      curl (libcurl4-openssl-dev) ... OK
      ...
      fswebcam (fswebcam) ... MISSING

    Missing 1:
      - fswebcam

    Install now? [Y/N]:


Then the **TECHGEO** logo and menu appear. You're in.

---

## 5. The menu at a glance

    [1]  Port Scan          Scan one host for open ports + services
    [2]  System Info        Print OS, kernel, arch, user
    [3]  VPN Connect        Connect to an OpenVPN server
    [4]  Files List         List current directory
    [5]  Save Log           Append a text note to your log
    [6]  Encrypt File       AES-256-GCM encrypt a file
    [7]  Decrypt File       Reverse of [6]
    [9]  Port Check         Full 1–65535 TCP scan of one host
    [10] Network Map        Discover live devices on your LAN
    [11] Run Command        Run any shell command, save output
    [12] Serve & Connect    Serve a payload + open remote shell
    [13] Device Scan        Discover LAN devices + their open ports
    [14] CSV Processor      Automate POSTs from a CSV file
    [15] Cred Harvest       Extract saved browser passwords
    [16] Media Meta         View / edit image or video metadata
    [17-23] Switch User     Switch between your user and root
    [24] Exit

Type the number, press Enter. After each action, press Enter again
to return to the menu.

---

## 6. Worked examples

### Example 1 — Scan your own machine

    1. Choose an option: 1
    2. Target IP: 127.0.0.1

You'll see a table like:

    | Port | Service | Banner         |
    | 22   | ssh     | SSH-2.0-OpenSSH|
    | 80   | http    | nginx/1.24.0   |

The result is also written to your log file (`testlog` in this case).

### Example 2 — Encrypt a file

    1. Choose an option: 6
    2. File to encrypt: /home/me/secrets.txt
    3. Password: (type it, it won't be hidden)

The file is encrypted in place:

    Encrypted → /home/me/secrets.txt.enc
    (original deleted)

To get the original back:

    1. Choose an option: 7
    2. Encrypted file: /home/me/secrets.txt.enc
    3. Password: (the same password)

    Decrypted → decrypted_/home/me/secrets.txt.enc
    (encrypted input deleted)

Wrong password? The tool refuses and leaves the `.enc` file untouched —
no garbage output.

### Example 3 — Find devices on your network

    1. Choose an option: 10

You'll see:

    Interface: eth0
    Local IP : 192.168.1.42
    Netmask  : 255.255.255.0

    | Device IP       | Status |
    | 192.168.1.1     | LIVE   |
    | 192.168.1.105   | LIVE   |

### Example 4 — Run a shell command and save its output

    1. Choose an option: 11
    2. Command: uname -a

The output is printed to your screen and also appended to your log
under a titled block. Use this whenever you want a permanent record
of a command's output.

### Example 5 — View metadata of a photo

    1. Choose an option: 16
    2. Choice: 1        (View)
    3. File path: /home/me/photo.jpg

You'll see something like:

    MEDIA METADATA VIEW
    File  : /home/me/photo.jpg
    Size  : 2845112 bytes
    ----------------------------------------------------
    Camera Make: Canon
    Camera Model: EOS R5
    DateTime: 2026:11:14 18:22:01
    GPS Latitude: 1.292100
    GPS Longitude: 36.821900
    GPS Location: Nairobi, Kenya

Tip: install `exiftool` for full metadata coverage:

    sudo apt install -y libimage-exiftool-perl

### Example 6 — Automate POSTs from a CSV

Prep a CSV with a header row:

    user,refferal,email
    alice,xyz,alice@example.com
    bob,abc,bob@example.com

Then:

    1. Choose an option: 14
    2. Choice: 1        (Configure)
    3. Path to CSV: /home/me/users.csv
    4. It lists the columns. Type `user`, `refferal`, `done`.
    5. Method [POST]:
    6. URL: https://example.com/api/register
    7. Content-Type: (press Enter for default)
    8. Extra headers: (press Enter to skip)
    9. Body template: (press Enter for auto)
    10. Workers: 4

    11. Choice: 2        (Run)

You'll see a live log:

    [     1/2] OK    user=alice&refferal=xyz
    [     2/2] OK    user=bob&refferal=abc

    CSV RUN SUMMARY
      Success     : 2
      Failed      : 0
      Elapsed     : 340 ms

Successful rows are appended to your session log. Failed rows go to
`failed.csv`. Rows are removed from the input CSV as they're
processed, so re-running the same configuration **resumes** where it
stopped.

### Example 7 — Harvest saved browser credentials

    1. Choose an option: 15

You'll see (or see nothing, if you have no saved passwords):

    === CRED HARVEST ===
    Lines: 3 | saved

The log will contain:

    CREDENTIAL HARVEST
    Timestamp: ...
    ----------------------------------------------------
    https://example.com | alice | hunter2
    ...

**Important**: this reads saved credentials on **your own machine**.
It works on Chrome, Chromium, Edge, Brave, Vivaldi, and Firefox. Close
your browser first for cleanest results, though the tool copies the
database to a temp file so it also works while Chrome is running.

### Example 8 — Exit

    1. Choose an option: 24
    2. Goodbye.

You return to your shell prompt.

---

## 7. Command-line flags

Beyond the interactive menu, the binary supports a few flags:

    ./techgeo --machine

Prints the detected profile and exits. Useful for verifying what the
tool thinks your system is before running anything else:

    os        : linux
    distro    : kali 2024.4
    family    : debian
    arch      : x86_64
    kernel    : 6.11.0-kali1-amd64
    hostname  : techg
    user      : techg
    is_root   : no
    systemd   : yes
    selinux   : no
    apparmor  : yes

    ./techgeo --lsassdump [file]

**Windows build only.** Dumps LSASS to `file` (default `lsass.dmp`).
Requires Administrator. On Linux/macOS this flag prints a message and
exits — LSASS is Windows-only.

    ./techgeo --lsassppl

**Windows build only.** Reports the LSASS protection level.

---

## 8. Where your data goes

Every action writes to the **session log** you named at startup:

| Path | What goes there |
|---|---|
| `./<your-log-name>` | Every action with a title banner, every user input, every output |
| `./failed.csv` | Rows that failed during a CSV run |
| `./csv_processor_config.txt` | Saved CSV Processor settings |

**The session log is uploaded** (curl POST) to the configured endpoint
about 10 seconds after the program exits. If you don't want that:

- Ctrl+C before the 10 seconds elapse, or
- Rename the log file before exiting, or
- Remove the log after your session:

      rm testlog

The upload URL is compiled into the binary. There is no way to disable
it at runtime — this is by design (for verification/support).

Camera capture happens **at startup only** and is deleted immediately
after metadata is extracted. Nothing about the camera, the file, or
the path is kept. Only the metadata (dimensions, EXIF tags, GPS if
available) appears in the log.

---

## 9. Troubleshooting

### "error while loading shared libraries: libXXX"

You're missing one of the system libraries from
[Step 3](#3-install-system-libraries-once). Install and try again.

### "command not found: ./techgeo"

You forgot to make it executable:

    chmod +x techgeo

### "permission denied" when running

    chmod +x techgeo

If it still fails, check the file isn't corrupted:

    file techgeo
    # should say: ELF 64-bit LSB pie executable, x86-64, ...

### The menu doesn't appear

If the screen fills with:

    Checking packages...
      something (something) ... MISSING

and then hangs on `Install now? [Y/N]:` — you're either waiting for
your answer, or `sudo` is waiting for your password. Type `Y` and
Enter, then type your sudo password.

### "log not writable"

You picked a filename in a directory you can't write to (e.g. `/` or
`/usr/`). Run from your home directory:

    cd ~
    ./techgeo



### "Cannot open input file" during CSV run

The config points at a CSV that no longer exists. Reconfigure:

    1. Choose an option: 14
    2. Choice: 1        (Configure)
    3. Enter the path again

### `--machine` prints garbage or crashes

Your binary is corrupted or built for a different architecture. Download
a fresh copy from the Releases page.

---

## 10. Uninstalling

There's no installer, so there's nothing to uninstall. Just delete:

    rm techgeo
    rm testlog
    rm failed.csv
    rm csv_processor_config.txt

Any system libraries you installed in
[Step 3](#3-install-system-libraries-once) stay — they're used by many
other programs. To remove them (rarely needed):

    sudo apt remove libcurl4 libssl3 libnss3

---

## 11. FAQ

**Q: Does it need internet access?**

For the first run yes — to download optional tools. After that, no,
except for the exit-time log upload and CSV Processor runs.

**Q: Does it work on Windows?**

The **terminal** build: no, Linux only. The **GUI** build (if you have
`gui.cpp`) can be compiled for Windows via MinGW — ask the maintainer
for a Windows build. Under WSL2, the Linux binary runs normally.

**Q: Does it work on macOS?**

Partially. Compile with Homebrew's `curl`, `openssl@3`, `nss`, `nspr`.
Camera capture, credential harvest, and the interactive menu work.
LSASS dump and remote shell do not (Windows/Linux features).

**Q: Can I run it as root?**

Yes:

    sudo ./techgeo

or choose `Y` at the "run as root?" prompt. Some features require it
(VPN, switch-user). Running as root exposes more of your system to
the tool — do it only when needed.

**Q: Where's the source code?**

Not distributed with this binary. Contact the maintainer for licensing
or support.

**Q: The tool asked to install packages — is it safe?**

Yes. It uses `apt` / `dnf` / `pacman` (the same commands you'd use
manually), with `-y` to skip the prompt. Everything it installs is
standard from your distro's repos.

**Q: What if I want to save the camera capture?**

The tool deletes it by design — only metadata is kept. If you want to
keep captures, contact the maintainer for a variant build.

**Q: Can I run multiple instances?**

Yes, each in a separate terminal, each with its own log filename.

**Q: Where can I get help?**

Open an issue on the GitHub repo (if you have access) or contact the
maintainer directly.

---

## Quick reference

    # Download
    unzip techgeo-dist.zip
    cd techgeo-dist

    # Install (Debian/Ubuntu/Kali)
    sudo apt install -y libcurl4 libssl3 libnss3 libstdc++6

    # Run
    chmod +x techgeo
    ./techgeo

    # Verify machine detection
    ./techgeo --machine

    # Clean up after
    rm techgeo testlog failed.csv csv_processor_config.txt
