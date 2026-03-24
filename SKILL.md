---
name: filo-email
version: 1.1.0
description: Set up and manage email via IMAP/SMTP. Supports Gmail, Outlook, Yahoo, iCloud, FastMail, ProtonMail, Zoho, QQ Mail, 163 Mail, 126 Mail, Sina Mail, 139 Mail, Aliyun Mail, Tencent Exmail, GMX, Web.de, T-Online, Orange, Free.fr, Libero, Mail.ru, Posteo, Mailbox.org, AOL, Comcast, AT&T, and more. Guides through auth setup and verifies connection. Use when the user wants to connect an email account, read emails, send emails, or troubleshoot IMAP access.
homepage: https://github.com/FiloAI
metadata: {"emoji":"📮"}
---

Connect email accounts to the terminal. Works with every major email provider worldwide.

When invoked, walk the user through every step in order. Do not skip steps.

---

## Provider Registry

Single source of truth for all supported providers.

**To add a new provider**: add one row to this table, one auth section in Step 3, and one config block in Step 4. No other changes needed.

### China

| ID | Provider | Domains | IMAP | SMTP | Auth |
|----|----------|---------|------|------|------|
| CN1 | QQ Mail | qq.com | imap.qq.com:993 TLS | smtp.qq.com:465 TLS | Authorization code |
| CN2 | 163 Mail | 163.com | imap.163.com:993 TLS | smtp.163.com:465 TLS | Authorization code |
| CN3 | 126 Mail | 126.com | imap.126.com:993 TLS | smtp.126.com:465 TLS | Authorization code |
| CN4 | Sina Mail | sina.com | imap.sina.com:993 TLS | smtp.sina.com:465 TLS | Authorization code |
| CN5 | 139 Mail | 139.com | imap.139.com:993 TLS | smtp.139.com:465 TLS | Authorization code |
| CN6 | Aliyun Mail | aliyun.com | imap.aliyun.com:993 TLS | smtp.aliyun.com:465 TLS | Regular password |
| CN7 | Tencent Exmail | exmail.qq.com | imap.exmail.qq.com:993 TLS | smtp.exmail.qq.com:465 TLS | Client password (if Secure Login on) |

### USA & Global

| ID | Provider | Domains | IMAP | SMTP | Auth |
|----|----------|---------|------|------|------|
| US4 | Yahoo Mail | yahoo.com, ymail.com | imap.mail.yahoo.com:993 TLS | smtp.mail.yahoo.com:465 TLS | App Password |
| US6 | AOL Mail | aol.com | imap.aol.com:993 TLS | smtp.aol.com:465 TLS | App Password (always required) |
| US7 | AT&T Mail | att.net | imap.mail.att.net:993 TLS | smtp.mail.att.net:465 TLS | Secure Mail Key |
| US8 | Comcast / Xfinity | comcast.net | imap.comcast.net:993 TLS | smtp.comcast.net:587 STARTTLS | Regular password (3rd-party access must be enabled) |
| US9 | Verizon | verizon.net | imap.aol.com:993 TLS | smtp.aol.com:465 TLS | AOL App Password (migrated to AOL) |
| US10 | Cox Mail | cox.net | imap.mail.yahoo.com:993 TLS | smtp.mail.yahoo.com:465 TLS | Yahoo App Password (migrated to Yahoo) |
| US11 | FastMail | fastmail.com, fastmail.fm | imap.fastmail.com:993 TLS | smtp.fastmail.com:465 TLS | Regular or App Password |

### Europe

