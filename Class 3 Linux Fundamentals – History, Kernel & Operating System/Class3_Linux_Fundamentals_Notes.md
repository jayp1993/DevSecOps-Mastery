# Class 3: Linux Fundamentals – History, Kernel & Operating System

## 1. Introduction to Linux

**Linux** is an open-source operating system kernel. A complete Linux-based operating system combines the Linux kernel with system utilities, libraries, a shell, services, and applications.

Linux is used in:
- Cloud platforms such as Azure, AWS, and Google Cloud
- Web and application servers
- Docker containers and Kubernetes environments
- DevOps tools and CI/CD pipelines
- Networking, cybersecurity, and embedded devices

**Real-world example:** When you create an Ubuntu Virtual Machine in Azure and install Nginx, Linux provides the environment in which the web server runs.

## 2. History of Linux and Unix

| Year | Development | Importance |
|---|---|---|
| 1969 | UNIX | Development of a multiuser and multitasking operating system began. |
| 1983 | GNU Project | Richard Stallman started the GNU Project to build a free software operating system. |
| 1985 | Windows 1.0 | Microsoft released an early graphical operating environment. |
| 1991 | Linux kernel | Linus Torvalds began developing the Linux kernel. |
| Today | Linux distributions | Linux is used in servers, cloud platforms, containers, and personal devices. |

### Important people

- **Richard Stallman:** Associated with the GNU Project and the free software movement.
- **Linus Torvalds:** Original developer of the Linux kernel.

### Open Source vs. Closed Source

| Open Source | Closed Source |
|---|---|
| Source code can be inspected under the applicable license. | Source code is generally not publicly available. |
| Modification and redistribution depend on the license. | Modification and redistribution are subject to vendor restrictions. |
| Examples: Linux kernel, Ubuntu. | Examples: Microsoft Windows and proprietary commercial software. |

**Interview point:** Open source does not always mean completely free of cost. It primarily concerns source-code availability and the freedoms granted by the license.

## 3. What Is an Operating System (OS)?

An **Operating System (OS)** is system software that provides an interface between users, applications, and computer hardware.

### Main responsibilities of an OS

1. Manage hardware resources.
2. Provide an environment for running applications.
3. Manage files and directories.
4. Manage users and permissions.
5. Support networking and device access.
6. Support system security and resource allocation.

### Examples of operating systems

- Microsoft Windows
- Linux-based operating systems
- macOS
- Android
- ChromeOS

## 4. Linux Architecture

Linux architecture can be understood as layers:

```text
+--------------------------------------+
| User / Applications                   |
| Browser, editor, terminal, apps       |
+--------------------------------------+
                    |
                    v
+--------------------------------------+
| Operating System Environment          |
| Shell, libraries, utilities, services |
+--------------------------------------+
                    |
                    v
+--------------------------------------+
| Kernel                                |
| CPU, RAM, processes, devices          |
+--------------------------------------+
                    |
                    v
+--------------------------------------+
| Hardware                              |
| CPU, RAM, disk, network interface     |
+--------------------------------------+
```

### Architecture example

Suppose you execute this command in a Linux terminal:

```bash
ls -l
```

- **User:** Executes the command.
- **Shell:** Interprets the command.
- **System utility:** The `ls` program requests directory information.
- **Kernel:** Provides controlled access to the filesystem and required resources.
- **Hardware/storage:** Makes the stored directory data available.

Each layer has a different responsibility.

## 5. What Is the Linux Kernel?

The **kernel is the core component of an operating system**. It manages system resources and provides controlled interaction between applications and hardware.

### Main functions of the kernel

| Function | Explanation | Example |
|---|---|---|
| CPU management | Allocates CPU time | Scheduling multiple applications |
| Memory management | Allocates and releases RAM | Providing memory to a browser |
| Process management | Creates, schedules, and terminates processes | Running an Nginx process |
| Device management | Coordinates devices and drivers | Accessing a disk or network card |
| Filesystem management | Supports filesystem operations | Reading and writing files |
| Networking | Supports network communication | Handling incoming server connections |
| Security and access control | Enforces access restrictions and isolation | Restricting process access |

### Kernel example

```bash
sudo systemctl start nginx
```

Systemd helps start the service. The kernel manages resources needed for process execution, memory, and networking.

**Important:** The kernel and the operating system are not the same thing. The kernel is the core; a complete OS also includes utilities, libraries, services, and user interfaces.

## 6. Kernel vs. Operating System

| Feature | Kernel | Operating System |
|---|---|---|
| Meaning | Core of the OS | Complete system-software environment |
| Hardware interaction | Manages core resources | Uses the kernel and other components to access hardware |
| Process management | Provides core process-management functions | May also provide management tools and services |
| User interface | Does not itself provide the normal user interface | Can provide CLI or GUI |
| Components | Scheduling, memory management, drivers, networking | Kernel, utilities, libraries, services, interface |
| Example | Linux kernel | Ubuntu, Fedora, RHEL |

## 7. Linux-Based Operating Systems (Distributions)

A Linux-based operating system is often called a **Linux distribution** or **distro**.

A distribution commonly includes:
- Linux kernel
- Package manager
- System utilities
- Libraries
- Services
- Shell
- Optional graphical desktop environment

### Linux distributions and their uses

