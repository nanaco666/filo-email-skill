# filo-email

A Claude Code skill for connecting and managing email accounts via IMAP/SMTP.

Guides you through auth setup for every major provider, writes the config, and verifies the connection. Works anywhere Claude Code skills are supported.

## Providers

### China
| Provider | Domain |
|----------|--------|
| QQ Mail | qq.com |
| 163 Mail | 163.com |
| 126 Mail | 126.com |
| Sina Mail | sina.com |
| 139 Mail | 139.com |
| Aliyun Mail | aliyun.com |
| Tencent Exmail | exmail.qq.com |

### USA & Global
| Provider | Domain |
|----------|--------|
| Gmail | gmail.com |
| Outlook / Hotmail / Live | outlook.com, hotmail.com |
| Microsoft 365 | work/school accounts |
| Yahoo Mail | yahoo.com |
| iCloud Mail | icloud.com, me.com |
| AOL Mail | aol.com |
| AT&T Mail | att.net |
| Comcast / Xfinity | comcast.net |
| Verizon | verizon.net |
| Cox Mail | cox.net |
| FastMail | fastmail.com |

### Europe
| Provider | Domain |
|----------|--------|
| GMX | gmx.com, gmx.de |
| Web.de | web.de |
| T-Online / Telekom | t-online.de |
| Orange France | orange.fr |
| Free.fr | free.fr |
| Libero (Italy) | libero.it |
| Mail.ru / VK Mail | mail.ru |
| Posteo | posteo.de |
| Mailbox.org | mailbox.org |
| ProtonMail (via Bridge) | proton.me |
| Zoho Mail | zoho.com |

### India
| Provider | Domain |
|----------|--------|
| Rediffmail Pro | rediffmail.com |

Custom IMAP servers are also supported.

## What it does

- Auto-detects provider from your email address
- Guides through App Password / authorization code generation with direct links
- Writes `~/.config/himalaya/config.toml` for you
- Verifies the connection and explains errors
- Supports multiple accounts with aliases

## Install

```bash
# Install the filo-mail wrapper (auto-installs backend)
curl -fsSL https://raw.githubusercontent.com/nanaco666/filo-email-skill/main/filo-mail \
  -o /usr/local/bin/filo-mail && chmod +x /usr/local/bin/filo-mail
```

Then add the skill to your Claude Code config and invoke it:

```
/filo-email
```

## Usage

```bash
filo-mail envelope list                        # read inbox
filo-mail envelope list subject "invoice"      # search
filo-mail message read 42                      # read a message
filo-mail message reply 42                     # reply
filo-mail message write                        # compose
filo-mail --account work envelope list         # switch account
```

## Adding a new provider

Open `SKILL.md` and:
1. Add one row to the Provider Registry table
2. Add an auth section in Step 3
3. Add a config block in Step 4

No code changes needed.

## License

MIT
