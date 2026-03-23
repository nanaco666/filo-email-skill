---
name: filo-email
version: 1.0.0
description: Set up and manage email via IMAP/SMTP using Himalaya CLI. Supports Gmail, Outlook, Yahoo, iCloud, FastMail, ProtonMail, Zoho, and any custom IMAP server. Guides through App Password setup, multi-account config, and verifies connection. Use when you want to read, send, search, or organize email from the terminal.
homepage: https://github.com/FiloAI
metadata: {"clawdbot":{"emoji":"📮","requires":{"bins":["himalaya"]},"install":[{"id":"himalaya","kind":"brew","formula":"himalaya","bins":["himalaya"],"label":"Install Himalaya (brew)"}]}}
---

Connect your email accounts to the terminal. Himalaya supports IMAP/SMTP and works with every major email provider.

When invoked, walk the user through every step in order. Do not skip steps.

---

## Provider Registry

This is the single source of truth for all supported providers. To add a new provider in the future: add one row here and a matching config block + auth section in Steps 3–4.

| # | Provider | IMAP Host | IMAP Port | SMTP Host | SMTP Port | SMTP Enc | Auth Required |
|---|----------|-----------|-----------|-----------|-----------|----------|---------------|
| 1 | Gmail | imap.gmail.com | 993/TLS | smtp.gmail.com | 465/TLS | TLS | App Password |
| 2 | Outlook / Hotmail / Live | outlook.office365.com | 993/TLS | smtp.office365.com | 587/STARTTLS | STARTTLS | Regular or App Password |
| 3 | Microsoft 365 (work) | outlook.office365.com | 993/TLS | smtp.office365.com | 587/STARTTLS | STARTTLS | Regular or App Password |
| 4 | Yahoo Mail | imap.mail.yahoo.com | 993/TLS | smtp.mail.yahoo.com | 465/TLS | TLS | App Password |
| 5 | iCloud Mail | imap.mail.me.com | 993/TLS | smtp.mail.me.com | 587/STARTTLS | STARTTLS | App-Specific Password |
| 6 | FastMail | imap.fastmail.com | 993/TLS | smtp.fastmail.com | 465/TLS | TLS | Regular or App Password |
| 7 | ProtonMail (via Bridge) | 127.0.0.1 | 1143/none | 127.0.0.1 | 1025/none | none | Bridge Password |
| 8 | Zoho Mail | imap.zoho.com | 993/TLS | smtp.zoho.com | 465/TLS | TLS | App Password |
| 9 | GMX | imap.gmx.com | 993/TLS | mail.gmx.com | 587/STARTTLS | STARTTLS | Regular password |
| 10 | Web.de | imap.web.de | 993/TLS | smtp.web.de | 587/STARTTLS | STARTTLS | Regular password |
| 11 | Yandex Mail | imap.yandex.com | 993/TLS | smtp.yandex.com | 465/TLS | TLS | App Password |
| 12 | Namecheap / cPanel | mail.YOURDOMAIN.com | 993/TLS | mail.YOURDOMAIN.com | 465/TLS | TLS | Regular password |
| 13 | Custom IMAP | (user-supplied) | 993 | (user-supplied) | 465 | TLS | Regular password |

> **Adding a new provider**: Insert a row above, add an auth subsection in Step 3, and add a config block in Step 4. No code changes needed.

---

## Step 1 — Check Himalaya

```bash
himalaya --version
```

If not installed:
```bash
brew install himalaya
```

Verify again after install. If `brew` is unavailable, direct user to https://github.com/pimalaya/himalaya/releases for a prebuilt binary.

---

## Step 2 — Choose Provider

Ask the user which email provider they want to connect. Present these options:

1. **Gmail** (google.com, googlemail.com)
2. **Outlook / Hotmail / Live** (outlook.com, hotmail.com, live.com)
3. **Microsoft 365** (work/school accounts ending in custom domain via Microsoft)
4. **Yahoo Mail** (yahoo.com, ymail.com)
5. **iCloud Mail** (icloud.com, me.com, mac.com)
6. **FastMail** (fastmail.com, fastmail.fm)
7. **ProtonMail** (proton.me, protonmail.com) — requires Bridge
8. **Zoho Mail** (zoho.com, zohomail.com)
9. **Custom IMAP** — any other server

Note which option the user picks — it determines the next two steps.

---

## Step 3 — App Password / Auth Setup

Different providers require different auth setup before you can use IMAP. Follow the section for the chosen provider.

### Gmail