| ID | Provider | Domains | IMAP | SMTP | Auth |
|----|----------|---------|------|------|------|
| EU1 | GMX | gmx.com, gmx.de, gmx.net | imap.gmx.com:993 TLS | mail.gmx.com:587 STARTTLS | Regular password (IMAP must be enabled first) |
| EU2 | Web.de | web.de | imap.web.de:993 TLS | smtp.web.de:587 STARTTLS | Regular password (IMAP must be enabled first) |
| EU3 | T-Online / Telekom | t-online.de | secureimap.t-online.de:993 TLS | securesmtp.t-online.de:465 TLS | Email-program password (NOT main Telekom login) |
| EU4 | Orange France | orange.fr, wanadoo.fr | imap.orange.fr:993 TLS | smtp.orange.fr:465 TLS | Regular password |
| EU5 | Free.fr | free.fr | imap.free.fr:993 TLS | smtp.free.fr:587 STARTTLS | Regular password |
| EU6 | Libero (Italy) | libero.it | imapmail.libero.it:993 TLS | smtp.libero.it:587 STARTTLS | Regular password |
| EU7 | Mail.ru / VK Mail | mail.ru, bk.ru, inbox.ru | imap.mail.ru:993 TLS | smtp.mail.ru:465 TLS | Regular password; App Password if 2FA on |
| EU8 | Posteo | posteo.de | posteo.de:993 TLS | posteo.de:587 STARTTLS | Regular password |
| EU9 | Mailbox.org | mailbox.org | imap.mailbox.org:993 TLS | smtp.mailbox.org:465 TLS | Regular password; App Password if 2FA on |
| EU10 | ProtonMail (via Bridge) | proton.me, protonmail.com | 127.0.0.1:1143 none | 127.0.0.1:1025 none | Bridge password (not Proton account password) |
| EU11 | Zoho Mail | zoho.com, zohomail.com | imap.zoho.com:993 TLS | smtp.zoho.com:465 TLS | App Password |

### India

| ID | Provider | Domains | IMAP | SMTP | Auth |
|----|----------|---------|------|------|------|
| IN1 | Rediffmail Pro | rediffmail.com | imap.rediffmailpro.com:993 TLS | smtp.rediffmail.com:465 TLS | Regular password (Pro paid accounts only — free accounts do NOT support IMAP) |

---

## Step 1 — Check filo-mail

```bash
filo-mail --version
```

If not installed, the skill package includes a `filo-mail` wrapper. Install it:

```bash
curl -fsSL https://raw.githubusercontent.com/nanaco666/filo-email-skill/main/filo-mail \
  -o /usr/local/bin/filo-mail && chmod +x /usr/local/bin/filo-mail
```

Run `filo-mail --version` again to confirm.

---

## Step 2 — Choose Provider

Ask the user which email provider they want to connect. Present the table above grouped by region, or ask for their email address and auto-detect from the domain.

Domain auto-detection:
- `@qq.com` → CN1
- `@163.com` → CN2
- `@126.com` → CN3
- `@sina.com` / `@sina.cn` → CN4
- `@139.com` → CN5
- `@aliyun.com` → CN6
- `@exmail.qq.com` or custom domain via Tencent Exmail → CN7
- `@yahoo.com` / `@ymail.com` → US4
- `@aol.com` → US6
- `@att.net` → US7
- `@comcast.net` → US8
- `@verizon.net` → US9
- `@cox.net` → US10
- `@fastmail.com` / `@fastmail.fm` → US11
- `@gmx.com` / `@gmx.de` / `@gmx.net` → EU1
- `@web.de` → EU2
- `@t-online.de` → EU3
- `@orange.fr` / `@wanadoo.fr` → EU4
- `@free.fr` → EU5
- `@libero.it` → EU6
- `@mail.ru` / `@bk.ru` / `@inbox.ru` → EU7
- `@posteo.de` → EU8
- `@mailbox.org` → EU9
- `@proton.me` / `@protonmail.com` → EU10
- `@zoho.com` → EU11
- `@rediffmail.com` → IN1
- anything else → custom IMAP

---

## Step 3 — Auth Setup

Each provider has different authentication requirements. Follow the section for the detected provider.

---

### CN1 — QQ Mail

QQ 邮箱使用**授权码**，不能用 QQ 密码登录。

1. 登录网页端：https://mail.qq.com
2. 右上角「设置」→「账户」
3. 找到「POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV 服务」
4. 开启「IMAP/SMTP 服务」（需手机短信验证）
5. 点「生成授权码」→ 短信验证后显示 16 位授权码
6. 复制授权码（只显示一次）

授权码生成页：https://mail.qq.com → 设置 → 账户 → POP3/IMAP/SMTP

> ⚠️ 普通 QQ 密码无法用于 IMAP 登录，必须使用授权码。

---

### CN2 — 163 Mail

