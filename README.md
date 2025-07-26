# 🔓 Hacking Windows Login Password via Utilman.exe Exploit

> ⚠️ **Disclaimer**
> This guide is for **educational and ethical hacking purposes only**. Gaining unauthorized access to computer systems is illegal and unethical. Only use this technique in controlled environments or systems you own or have permission to test.

---

## 📖 Overview

This repository documents a **step-by-step method to bypass a Windows local user account password** using a well-known technique involving the `utilman.exe` accessibility executable on the Windows login screen. This technique demonstrates how physical access to a machine, combined with bootable media and Windows command-line tools, can be exploited to gain system access.

This exploit works by replacing `utilman.exe` (the Ease of Access application) with `cmd.exe` (Command Prompt) to execute commands with **SYSTEM privileges** directly from the login screen.

---

## 🎯 Objectives

* Demonstrate how attackers can bypass the Windows login screen using command-line tools.
* Understand the critical importance of **physical security**, **disk encryption**, and **boot protection**.
* Provide preventive measures to mitigate such attacks in enterprise or home environments.

---

## 🧰 Requirements

| Requirement            | Description                                                                                             |
| ---------------------- | ------------------------------------------------------------------------------------------------------- |
| 💾 USB Drive           | 8GB or larger, formatted for boot                                                                       |
| 💿 Windows ISO         | Downloaded via Microsoft’s [Media Creation Tool](https://www.microsoft.com/software-download/windows10) |
| 🧠 Basic CMD Knowledge | Understanding of `net user`, `cd`, `copy` commands                                                      |
| ⚙️ Physical Access     | Must have direct access to the target machine                                                           |

---

## 🚧 Exploitation Process: Step-by-Step

### 🔹 Step 1: Boot into Safe Mode

Some security features, like Windows Defender, may prevent system file changes. To safely bypass this:

1. On the Windows **login screen**, press and hold the **Shift key**.
2. While holding Shift, click the **Power icon > Restart**.
3. The system will enter the **Advanced Startup Environment**.
4. Navigate to:

## Troubleshoot
<img width="317" height="365" alt="Screenshot 2025-07-26 123703" src="https://github.com/user-attachments/assets/6d4f900a-8fa6-47f8-9797-395561c3397d" />

## Advanced Options 
<img width="357" height="290" alt="Screenshot 2025-07-26 123749" src="https://github.com/user-attachments/assets/7618065b-cf29-4e7a-84a4-07511c058353" />

## Startup Settings → Restart
<img width="582" height="366" alt="Screenshot 2025-07-26 123804" src="https://github.com/user-attachments/assets/979d61bf-93fc-4c6f-929e-5077256c9276" />

5. After reboot, select the option for **Safe Mode** by pressing the corresponding function key (usually **F4**).
<img width="564" height="485" alt="Screenshot 2025-07-26 123913" src="https://github.com/user-attachments/assets/6cc397a6-4b97-4bd9-9e13-10052f45a0a4" />

---

### 🔹 Step 2: Boot from USB Installation Media

1. Plug in the **Windows installation USB**.
2. Restart the system and boot from the USB. This may require entering the **boot menu (F12, Esc, or F2)** depending on the machine.
3. On the first Windows Setup screen, **do not proceed with installation**.
4. Instead, press **Shift + F10** to open a **Command Prompt** with system-level privileges.

---

### 🔹 Step 3: Identify the Windows Drive

Type the following command:

```cmd
wmic logicaldisk get name
```
<img width="745" height="398" alt="Screenshot 2025-07-26 123306" src="https://github.com/user-attachments/assets/4116aff1-28b3-4717-bf80-b6370f4b10be" />

This will list all available disk partitions (e.g., `C:`, `D:`, etc.).

Identify which drive contains the **Windows installation** (usually by checking for the `Windows` folder). For example:

```cmd
cd D:\Windows\System32
```

(Replace `D:` with the correct drive letter.)

---

### 🔹 Step 4: Replace `utilman.exe` with `cmd.exe`

```cmd
copy cmd.exe utilman.exe
```
<img width="776" height="432" alt="Screenshot 2025-07-26 123356" src="https://github.com/user-attachments/assets/064a26b0-36a1-4d14-ae75-e8dbee92b894" />

This makes clicking the **Ease of Access icon** launch the command prompt instead of the accessibility tool.

---

### 🔹 Step 5: Reset the Local User Password

1. Close the installation window and restart the system.
2. On the login screen, click the **Ease of Access** button (bottom-right corner).
3. This opens **Command Prompt** with **SYSTEM privileges**.
4. To reset the password of a user:

```cmd
net user <username> <newpassword>
```
<img width="649" height="182" alt="Screenshot 2025-07-26 124145" src="https://github.com/user-attachments/assets/13cc65cb-0b7b-46af-a013-54082d8a9275" />

✅ Example:

```cmd
net user Pranav 123456
```

5. Close the Command Prompt and login with the new credentials.

---

## 🔒 How to Prevent This Exploit

Although this method is effective, it can be **completely prevented** with proper system hardening techniques.

### 🛑 1. Restrict Physical Access

* Do not allow unauthorized users near unattended devices.
* Lock or secure workstations in public areas.

### 🔐 2. Enable BitLocker Encryption

* BitLocker encrypts the entire disk. Even if someone boots from USB, the encrypted drive cannot be accessed without a recovery key.

> 💡 BitLocker is available in Pro and Enterprise editions of Windows.

### 🧬 3. Enable BIOS/UEFI Password

* Set a **BIOS or UEFI master password** to prevent:

  * Boot order modification
  * USB booting
  * Unauthorized firmware access

---

## 🧠 Summary

| Exploit Feature      | Description                                 |
| -------------------- | ------------------------------------------- |
| Attack Vector        | Physical access + USB                       |
| Vulnerable Component | `utilman.exe` on login screen               |
| Privilege Escalation | SYSTEM-level via CMD                        |
| Tools Used           | Windows CMD, Bootable USB                   |
| Prevention           | BitLocker, BIOS password, physical security |

This exploit highlights a fundamental truth in cybersecurity:

> **If an attacker has physical access to a device, and the device isn't protected by full-disk encryption, it's already compromised.**

---

## 📁 Related Resources

* [BitLocker Overview (Microsoft Docs)](https://learn.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview)
* [Net User Command Reference](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/net-user)
* [Media Creation Tool](https://www.microsoft.com/software-download/windows10)

