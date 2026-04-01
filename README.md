# CIPP Skill

A comprehensive [AgentSkill](https://docs.openclaw.ai) for **CIPP** (CyberDrain Improved Partner Portal) — the open-source MSP management platform for Microsoft 365 and Azure tenant administration.

## What This Is

An AI skill that gives Claude, OpenClaw, or any AgentSkills-compatible system deep knowledge of CIPP's full capabilities — both API automation and UI workflows. When loaded, the agent can:

- **Automate CIPP operations** via the REST API (~250 endpoints covered)
- **Guide users through UI workflows** when an API endpoint doesn't exist
- **Answer "can CIPP do X?" questions** definitively, with exact steps
- **Build n8n / Power Automate workflows** that integrate with CIPP
- **Help with CIPP setup**, troubleshooting, and configuration

## Coverage

| Area | Reference File | Endpoints |
|------|---------------|-----------|
| **Auth & Setup** | `auth-setup.md` | OAuth2 flow, shell helpers, n8n integration, PowerShell module |
| **Users** | `identity-users.md` | 30+ endpoints — CRUD, password reset, MFA, offboarding, BEC, JIT admin |
| **Groups** | `identity-groups.md` | Group CRUD, templates, Teams conversion, membership |
| **Devices** | `identity-devices.md` | Azure AD device management, delete/restore |
| **Email & Exchange** | `email-exchange.md` | Mailboxes, permissions, forwarding, quarantine, spam, transport rules, contacts |
| **Intune / MEM** | `endpoint-intune.md` | Policies, compliance, device actions, scripts, apps, Defender |
| **Autopilot** | `endpoint-autopilot.md` | AP devices, profiles, enrollment, DEP sync |
| **Tenant Admin** | `tenant-administration.md` | Tenant CRUD, GDAP, domains, alerts, audit logs, backup |
| **Conditional Access** | `tenant-conditional-access.md` | CA policies, templates, named locations, what-if testing |
| **Security** | `security.md` | Incidents, alerts, MDO, Safe Links, phishing checks |
| **Standards** | `standards-compliance.md` | BPA, domain analyser, drift detection, standards deployment |
| **Teams & SharePoint** | `teams-sharepoint.md` | Teams, SharePoint sites, voice, permissions |
| **Extensions** | `extensions-integrations.md` | HaloPSA, NinjaOne, Hudu, Sherweb, scheduler, custom roles |

Every capability is marked **[API]**, **[UI]**, or **[API + UI]** so the agent knows exactly what's automatable vs what needs manual UI steps.

## Installation

### OpenClaw

Copy the skill folder into your skills directory:

```bash
# Clone into your skills folder
git clone https://github.com/mrsmickers/cipp-skill.git ~/clawd/skills/cipp

# Or symlink if you prefer
git clone https://github.com/mrsmickers/cipp-skill.git ~/cipp-skill
ln -s ~/cipp-skill ~/clawd/skills/cipp
```

OpenClaw will automatically detect the skill from the `SKILL.md` frontmatter.

### Claude Code / Other AgentSkills Systems

Place the folder anywhere your agent can read it, and point your skill configuration at the `SKILL.md` file.

### Manual / Standalone

The reference files are plain Markdown — they're useful as documentation even without an AI agent. Browse the `references/` folder for any CIPP topic.

## Universal & Credential-Free

This skill contains **no hardcoded credentials, URLs, or identifying information**. All examples use placeholders (`contoso.onmicrosoft.com`, `${CIPP_URL}`, etc.). Configure your own CIPP connection details via environment variables or your agent's secret management.

## CIPP API Quick Start

```bash
# 1. Get OAuth2 token
TOKEN=$(curl -s -X POST "https://login.microsoftonline.com/${TENANT_ID}/oauth2/v2.0/token" \
  -d "client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&scope=api://${CLIENT_ID}/.default&grant_type=client_credentials" \
  | jq -r '.access_token')

# 2. List tenants
curl -s -X POST -H "Authorization: Bearer $TOKEN" "${CIPP_URL}/api/ListTenants"

# 3. List users for a tenant
curl -s -H "Authorization: Bearer $TOKEN" "${CIPP_URL}/api/ListUsers?TenantFilter=contoso.onmicrosoft.com"
```

See `references/auth-setup.md` for full auth details, shell helpers, and n8n integration patterns.

## Contributing

PRs welcome — especially for:
- New CIPP endpoints as they're added
- UI workflow documentation improvements
- Additional integration patterns (Power Automate, Zapier, etc.)
- Corrections or clarifications

## Links

- **CIPP Docs:** https://docs.cipp.app
- **CIPP GitHub:** https://github.com/KelvinTegelaar/CIPP
- **CIPP API GitHub:** https://github.com/KelvinTegelaar/CIPP-API
- **OpenClaw:** https://docs.openclaw.ai
- **AgentSkills Spec:** https://docs.openclaw.ai/skills

## License

MIT