1. 登录：https://mail.163.com
2. 设置 → POP3/SMTP/IMAP
3. 开启 IMAP/SMTP 服务
4. 生成授权码（16 位）

授权码生成页：https://mail.163.com → 设置 → POP3/SMTP/IMAP

> ⚠️ 可以为不同客户端生成多个授权码。普通密码无效。

---

### CN3 — 126 Mail

1. 登录：https://mail.126.com
2. 设置 → POP3/SMTP/IMAP
3. 开启 IMAP 服务 → 生成授权码

授权码生成页：https://mail.126.com → 设置 → POP3/SMTP/IMAP

---

### CN4 — Sina Mail

1. 登录：https://mail.sina.com.cn
2. 设置 → 客户端 POP/IMAP/SMTP
3. 开启服务 → 生成「客户端授权码」

授权码生成页：https://mail.sina.com.cn → 设置 → 客户端 POP/IMAP/SMTP

---

### CN5 — 139 Mail (China Mobile)

1. 登录：https://mail.10086.cn
2. 设置 → 客户端协议管理
3. 开启 IMAP/SMTP → 生成授权码（可能需要安全问题验证）

授权码生成页：https://mail.10086.cn → 设置 → 客户端协议管理

---

### CN6 — Aliyun Mail

个人 @aliyun.com 账号默认可使用普通登录密码。
若管理员启用了「第三方客户端专用密码」，则需要在账户设置中生成专用密码：
https://mail.aliyun.com → 设置 → 账户 → 第三方客户端安全密码

---

### CN7 — Tencent Exmail

若企业管理员开启了「安全登录」，需生成专用客户端密码：
https://exmail.qq.com → 设置 → 绑定邮箱 → 开启安全登录 → 新建密码

若「安全登录」未开启，直接使用邮箱登录密码即可。

---

### US4 — Yahoo Mail

Yahoo requires an **App Password**.

1. Open: https://login.yahoo.com/myaccount/security/
2. Click **Generate app password** → Select Other → name it `filo-mail`
3. Copy the generated password

---

### US6 — AOL Mail

AOL **always** requires an App Password regardless of 2FA status.

1. Open: https://login.aol.com/account/security
2. Scroll to "Generate app password" → Select app: Other → name it `filo-mail`
3. Copy the generated password (shown once)

> ⚠️ Regular AOL password will always fail for IMAP. No exceptions.

---

### US7 — AT&T Mail

AT&T uses a "Secure Mail Key" (equivalent to App Password).

1. Open: https://www.att.com/myatt/
2. Sign in → Profile → Sign-in info → **Manage Secure Mail Key**
3. Generate a new key → copy it

> ⚠️ Use AT&T-specific server addresses (imap.mail.att.net), NOT Yahoo servers, even though the backend is Yahoo.

---

### US8 — Comcast / Xfinity

Third-party access is **disabled by default**.

1. Open: https://connect.xfinity.com (sign in)
2. Gear icon → Settings → Security
3. Check **"Third Party Access Security"** → Save
4. Use your regular Xfinity password for IMAP

---

### US9 — Verizon

Verizon email migrated to AOL in 2017. Uses AOL servers and AOL App Password.
Follow US6 (AOL) instructions, using your verizon.net address to log in at https://login.aol.com/account/security

---

### US10 — Cox Mail

Cox email migrated to Yahoo. Uses Yahoo servers and Yahoo App Password.
Follow US4 (Yahoo) instructions, using your cox.net address.

---

### US11 — FastMail

FastMail supports regular password, but App Password is recommended:

1. Open: https://app.fastmail.com/settings/security/passwords/new
2. Access: IMAP/POP/SMTP → name it `filo-mail`
3. Copy the generated password

---

### EU1 — GMX

GMX disables IMAP by default.

1. Open: https://www.gmx.com → Settings → POP3 & IMAP
2. Check **"Access IMAP"** and **"Access POP3"** → Save
3. Use your regular GMX password

> ⚠️ GMX will auto-disable IMAP again if not used for an extended period. Re-enable if you get auth errors after a break.

---

### EU2 — Web.de

Same as GMX — IMAP disabled by default.

1. Open: https://www.web.de → Settings → POP3 & IMAP
2. Enable IMAP access → Save
3. Use regular Web.de password

