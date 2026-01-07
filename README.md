# Laravel Forge Claude Code Plugin

A Claude Code plugin for managing Laravel Forge infrastructure directly from your terminal. Control servers, sites, deployments, databases, SSL certificates, and more through natural conversation with Claude.

**Zero dependencies** - uses only `curl` which is available on all systems.

## Features

- Full Laravel Forge API coverage via curl
- Manage servers, sites, and deployments
- Database and user management
- SSL certificate installation (Let's Encrypt)
- SSH key management
- Firewall rule configuration
- Scheduled jobs (cron)
- Background processes/daemons (queue workers)
- Redirect rules
- Remote command execution

## Usage

Just ask Claude to manage your Forge infrastructure:

```
> List all my servers
> Deploy the production site
> Show me the .env file for site 12345
> Create a new database called "myapp_db" on server 67890
> Install SSL certificate for example.com
> Add a cron job to run backups nightly
> Restart the queue workers
```

## Available Operations

| Category | Operations |
|----------|------------|
| **Servers** | List, get details, reboot, restart nginx/php/mysql |
| **Sites** | List, create, delete, get/update .env, get/update nginx config, logs |
| **Deployments** | Trigger, list history, get output, get/update deploy script |
| **Databases** | List, create, delete databases and users |
| **Domains & SSL** | List, add, delete domains; install/delete Let's Encrypt certificates |
| **SSH Keys** | List, add, delete keys |
| **Firewall** | List, add, delete rules |
| **Scheduled Jobs** | List, create, delete cron jobs |
| **Daemons** | List, create, delete, restart workers; get logs |
| **Redirects** | List, create, delete redirect rules |
| **Commands** | Run commands on sites, view history and output |

## Installation

### Via Plugin Marketplace

```bash
# Add the repository as a marketplace
/plugin marketplace add underwear/laravel-forge-claude-skill

# Install the plugin
/plugin install laravel-forge
```

### Manual Installation

```bash
# Clone the repository
git clone https://github.com/underwear/laravel-forge-claude-skill.git

# Copy to your Claude skills directory
mkdir -p ~/.claude/skills/laravel-forge
cp laravel-forge-claude-skill/skills/laravel-forge/SKILL.md ~/.claude/skills/laravel-forge/
```

## Setup

1. Generate a Laravel Forge API token at https://forge.laravel.com/profile/api

2. Find your organization slug - this is what you see in the URL when logged into Laravel Forge:
   - Personal account: `https://forge.laravel.com/{username}` → use your username
   - Organization: `https://forge.laravel.com/orgs/{org-slug}/servers` → use the org-slug

3. Configure credentials (choose one method):

### Option A: Claude Code Settings (Recommended)

Create `~/.claude/settings.local.json` for global access:

```json
{
  "env": {
    "FORGE_API_TOKEN": "your-api-token",
    "FORGE_ORG_SLUG": "your-organization-slug"
  }
}
```

Or create `.claude/settings.local.json` in your project directory for project-specific credentials.

> **Note:** Use `settings.local.json` for personal credentials (git-ignored), `settings.json` for team-shared non-sensitive config.

### Option B: Environment Variables

```bash
export FORGE_API_TOKEN="your-api-token"
export FORGE_ORG_SLUG="your-organization-slug"
```

Add to `~/.zshrc` or `~/.bashrc` for persistence.

## Requirements

- curl (pre-installed on macOS, Linux, and Windows with Git)
- Claude Code CLI
- Laravel Forge account with API access

## License

MIT

## Links

- [Laravel Forge](https://forge.laravel.com)
- [Forge API Documentation](https://forge.laravel.com/docs/api-reference/introduction)
- [Claude Code](https://claude.ai/code)