Gmail requires an **App Password** (regular password won't work if 2-Step Verification is on — and it should be).

1. Go to: https://myaccount.google.com/apppasswords
2. Sign in if prompted
3. Under "Select app" choose **Mail**, under "Select device" choose **Other** → name it `himalaya`
4. Click **Generate** → copy the 16-character password (shown once)

Keep this password — you'll need it in Step 4.

> If you see "App passwords" is missing: 2-Step Verification must be enabled first at https://myaccount.google.com/security

---

### Outlook / Hotmail / Live (personal accounts)

Personal Microsoft accounts support IMAP with your regular password **or** an App Password if you have 2FA.

- Without 2FA: use your regular Outlook password directly
- With 2FA (recommended): generate an App Password at https://account.microsoft.com/security → **Advanced security options** → **App passwords**

---

### Microsoft 365 (work/school)

Work/school accounts (e.g. `you@yourcompany.com` via Microsoft) may have IMAP disabled by the IT admin.

Ask the user to check with their admin or test with:
```
IMAP: outlook.office365.com:993 (TLS)
SMTP: smtp.office365.com:587 (STARTTLS)
```
Auth: use the work account password (or App Password if MFA is enforced).

---

### Yahoo Mail

Yahoo requires an **App Password**.

1. Go to: https://login.yahoo.com/myaccount/security/
2. Click **Generate app password** (under "Sign-in and security")
3. Select **Other app** → name it `himalaya` → Generate
4. Copy the password shown

---

### iCloud Mail

iCloud requires an **App-Specific Password**.

1. Go to: https://appleid.apple.com/account/manage
2. Sign in → under **Sign-In and Security** click **App-Specific Passwords**
3. Click **+** → name it `himalaya` → click Create
4. Copy the password shown (format: `xxxx-xxxx-xxxx-xxxx`)

---

### FastMail

FastMail supports standard IMAP with your account password, or you can use an **App Password** (recommended):

1. Go to: https://app.fastmail.com/settings/security/passwords/new
2. Set access to **IMAP/POP/SMTP** → name it `himalaya`
3. Copy the generated password

---

### ProtonMail

ProtonMail uses end-to-end encryption and **requires the Proton Mail Bridge** to expose a local IMAP interface.

Check if Bridge is running:
```bash
protonmail-bridge --version 2>/dev/null || proton-mail-bridge --version 2>/dev/null || echo "NOT_RUNNING"
```

If not installed: https://proton.me/mail/bridge → download and install the desktop app, then start it.

Once Bridge is running:
- IMAP: `127.0.0.1:1143` (no TLS — Bridge handles encryption internally)
- SMTP: `127.0.0.1:1025`
- Login: your Proton email address
- Password: the **Bridge password** (shown inside the Bridge app, NOT your Proton account password)

---

### Zoho Mail

Zoho supports IMAP with your regular password **or** an App Password if 2FA is on:

1. Go to: https://accounts.zoho.com/home#security/app-specific-passwords
2. Generate a new password for `himalaya`

---

### GMX / Web.de

Standard password authentication — no App Password needed. Just use your GMX/Web.de account password directly.

If IMAP access is blocked, enable it at: https://www.gmx.com/mail/settings → POP3/IMAP.

---

### Yandex Mail

Yandex requires an **App Password** when 2FA is enabled:

1. Go to: https://id.yandex.com/security/app-passwords
2. Create a new password for `himalaya`
3. Copy the generated password

If 2FA is off, your regular Yandex password works.

---

### Namecheap / cPanel Hosting

For domains hosted on cPanel (Namecheap, Bluehost, SiteGround, etc.):
- IMAP/SMTP host is typically `mail.YOURDOMAIN.com`
- Login is your full email address
- Password is the email account password set in cPanel
- Check the exact host in cPanel → Email → Email Accounts → Connect Devices

---

### Custom IMAP

Ask the user for:
- IMAP host and port (typically 993 with TLS, or 143 with STARTTLS)
- SMTP host and port (typically 465 with TLS, or 587 with STARTTLS)
- Username (usually the full email address)
- Password

---

## Step 4 — Write Config

Check if a config already exists:
```bash
cat ~/.config/himalaya/config.toml 2>/dev/null || echo "NO_CONFIG"
```

If it exists, ask whether to add a new account to it or overwrite.

Write (or append) the account block based on provider. Use the account alias `personal` for the first account, `work` for a second, or ask the user to name it.

### Gmail config block
```toml
[accounts.personal]
email = "USER@gmail.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.gmail.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@gmail.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'APP_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.gmail.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@gmail.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'APP_PASSWORD_HERE'"
```

### Outlook / Hotmail / Live config block
```toml
[accounts.personal]
email = "USER@outlook.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "outlook.office365.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@outlook.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.office365.com"
message.send.backend.port = 587
message.send.backend.encryption.type = "start-tls"
message.send.backend.login = "USER@outlook.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### Yahoo config block
```toml
[accounts.personal]
email = "USER@yahoo.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.mail.yahoo.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@yahoo.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'APP_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.mail.yahoo.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@yahoo.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'APP_PASSWORD_HERE'"
```

### iCloud config block
```toml
[accounts.personal]
email = "USER@icloud.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.mail.me.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@icloud.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'APP_SPECIFIC_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.mail.me.com"
message.send.backend.port = 587
message.send.backend.encryption.type = "start-tls"
message.send.backend.login = "USER@icloud.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'APP_SPECIFIC_PASSWORD_HERE'"
```

### FastMail config block
```toml
[accounts.personal]
email = "USER@fastmail.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.fastmail.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@fastmail.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.fastmail.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@fastmail.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### ProtonMail config block (via Bridge)
```toml
[accounts.personal]
email = "USER@proton.me"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "127.0.0.1"
backend.port = 1143
backend.encryption.type = "none"
backend.login = "USER@proton.me"
backend.auth.type = "password"
backend.auth.cmd = "echo 'BRIDGE_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "127.0.0.1"
message.send.backend.port = 1025
message.send.backend.encryption.type = "none"
message.send.backend.login = "USER@proton.me"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'BRIDGE_PASSWORD_HERE'"
```

### Zoho config block
```toml
[accounts.personal]
email = "USER@zoho.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.zoho.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@zoho.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'APP_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.zoho.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@zoho.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'APP_PASSWORD_HERE'"
```

### GMX config block
```toml
[accounts.personal]
email = "USER@gmx.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.gmx.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@gmx.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "mail.gmx.com"
message.send.backend.port = 587
message.send.backend.encryption.type = "start-tls"
message.send.backend.login = "USER@gmx.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### Yandex config block
```toml
[accounts.personal]
email = "USER@yandex.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.yandex.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@yandex.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'APP_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.yandex.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@yandex.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'APP_PASSWORD_HERE'"
```

### Namecheap / cPanel config block
```toml
[accounts.personal]
email = "USER@YOURDOMAIN.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "mail.YOURDOMAIN.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@YOURDOMAIN.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "mail.YOURDOMAIN.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@YOURDOMAIN.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### Custom IMAP config block
```toml
[accounts.personal]
email = "USER@example.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "IMAP_HOST"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@example.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "SMTP_HOST"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@example.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

> **Security note**: The `echo 'password'` form stores the password in plain text in the config file. For better security, use the system keychain:
> - macOS: `backend.auth.cmd = "security find-generic-password -s himalaya-imap -w"`
> - Linux (with `pass`): `backend.auth.cmd = "pass show email/imap"`

---

## Step 5 — Verify Connection

```bash
himalaya envelope list
```

Expected: a list of emails from your INBOX.

If the command fails:

| Error | Likely cause | Fix |
|-------|-------------|-----|
| `authentication failed` | Wrong password or App Password not yet saved | Re-check Step 3 |
| `connection refused` | Wrong host/port | Double-check provider settings |
| `certificate verify failed` | TLS issue | Try `backend.encryption.type = "start-tls"` instead |
| `IMAP access is disabled` | Provider setting | Enable IMAP in provider's account settings (Gmail: Settings → See all settings → Forwarding and POP/IMAP) |
| ProtonMail errors | Bridge not running | Start Proton Mail Bridge desktop app first |

---

## Common Operations

### Read emails
```bash
himalaya envelope list                          # INBOX, latest 10
himalaya envelope list --page 2                 # next page
himalaya envelope list --folder Sent            # another folder
himalaya envelope list --output json            # machine-readable
```

### Search
```bash
himalaya envelope list from alice@example.com
himalaya envelope list subject "invoice"
himalaya envelope list subject "meeting" from boss@company.com
```

### Read a message
```bash
himalaya message read 42
```

### Reply / Forward
```bash
himalaya message reply 42           # opens $EDITOR
himalaya message reply 42 --all     # reply-all
himalaya message forward 42
```

### Write a new email
```bash
himalaya message write              # interactive ($EDITOR)
```

Or send directly:
```bash
himalaya template send <<'EOF'
From: you@example.com
To: recipient@example.com
Subject: Hello

Message body here.
EOF
```

### Organize
```bash
himalaya message move 42 Archive
himalaya message copy 42 Important
himalaya message delete 42
himalaya flag add 42 --flag seen
himalaya flag remove 42 --flag seen
```

### Attachments
```bash
himalaya attachment download 42
himalaya attachment download 42 --dir ~/Downloads
```

### Multiple accounts
```bash
himalaya account list
himalaya --account work envelope list
himalaya --account personal message reply 42
```

---

## Add Another Account

To connect a second email account, run this skill again. When Step 4 asks, choose "add to existing config" — the new account block will be appended with a different alias (e.g. `work`).

---

## Troubleshooting

**Gmail: IMAP disabled**
Go to Gmail Settings → See all settings → Forwarding and POP/IMAP → Enable IMAP → Save.

**Gmail: "Less secure app" error**
This means you're using the regular password. Generate an App Password instead (Step 3).

**Outlook: `AUTHENTICATE failed`**
Modern auth may be required. Check if your admin allows IMAP Basic Auth at https://admin.microsoft.com.

**iCloud: `LOGIN failed`**
Make sure you're using the App-Specific Password, not your Apple ID password.

**ProtonMail: can't connect to 127.0.0.1**
The Bridge app must be open and logged in. Check its status in the system tray.

**`$EDITOR` not set**
```bash
export EDITOR=nano     # or vim, code, etc.
```