---

### EU3 — T-Online / Telekom Mail

T-Online requires a **separate email-program password** — the main Telekom account password will NOT work.

1. Open: https://email.t-online.de
2. Gear icon → All Settings → Account Details
3. Find **"Passwort für E-Mail-Programme"** (Password for email programs)
4. Set a password specifically for email clients → Save

> ⚠️ This is one of the most common setup failures. The error is always "authentication failed" and the fix is always this separate password.

---

### EU4 — Orange France

Standard setup. Regular password works directly.
No special steps required.

---

### EU5 — Free.fr

Standard setup. Regular password works.
Use SMTP port 587 (STARTTLS), not 465.

---

### EU6 — Libero (Italy)

Standard setup. Regular password works.
Note: IMAP hostname is `imapmail.libero.it` (not `imap.libero.it`).

---

### EU7 — Mail.ru / VK Mail

- Without 2FA: regular password works
- With 2FA: generate App Password at https://account.mail.ru → Security → Password for external applications → Add

---

### EU8 — Posteo

Standard setup. Regular password works directly.
Note: IMAP/SMTP hostname is simply `posteo.de` (no subdomain prefix).

---

### EU9 — Mailbox.org

- Without 2FA: regular password works
- With 2FA: generate Mail App Password at https://mailbox.org → All Settings → Security → Mail App Passwords → Create

---

### EU10 — ProtonMail (via Bridge)

ProtonMail requires the **Proton Mail Bridge** app running locally.

Check if Bridge is running:
```bash
protonmail-bridge --version 2>/dev/null || proton-mail-bridge --version 2>/dev/null || echo "NOT_RUNNING"
```

If not installed: https://proton.me/mail/bridge → download and start the desktop app.

Once Bridge is running:
- Open Bridge → find the **Bridge password** (NOT your Proton account password)
- IMAP and SMTP connect to localhost — Bridge handles encryption

---

### EU11 — Zoho Mail

Zoho requires an App Password if 2FA is enabled:

1. Open: https://accounts.zoho.com/home#security/app-specific-passwords
2. Generate new password for `filo-mail`
3. Without 2FA, regular password works

---

### IN1 — Rediffmail

⚠️ Free Rediffmail accounts do **not** support IMAP — POP3 only.
IMAP is exclusively available on **Rediffmail Pro** (paid).

If the user has a Pro account:
- Login: full email address
- Password: regular Rediffmail password

If the user has a free account: inform them IMAP is unavailable and suggest Gmail/Outlook alternatives.

---

### Custom IMAP

Ask the user for:
- IMAP host and port
- SMTP host and port
- Username (usually full email address)
- Password or auth method

---

## Step 4 — Write Config

Check existing config:
```bash
cat ~/.config/himalaya/config.toml 2>/dev/null || echo "NO_CONFIG"
```

If config exists, ask: add a new account or overwrite?

Create directory if needed:
```bash
mkdir -p ~/.config/himalaya
```

Write the config block for the detected provider. Replace `USER@...` and `PASSWORD_HERE` with actual values.

---

### CN1 — QQ Mail
```toml
[accounts.qq]
email = "USER@qq.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.qq.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@qq.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'AUTHORIZATION_CODE_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.qq.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@qq.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'AUTHORIZATION_CODE_HERE'"
```

### CN2 — 163 Mail
```toml
[accounts.netease163]
email = "USER@163.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.163.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@163.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'AUTHORIZATION_CODE_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.163.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@163.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'AUTHORIZATION_CODE_HERE'"
```

### CN3 — 126 Mail
```toml
[accounts.netease126]
email = "USER@126.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.126.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@126.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'AUTHORIZATION_CODE_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.126.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@126.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'AUTHORIZATION_CODE_HERE'"
```

### CN4 — Sina Mail
```toml
[accounts.sina]
email = "USER@sina.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.sina.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@sina.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'AUTHORIZATION_CODE_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.sina.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@sina.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'AUTHORIZATION_CODE_HERE'"
```

### CN5 — 139 Mail
```toml
[accounts.china-mobile]
email = "USER@139.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.139.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@139.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'AUTHORIZATION_CODE_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.139.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@139.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'AUTHORIZATION_CODE_HERE'"
```

