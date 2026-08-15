# Linux Hardening - ISO/IEC 27001

Applying practical ISO/IEC 27001 information security controls on an Ubuntu Linux server (AWS EC2 lab environment).

[Leia em português](README.pt-BR.md)

## Objective

Simulate a real-world Linux hardening audit, applying and documenting controls commonly requested in Information Security and Cloud roles.

## Environment

- Ubuntu Server (AWS EC2)
- Standard user with sudo privileges

## Controls Applied

1. **Installed package audit** - listed all installed packages (`dpkg -l`) for baseline review.
2. **Terminal session timeout** - configured automatic session logout after inactivity (`/etc/profile`, `TMOUT`).
3. **Root account lockdown** - disabled direct root login to enforce accountability via named user accounts + sudo.
4. **Password aging policy** - enforced password expiration, warning period, and inactivity lock (`chage -M 90 -W 7 -I 14`).
5. **Weak password detection (bonus)** - used John the Ripper to demonstrate how weak passwords (e.g. `123456`) are trivially cracked, reinforcing the case for strong password policies.

## Evidence

### 1. Installed package audit
![Package audit](images/01-package-audit.png)

### 2. Terminal session timeout
![Terminal timeout](images/02-terminal-timeout.png)

### 3. Root account lockdown
![Root lockdown](images/03-root-lockdown.png)

### 4. Password aging policy - before
![Password aging before](images/04-chage-before.png)

### 5. Password aging policy - after
![Password aging after](images/05-chage-after.png)

### 6. Weak password cracked (John the Ripper)
![John the Ripper weak password](images/06-john-weak-password.png)

> Note: if any image above does not render, check that the corresponding filename inside the `images/` folder exactly matches the path referenced here (case-sensitive, no duplicated extensions).

## Key Commands

```bash
# Package audit
sudo dpkg -l | sudo tee /root/documents/auditoria.txt > /dev/null

# Password aging policy
sudo chage -M 90 -W 7 -I 14 usertest
sudo chage -l usertest

# Weak password test
sudo unshadow /etc/passwd /etc/shadow > /tmp/senhas.txt
sudo john /tmp/senhas.txt
```

## Notes

- This lab environment (AWS EC2 instance) has been terminated after the exercise to avoid unnecessary cloud costs.
- SHA-512 password hashing was temporarily enabled (instead of the Ubuntu default yescrypt) to allow compatibility with John the Ripper (core edition) for demonstration purposes only.
