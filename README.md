# Windows SSH Keys (PowerShell) — Zero‑Confusion Guide 

This guide is **Windows‑only**, **PowerShell‑only**, and intentionally boring. It works.

---

## What this is

You need SSH keys to:

* Clone GitHub repos without passwords
* Push code securely
* SSH into remote servers (GPU / AI instances)

SSH keys are **cryptographic identity**, not magic.

---

## Step 0 — Open the correct terminal

* Open **PowerShell**
* **Run as Administrator**

You should see something like:

```
PS C:\Windows\system32>
```

---

## Step 1 — Generate an SSH key (DO NOT rename it)

Run:

```powershell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

When prompted:

* **File location** → press **Enter** (accept default)
* **Passphrase** → optional (Enter twice for none)

This creates:

```
~/.ssh/id_ed25519      (private key — NEVER SHARE)
~/.ssh/id_ed25519.pub  (public key — safe to share)
```

---

## Step 2 — Enable & start the SSH agent (Windows way)

> Linux/macOS uses `eval`. **Windows does NOT.**

Run these **exact commands**:

```powershell
Set-Service ssh-agent -StartupType Automatic
Start-Service ssh-agent
```

Verify:

```powershell
Get-Service ssh-agent
```

You want:

```
Status : Running
```

---

## Step 3 — Add your key to the agent

```powershell
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

Expected:

```
Identity added
```

---

## Step 4 — Copy your PUBLIC key to clipboard

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub | Set-Clipboard
```

✔️ Your public key is now copied

---

**That’s it. You now have cryptographic identity.**