### CN6 — Aliyun Mail
```toml
[accounts.aliyun]
email = "USER@aliyun.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.aliyun.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@aliyun.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.aliyun.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@aliyun.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### CN7 — Tencent Exmail
```toml
[accounts.exmail]
email = "USER@yourcompany.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.exmail.qq.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@yourcompany.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'CLIENT_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.exmail.qq.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@yourcompany.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'CLIENT_PASSWORD_HERE'"
```

### US4 — Yahoo Mail
```toml
[accounts.yahoo]
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

### US6 — AOL Mail
```toml
[accounts.aol]
email = "USER@aol.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.aol.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@aol.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'APP_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.aol.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@aol.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'APP_PASSWORD_HERE'"
```

### US7 — AT&T Mail
```toml
[accounts.att]
email = "USER@att.net"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.mail.att.net"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@att.net"
backend.auth.type = "password"
backend.auth.cmd = "echo 'SECURE_MAIL_KEY_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.mail.att.net"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@att.net"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'SECURE_MAIL_KEY_HERE'"
```

### US8 — Comcast / Xfinity
```toml
[accounts.comcast]
email = "USER@comcast.net"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.comcast.net"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@comcast.net"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.comcast.net"
message.send.backend.port = 587
message.send.backend.encryption.type = "start-tls"
message.send.backend.login = "USER@comcast.net"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### US9 — Verizon (uses AOL servers)
```toml
[accounts.verizon]
email = "USER@verizon.net"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.aol.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@verizon.net"
backend.auth.type = "password"
backend.auth.cmd = "echo 'AOL_APP_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.aol.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@verizon.net"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'AOL_APP_PASSWORD_HERE'"
```

### US10 — Cox Mail (uses Yahoo servers)
```toml
[accounts.cox]
email = "USER@cox.net"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.mail.yahoo.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@cox.net"
backend.auth.type = "password"
backend.auth.cmd = "echo 'YAHOO_APP_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.mail.yahoo.com"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@cox.net"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'YAHOO_APP_PASSWORD_HERE'"
```

### US11 — FastMail
```toml
[accounts.fastmail]
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

### EU1 — GMX
```toml
[accounts.gmx]
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

### EU2 — Web.de
```toml
[accounts.webde]
email = "USER@web.de"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.web.de"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@web.de"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.web.de"
message.send.backend.port = 587
message.send.backend.encryption.type = "start-tls"
message.send.backend.login = "USER@web.de"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### EU3 — T-Online / Telekom Mail
```toml
[accounts.telekom]
email = "USER@t-online.de"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "secureimap.t-online.de"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@t-online.de"
backend.auth.type = "password"
backend.auth.cmd = "echo 'EMAIL_PROGRAM_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "securesmtp.t-online.de"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@t-online.de"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'EMAIL_PROGRAM_PASSWORD_HERE'"
```

### EU4 — Orange France
```toml
[accounts.orange]
email = "USER@orange.fr"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.orange.fr"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@orange.fr"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.orange.fr"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@orange.fr"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### EU5 — Free.fr
```toml
[accounts.freefr]
email = "USER@free.fr"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.free.fr"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@free.fr"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.free.fr"
message.send.backend.port = 587
message.send.backend.encryption.type = "start-tls"
message.send.backend.login = "USER@free.fr"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### EU6 — Libero (Italy)
```toml
[accounts.libero]
email = "USER@libero.it"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imapmail.libero.it"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@libero.it"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.libero.it"
message.send.backend.port = 587
message.send.backend.encryption.type = "start-tls"
message.send.backend.login = "USER@libero.it"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### EU7 — Mail.ru / VK Mail
```toml
[accounts.mailru]
email = "USER@mail.ru"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.mail.ru"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@mail.ru"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_OR_APP_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.mail.ru"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@mail.ru"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_OR_APP_PASSWORD_HERE'"
```

### EU8 — Posteo
```toml
[accounts.posteo]
email = "USER@posteo.de"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "posteo.de"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@posteo.de"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "posteo.de"
message.send.backend.port = 587
message.send.backend.encryption.type = "start-tls"
message.send.backend.login = "USER@posteo.de"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

