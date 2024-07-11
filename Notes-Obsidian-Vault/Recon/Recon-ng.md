## Table of Contents


| Module                                         | Description                                                            | Usage                               |
| ---------------------------------------------- | ---------------------------------------------------------------------- | ----------------------------------- |
| `recon/domains-hosts/hackertarget`             | Find host names associated with a domain using HackerTarget API        | `hackertarget <domain>`             |
| `recon/domains-hosts/brute_hosts`              | Perform DNS brute forcing to discover host names                       | `brute_hosts <domain>`              |
| `recon/domains-hosts/google_site_web`          | Use Google search to find host names associated with a domain          | `google_site_web <domain>`          |
| `recon/domains-hosts/certificate_transparency` | Use Certificate Transparency logs to find host names                   | `certificate_transparency <domain>` |
| `recon/domains-vulnerabilities/xssed`          | Search XSSed database for potential XSS vulnerabilities                | `xssed <domain>`                    |
| `recon/domains-vulnerabilities/ghdb`           | Use Google Hacking Database to find potential vulnerabilities          | `ghdb <domain>`                     |
| `recon/hosts-ports/shodan_ip`                  | Use Shodan API to find open ports and services for a target IP         | `shodan_ip <ip_address>`            |
| `recon/domains-contacts/whois_pocs`            | Retrieve WHOIS information to find points of contact (POCs)            | `whois_pocs <domain>`               |
| `recon/hosts-hosts/resolve`                    | Perform DNS resolution to find IP addresses associated with host names | `resolve <host_name>`               |
| `recon/companies-multi/github_miner`           | Use GitHub API to find repositories and code associated with a company | `github_miner <company_name>`       |

General Recon-ng Commands:
- `workspaces create <name>`: Create a new workspace
- `workspaces list`: List available workspaces
- `workspaces load <name>`: Load a workspace
- `modules search <keyword>`: Search for modules by keyword
- `modules load <module>`: Load a specific module
- `info`: Display information about the loaded module
- `options set <option> <value>`: Set an option for the loaded module
- `run`: Execute the loaded module
- `back`: Go back to the main menu
- `exit`: Exit Recon-ng