| Distribution | Common use | Real-world example |
|---|---|---|
| **Ubuntu** | Beginners, developers, cloud, servers | Azure VM, web server, Docker host |
| **Red Hat Enterprise Linux (RHEL)** | Enterprise production environments | Corporate servers and business applications |
| **CentOS Stream** | RHEL development ecosystem and testing | Enterprise software testing and platform development |
| **AlmaLinux** | Enterprise Linux servers | Web hosting and production workloads |
| **Rocky Linux** | Enterprise Linux environments | Application servers and infrastructure labs |
| **Debian** | Stable servers and general-purpose computing | Web servers and cloud infrastructure |
| **Fedora** | Developers and newer Linux technologies | Development workstations and technology testing |
| **Alpine Linux** | Lightweight containers | Docker images and microservices |
| **Kali Linux** | Cybersecurity and penetration testing | Authorized security labs and vulnerability assessments |
| **Parrot OS** | Security testing, digital forensics, privacy | Ethical hacking labs |
| **Linux Mint** | Desktop users and beginners | Personal computers and office desktops |
| **Arch Linux** | Advanced users and customization | Customized Linux workstation |
| **openSUSE / SUSE Linux Enterprise** | Enterprise systems and administration | Business servers and enterprise infrastructure |
| **Amazon Linux** | AWS cloud workloads | EC2 instances and cloud applications |
| **Oracle Linux** | Enterprise workloads and Oracle ecosystem | Database servers and business applications |
| **Raspberry Pi OS** | Education, IoT, embedded projects | Raspberry Pi and hardware automation |
| **Android** | Mobile devices | Smartphones, tablets, Android TV |
| **ChromeOS** | Chromebook devices | Education and browser-based computing |

**Note:** Android uses the Linux kernel, but it is not the same as a conventional desktop Linux distribution. Distribution choice depends on workload, organizational standards, support requirements, and security needs.

### Which Linux should you learn for Cloud and DevOps?

| Requirement | Useful starting point |
|---|---|
| Azure Linux VM | Ubuntu or RHEL |
| AWS EC2 | Amazon Linux or Ubuntu |
| Docker containers | Alpine, Debian-based images, or Ubuntu |
| Kubernetes nodes | A Linux distribution supported by the platform |
| Enterprise administration | RHEL or SUSE; Rocky Linux and AlmaLinux are also useful |
| Cybersecurity lab | Kali Linux or Parrot OS |

These are general recommendations. Always check the requirements and support policies of the target platform.

## 8. Client OS vs. Server OS

| Client OS | Server OS |
|---|---|
| Designed for end users' everyday tasks | Configured to host applications and services |
| Often includes a GUI | GUI is optional; CLI is commonly used |
| Focuses on desktop applications and user experience | Focuses on networking, availability, security, and remote administration |
| Examples: Windows 11, Ubuntu Desktop | Examples: Ubuntu Server, RHEL |

**Important:** The difference is not only the GUI. A Linux server can run with a GUI, and a desktop system can be operated through the CLI.

## 9. GUI vs. CLI

### GUI — Graphical User Interface

A GUI lets users interact with a system through windows, icons, menus, and a pointer.

Examples:
- Windows Desktop
- Ubuntu Desktop

### CLI — Command-Line Interface

A CLI lets users operate a system by entering text commands.

Examples:
- Bash terminal
- SSH session

### Why is CLI important in DevOps?

- Managing remote servers through SSH
- Troubleshooting Linux services and processes
- Running Docker and Kubernetes commands
- Automating Terraform and CI/CD workflows
- Administering cloud VMs without a GUI

### Basic commands

```bash
whoami
uname -r
free -h
df -h
```

- `whoami` — displays the current user.
- `uname -r` — displays the kernel version.
- `free -h` — displays memory usage.
- `df -h` — displays filesystem disk usage.

## 10. Quick Revision

| Question | Answer |
|---|---|
| What is the Linux kernel? | The core component of an OS |
| Who developed the Linux kernel originally? | Linus Torvalds |
| Who started the GNU Project? | Richard Stallman |
| What is the main purpose of an OS? | To manage applications and hardware resources |
| What is Ubuntu? | A Linux distribution |
| How does the kernel manage RAM? | Through memory-management functions |
| What does CLI stand for? | Command-Line Interface |
| What does GUI stand for? | Graphical User Interface |
| Where is Alpine Linux useful? | Lightweight containers |
| Where is Amazon Linux used? | AWS workloads |
| What is Kali Linux mainly used for? | Authorized security testing |
| Is a GUI mandatory on a Linux server? | No |

## 11. Practice Questions

1. What is the Linux kernel?
2. Who originally developed the Linux kernel?
3. Which Linux distribution is popular for lightweight container images?
4. What is required to run Nginx on an Azure VM?
5. What does CLI stand for?
6. What is the primary role of the Linux kernel?
7. What is the difference between a kernel and an operating system?
8. Name three Linux distributions commonly used for server workloads.
9. Why is CLI useful in DevOps?
10. Is Android based on the Linux kernel?

## 12. Practical Homework

Run these commands on your Ubuntu VM and record the output:

```bash
# Operating system information
cat /etc/os-release

# Kernel information
uname -a
uname -r

# Current logged-in user
whoami

# CPU information
lscpu

# Memory information
free -h

# Disk usage
df -h

# Running processes
ps aux
```

For each command, explain what information it provides about the OS, kernel, CPU, memory, disk, or processes.

## Key Takeaway

To learn Linux, first understand its history, kernel, operating-system architecture, distributions, and CLI. Once the fundamentals and basic commands are clear, scripting and automation will be easier to learn.

---

**Source:** Based on the uploaded `Class3_Linux-fundamentals.pdf`, pages 3–12. Explanations and practical examples have been expanded for beginner-friendly study notes.