### EU9 — Mailbox.org
```toml
[accounts.mailboxorg]
email = "USER@mailbox.org"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "imap.mailbox.org"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@mailbox.org"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_OR_APP_PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.mailbox.org"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@mailbox.org"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_OR_APP_PASSWORD_HERE'"
```

### EU10 — ProtonMail (via Bridge)
```toml
[accounts.protonmail]
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

### EU11 — Zoho Mail
```toml
[accounts.zoho]
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

### Custom IMAP
```toml
[accounts.custom]
email = "USER@YOURDOMAIN.com"
display-name = "Your Name"
default = true

backend.type = "imap"
backend.host = "IMAP_HOST"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "USER@YOURDOMAIN.com"
backend.auth.type = "password"
backend.auth.cmd = "echo 'PASSWORD_HERE'"

message.send.backend.type = "smtp"
message.send.backend.host = "SMTP_HOST"
message.send.backend.port = 465
message.send.backend.encryption.type = "tls"
message.send.backend.login = "USER@YOURDOMAIN.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "echo 'PASSWORD_HERE'"
```

> **Security tip**: Replace `echo 'password'` with a keychain command to avoid storing passwords in plain text:
> - macOS: `security find-generic-password -s filo-mail-imap -w`
> - Linux (pass): `pass show email/imap`

---

## Step 5 — Verify Connection

```bash
filo-mail envelope list
```

Expected: a list of emails from INBOX.

**Troubleshooting by error:**

| Error | Likely cause | Fix |
|-------|-------------|-----|
| `authentication failed` | Wrong password/code | Re-check Step 3 |
| `authentication failed` on CN1-CN5 | Used regular password | Must use authorization code |
| `authentication failed` on EU3 | Used main Telekom login | Set email-program password (Step 3 EU3) |
| `authentication failed` on US6/US7 | Used regular password | App Password / Secure Mail Key required |
| `connection refused` | Wrong host or port | Double-check Provider Registry table |
| `IMAP access is disabled` on EU1/EU2 | GMX/Web.de IMAP off | Settings → POP3 & IMAP → Enable |
| `IMAP not available` on US8 | Third-party access off | Xfinity Settings → Security → Third Party Access Security |
| `IMAP not available` on IN1 | Free Rediffmail | IMAP only on Rediffmail Pro (paid) |
| ProtonMail connection refused | Bridge not running | Start Proton Mail Bridge desktop app |

---

## Operations Reference

### Reading & Listing

```bash
# List inbox
filo-mail envelope list
filo-mail envelope list --folder Sent
filo-mail envelope list --page 2 --page-size 50

# Search — full query DSL
filo-mail envelope list from alice@example.com
filo-mail envelope list subject "invoice"
filo-mail envelope list to me@example.com
filo-mail envelope list body "payment"
filo-mail envelope list flag seen                          # only read emails
filo-mail envelope list date 2026-01-01                    # exact date
filo-mail envelope list after 2026-01-01                   # newer than
filo-mail envelope list before 2026-03-01                  # older than
filo-mail envelope list from alice subject meeting         # AND (implicit)
filo-mail envelope list "from alice or from bob"           # OR
filo-mail envelope list "not flag seen"                    # NOT
filo-mail envelope list "order by date desc"               # sort
filo-mail envelope list "from alice order by subject asc"  # search + sort

# Thread view
filo-mail envelope thread                                  # inbox as threads
filo-mail envelope thread subject "project"                # search threads

# Read a message
filo-mail message read 42
filo-mail message read 42 --preview                        # read without marking as seen
filo-mail message read 42 --no-headers                     # body only
filo-mail message read 42 -H From -H Subject               # specific headers only

# Read a full thread
filo-mail message thread 42                                # all messages in thread
filo-mail message thread 42 --preview

# Export raw MIME
filo-mail message export 42 --full                         # full raw MIME to stdout
filo-mail message export 42 --destination /tmp/email.eml   # save to file
filo-mail message export 42 --full --open                  # export + auto-open
```

### Composing & Sending

