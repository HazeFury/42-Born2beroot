*This project has been created as part of the 42 curriculum by marberge.*

___

<div align="center">
<br>
  <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTQPzuYKu7n0cWUYa5Kbg0_LrlEQAIURWeo9A&s" alt="42 Logo" width="400" />

  <br>
</div>

# Born2beRoot

![Language](https://img.shields.io/badge/Language-Shell-blue)
![Score](https://img.shields.io/badge/Score-110%2F100-brightgreen)
![Tag](https://img.shields.io/badge/UNIX-grey)
![Tag](https://img.shields.io/badge/GNU/Linux-grey)
![Tag](https://img.shields.io/badge/System-grey)
![Tag](https://img.shields.io/badge/Virtual_Machine-grey)
![Tag](https://img.shields.io/badge/Debian-grey)

## Description

**Born2beRoot** is the first System Administration project in the 42 curriculum. It introduces the fundamentals of virtualization, partitioning, and server security.

The goal is to create a secure **Debian** (or Rocky) server within a Virtual Machine. This server must adhere to strict rules regarding partitioning (LVM), encryption, password policies, firewall configuration, and SSH access. Additionally, a monitoring script must be implemented to broadcast system information to all terminals at specific intervals.

This project simulates the setup of a physical server, requiring a deep understanding of the Linux operating system, its file system hierarchy, and user privilege management.

I used this project as a good pretext to learn more about GNU/Linux (the philisophy, how it works, most useful commands, bash scripting, etc) and I've build my own personnal [DOCUMENTATION](https://github.com/HazeFury/linux_space) on Linux.
The goal of this repository is to centralize my kmowledge on this subject and I hope it will follow me during my dev carrier.

## Instructions

Since this project results in a Virtual Machine image, there is no "compilation" process. Here is how to run and connect to the server.

### 1. Verification (Signature)
Before launching the VM, verify that the signature of the virtual disk matches the one provided in the submission file `signature.txt`.

	shasum born2beroot.vdi

### 2. Launching the VM
- Open VirtualBox (or UTM on Apple Silicon).
- Import the machine using the `.vdi` (or `.utm`) file.
- Start the Virtual Machine.

### 3. Connection via SSH
The VM is configured to not allow root login via SSH and listens on port **4242**. You must connect using the standard user created during the defense.

	ssh marberge@localhost -p 4242

## Project Description

### Operating System Choice: Debian
For this project, I chose **Debian** (Stable) over Rocky Linux.

* **Why:** Debian is renowned for its extreme stability and its strict adherence to the free software philosophy. It uses `apt` and `.deb` packages, which have a vast community and documentation. It is the upstream distribution for Ubuntu, making it a great entry point for learning Linux servers.
* **Pros:** Stability, huge package repository, widely used in the industry, `AppArmor` is easier to configure than `SELinux` for beginners.
* **Cons:** Software versions in the "Stable" branch can be older than in other distributions (less "bleeding edge").

### Main Design Choices

* **Partitioning (LVM & Encryption):**
    The disk is encrypted using **LUKS**. On top of that, **LVM** (Logical Volume Manager) is used to create flexible partitions (`/root`, `/home`, `/var`, `/srv`, etc.). This allows for resizing partitions later without reformatting.

* **SSH Configuration:**
    * Port changed to **4242** (Security through obscurity).
    * **Root login disabled** (Forces the use of a user account + `sudo` or `su`).

* **Firewall (UFW):**
    All ports are blocked by default. Only port **4242** is explicitly allowed.

* **Password Policy:**
    A strict password policy has been enforced using `libpam-pwquality` (minimum length, uppercase, lowercase, digits, no username in password, expiration every 30 days).

* **Sudo Policy:**
    Configuration of `visudo` to limit authentication attempts, log all actions to `/var/log/sudo/`, and require a password for every command.

* **Monitoring Script:**
    A Bash script runs every 10 minutes (via `cron`) to display system architecture, CPU usage, RAM/Disk usage, and network connections using `wall`.

### Technical Comparisons

Here is a summary of the differences between the technologies available for this project.

#### 1. Debian vs Rocky Linux
| Feature | Debian | Rocky Linux |
| :--- | :--- | :--- |
| **Family** | Debian (Mother of Ubuntu/Kali) | Red Hat (Clone of RHEL) |
| **Package Manager** | `apt` / `dpkg` | `dnf` / `rpm` |
| **Philosophy** | Community-driven, 100% Free | Enterprise-focused, binary compatible with RHEL |
| **Security Module** | AppArmor (default) | SELinux (default) |

<br>

---

<br>

#### 2. AppArmor vs SELinux
| Feature | AppArmor | SELinux |
| :--- | :--- | :--- |
| **Used by** | Debian, Ubuntu, SUSE | Red Hat, Rocky, Fedora, Android |
| **Mechanism** | **Path-based**: Restrictions are applied to file paths. | **Label-based**: Files and processes have "labels" (inodes). |
| **Complexity** | Easier to learn and configure. | Steep learning curve, very granular control. |

<br>

---

<br>

#### 3. UFW vs Firewalld
| Feature | UFW (Uncomplicated Firewall) | Firewalld |
| :--- | :--- | :--- |
| **Used by** | Debian / Ubuntu | Red Hat / Rocky |
| **Approach** | Simple command-line interface for netfilter. Static rules. | Dynamic management with support for network/firewall "zones". |
| **Usage** | Great for single servers. | Great for complex networks and laptops changing Wi-Fi often. |

<br>

---

<br>

#### 4. VirtualBox vs UTM
| Feature | VirtualBox | UTM (QEMU) |
| :--- | :--- | :--- |
| **Architecture** | Best for **x86/AMD64** (Intel Macs, Windows, Linux). | Best for **ARM64** (Apple Silicon M1/M2/M3). |
| **Performance** | Slow emulation on Apple Silicon. | Native virtualization speed on Apple Silicon. |
| **Features** | Excellent Snapshot management. | Modern UI, but basic snapshot features compared to VBox. |

## Resources

### References
* **Debian Handbook:** The official documentation for Debian administration.
* **Mainstream website and more specific website**: Wikipedia, Stackoverlow, etc. 
* **Man pages:** for example => `man sudo`, `man ufw`, `man sshd_config` and more.
* **Born2beRoot Subject:** 42 School project specifications.
* **Born2beRoot Guide:** I used this [guide](https://noreply.gitbook.io/born2beroot) to help me with the concepts to explore and the tasks to complete.



### AI Usage
This project was realized with the assistance of **Gemini** (AI).
* **Usage:** The AI acted as a "docent" and a thought partner. It helped clarify complex concepts (LVM, TTY, SELinux vs AppArmor), debugged the bash monitoring script syntax, and helped structure the study plan for the defense.
* **Scope:** The code and configuration were manually typed and understood by the student. The AI provided explanations, comparative tables, and markdown formatting for documentation.

<br>

___

<p align="center">Made by ❤️ by marberge<p>