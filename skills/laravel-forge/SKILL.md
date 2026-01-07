---
name: laravel-forge
description: Complete Laravel Forge management via curl - servers, sites, deployments, databases, SSL, SSH keys, firewall, jobs, daemons, and more.
allowed-tools: Bash(curl:*)
---

# Laravel Forge Control

Manage Laravel Forge infrastructure using curl. No dependencies required - just curl (available on all systems).

## Setup

User must set environment variables:
```bash
export FORGE_API_TOKEN="your-token"      # From https://forge.laravel.com/profile/api
export FORGE_ORG_SLUG="your-org-slug"    # Your organization slug from Forge URL
```

## API Endpoints Reference

Base URL: `https://forge.laravel.com/api`

All requests require header: `Authorization: Bearer $FORGE_API_TOKEN`

---

### Servers

**List all servers:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers"
```

**Get server details:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}"
```

**Reboot server:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" -d '{"action":"reboot"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/actions"
```

**Restart Nginx:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" -d '{"action":"restart"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/nginx-action"
```

**Restart PHP-FPM:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" -d '{"action":"restart","version":"8.2"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/php-action"
```

**Restart MySQL:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" -d '{"action":"restart"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/mysql-action"
```

---

### Sites

**List all sites:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites"
```

**Get site details:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}"
```

**Create site:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"domain":"example.com","project_type":"laravel","directory":"/public"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/sites"
```

**Delete site:**
```bash
curl -s -X DELETE -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}"
```

**Get .env file:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}/env"
```

**Update .env file:**
```bash
curl -s -X PUT -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"content":"APP_ENV=production\nAPP_DEBUG=false"}' \
  "https://forge.laravel.com/api/sites/{site_id}/env"
```

**Get Nginx config:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}/nginx"
```

**Update Nginx config:**
```bash
curl -s -X PUT -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"content":"server { ... }"}' \
  "https://forge.laravel.com/api/sites/{site_id}/nginx"
```

**Get site logs:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}/logs?type=error"
```

---

### Deployments

**Trigger deployment:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}/deployments"
```

**List deployments:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}/deployments"
```

**Get deployment details:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/deployments/{deployment_id}"
```

**Get deployment output:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/deployments/{deployment_id}/output"
```

**Get deploy script:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}/deployment-script"
```

**Update deploy script:**
```bash
curl -s -X PUT -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"content":"cd /home/forge/site\ngit pull\ncomposer install --no-dev"}' \
  "https://forge.laravel.com/api/sites/{site_id}/deployment-script"
```

---

### Databases

**List databases:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/database/schemas"
```

**Create database:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" -d '{"name":"my_database"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/database/schemas"
```

**Delete database:**
```bash
curl -s -X DELETE -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/database/schemas/{database_id}"
```

**List database users:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/database/users"
```

**Create database user:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"dbuser","password":"secret123","databases":[1,2]}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/database/users"
```

**Delete database user:**
```bash
curl -s -X DELETE -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/database/users/{user_id}"
```

---

### Domains & SSL

**List domains:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}/domains"
```

**Add domain:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" -d '{"name":"www.example.com"}' \
  "https://forge.laravel.com/api/sites/{site_id}/domains"
```

**Delete domain:**
```bash
curl -s -X DELETE -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}/domains/{domain_id}"
```

**Get SSL certificate status:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}/domains/{domain_id}/certificate"
```

**Install Let's Encrypt SSL:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" -d '{"type":"letsencrypt"}' \
  "https://forge.laravel.com/api/sites/{site_id}/domains/{domain_id}/certificate"
```

**Delete SSL certificate:**
```bash
curl -s -X DELETE -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/sites/{site_id}/domains/{domain_id}/certificate"
```

---

### SSH Keys

**List SSH keys:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/ssh-keys"
```

**Add SSH key:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"My Key","key":"ssh-rsa AAAA..."}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/ssh-keys"
```

**Delete SSH key:**
```bash
curl -s -X DELETE -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/ssh-keys/{key_id}"
```

---

### Firewall Rules

**List firewall rules:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/firewall-rules"
```

**Add firewall rule:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"Custom Port","port":"8080","ip_address":null}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/firewall-rules"
```

**Add firewall rule with IP restriction:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"MySQL Restricted","port":"3306","ip_address":"10.0.0.1"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/firewall-rules"
```

**Delete firewall rule:**
```bash
curl -s -X DELETE -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/firewall-rules/{rule_id}"
```

---

### Scheduled Jobs (Cron)

**List scheduled jobs:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/scheduled-jobs"
```

**Create scheduled job:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"command":"php artisan schedule:run","frequency":"minutely","user":"forge"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/scheduled-jobs"
```

Frequency options: `minutely`, `hourly`, `nightly`, `weekly`, `monthly`, `custom`

**Create custom cron job:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"command":"php artisan backup:run","frequency":"custom","cron":"0 2 * * *","user":"forge"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/scheduled-jobs"
```

**Delete scheduled job:**
```bash
curl -s -X DELETE -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/scheduled-jobs/{job_id}"
```

---

### Daemons (Background Processes)

**List daemons:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/background-processes"
```

**Create daemon:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"command":"php artisan queue:work","user":"forge","processes":3}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/background-processes"
```

**Delete daemon:**
```bash
curl -s -X DELETE -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/background-processes/{daemon_id}"
```

**Restart daemon:**
```bash
curl -s -X PUT -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" -d '{"action":"restart"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/background-processes/{daemon_id}"
```

**Get daemon logs:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/background-processes/{daemon_id}/log"
```

---

### Redirect Rules

**List redirects:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/sites/{site_id}/redirect-rules"
```

**Create redirect (301 permanent):**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"from":"/old-page","to":"/new-page","type":"permanent"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/sites/{site_id}/redirect-rules"
```

**Create redirect (302 temporary):**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"from":"/promo","to":"/sale","type":"temporary"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/sites/{site_id}/redirect-rules"
```

**Delete redirect:**
```bash
curl -s -X DELETE -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/sites/{site_id}/redirect-rules/{redirect_id}"
```

---

### Command Execution

**Run command on site:**
```bash
curl -s -X POST -H "Authorization: Bearer $FORGE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"command":"php artisan migrate --force"}' \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/sites/{site_id}/commands"
```

**List command history:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/sites/{site_id}/commands"
```

**Get command output:**
```bash
curl -s -H "Authorization: Bearer $FORGE_API_TOKEN" \
  "https://forge.laravel.com/api/orgs/$FORGE_ORG_SLUG/servers/{server_id}/sites/{site_id}/commands/{command_id}/output"
```

---

## Usage Notes

1. **Always list first** - Use list commands to get IDs before other operations
2. **Confirm destructive actions** - Ask user before delete/reboot operations
3. **Replace placeholders** - Replace `{server_id}`, `{site_id}`, etc. with actual IDs
4. **Check response** - API returns JSON with operation status

## API Reference

Official docs: https://forge.laravel.com/docs/api-reference/introduction