```bash
filo-mail message write                          # compose (opens $EDITOR)
filo-mail message reply 42                       # reply
filo-mail message reply 42 --all                 # reply-all
filo-mail message forward 42                     # forward
filo-mail message edit 42                        # edit existing/draft message

# Open a mailto: link directly
filo-mail message mailto "mailto:alice@example.com?subject=Hello"

# Send raw message from stdin
cat email.eml | filo-mail message send

# Save message to folder without sending
cat email.eml | filo-mail message save --folder Drafts

# Template workflow (MML syntax)
filo-mail template write                         # compose template
filo-mail template reply 42                      # reply template
filo-mail template forward 42                    # forward template
filo-mail template save                          # save as draft
filo-mail template send                          # send directly from template
```

### Folders

```bash
filo-mail folder list                            # list all folders
filo-mail folder add "MyFolder"                  # create folder
filo-mail folder expunge                         # permanently remove deleted messages
filo-mail folder purge --yes                     # delete ALL messages in folder
filo-mail folder delete --yes                    # delete the folder itself
```

### Flags

```bash
filo-mail flag add 42 --flag seen                # mark as read
filo-mail flag add 42 --flag flagged             # star/flag
filo-mail flag remove 42 --flag seen             # mark as unread
filo-mail flag set 42 seen flagged               # replace all flags (overwrite)
```

### Attachments

```bash
filo-mail attachment download 42
filo-mail attachment download 42 --dir ~/Downloads
```

### Accounts

```bash
filo-mail account list
filo-mail account configure                      # interactive setup wizard
filo-mail account doctor                         # diagnose config issues
filo-mail account doctor --fix                   # auto-fix keyring / OAuth2 issues
filo-mail --account work envelope list           # switch account
```

### Output & Debugging

```bash
# Output formats
filo-mail envelope list --output json
filo-mail envelope list --output plain

# Global flags
filo-mail --quiet envelope list                  # silence all logs
filo-mail --debug envelope list                  # debug logs (= RUST_LOG=debug)
filo-mail --trace envelope list                  # trace + backtrace

# Environment variables
RUST_LOG=debug filo-mail envelope list
RUST_LOG=trace RUST_BACKTRACE=1 filo-mail envelope list
NO_COLOR=1 filo-mail envelope list               # disable colored output

# Shell completion
filo-mail completion bash >> ~/.bashrc
filo-mail completion zsh >> ~/.zshrc
filo-mail completion fish > ~/.config/fish/completions/filo-mail.fish
```

---

## Auth: OAuth 2.0

For providers that support OAuth2 (most modern providers), use the interactive wizard instead of plain password:

```bash
filo-mail account configure
```

The wizard supports **Thunderbird Autoconfiguration** — it can auto-discover IMAP/SMTP settings for your domain, then walk through the OAuth2 flow in the browser. No manual config file editing needed.

If OAuth2 breaks after token expiry:
```bash
filo-mail account doctor --fix
```

---

## PGP Encryption

Himalaya supports end-to-end encryption via PGP. Three backends available:

```toml
# Option 1: shell commands (gpg must be installed)
[accounts.personal.message.encrypt]
backend.type = "gpg"

# Option 2: native Rust (no gpg binary needed)
[accounts.personal.message.encrypt]
backend.type = "pgp"
backend.secret-key-path = "~/.config/himalaya/secret.pgp"

# Option 3: GPG bindings
[accounts.personal.message.encrypt]
backend.type = "gpg-native"
```

When composing, MML handles encryption/signing:
```
<#part encrypt=pgpmime sign=pgpmime>
Your secret message here.
<#/part>
```

---

## Useful Config Options

```toml
[accounts.personal]
# Where deleted messages go: move to Trash folder (default) or just set Deleted flag
message.delete.style = "folder"   # or "flag"

# Auto-save sent messages to Sent folder
message.send.save-copy = true

# Folder name aliases (provider-specific folder names)
folder.aliases.inbox = "INBOX"
folder.aliases.sent = "Sent Items"   # Outlook uses "Sent Items" not "Sent"
folder.aliases.trash = "Deleted"     # some providers use "Deleted"
folder.aliases.drafts = "Drafts"
```

---

## Add Another Account

To connect a second account, run this skill again. When Step 4 asks, choose "add to existing config" and use a different account alias (e.g. `work`, `personal2`).

Switch between accounts:
```bash
filo-mail --account gmail envelope list
filo-mail --account work message write
```
