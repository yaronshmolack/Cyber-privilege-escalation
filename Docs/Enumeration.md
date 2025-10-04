# 🧠 Linux Enumeration Commands

> **Note:** These commands are for *legal use only* — auditing, learning, or authorized CTF practice.  
> Enumeration is the first step in privilege escalation and system understanding.

---

## 🖥️ System Information

| Command | Description |
|----------|--------------|
| `hostname` | Returns the hostname of the target machine. |
| `uname -a` | Prints system and kernel information. |
| `cat /proc/version` | Shows kernel version and compiler info (e.g., GCC). |
| `cat /etc/issue` | Displays information about the operating system. |

---

## ⚙️ Processes & Environment

| Command | Description |
|----------|--------------|
| `ps aux` | Lists all processes, including their owners and terminals. |
| `env` | Shows environment variables. |
| `cat /etc/passwd` | Displays user accounts on the system. |

---

## 🌐 Network Information

| Command | Description |
|----------|--------------|
| `netstat -lpt` | Shows listening ports (`-l`), services (`-t`), and PID info (`-p`). |

---

## 🔍 File & Directory Enumeration

| Command | Description |
|----------|--------------|
| `find . -name flag1.txt` | Find “flag1.txt” in the current directory. |
| `find /home -name flag1.txt` | Find “flag1.txt” in `/home`. |
| `find / -type d -name config` | Find directories named “config” under `/`. |
| `find / -type f -perm 0777` | Find files with `777` permissions. |
| `find / -perm a=x` | Find executable files. |
| `find /home -user frank` | Find all files owned by user “frank” under `/home`. |
| `find / -mtime 10` | Find files modified in the last 10 days. |
| `find / -atime 10` | Find files accessed in the last 10 days. |
| `find / -cmin -60` | Find files changed within the last hour (60 min). |
| `find / -amin -60` | Find files accessed within the last hour (60 min). |
| `find / -size 50M` | Find files of size 50 MB. |

---

## ✅ Tips
- Use `2>/dev/null` to suppress permission errors.  
- Combine with `grep` or `xargs` for deeper searches.  
- Run with caution — some commands may take a long time.
