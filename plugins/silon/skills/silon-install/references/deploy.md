# Silon deployment commands

Substitute the user's actual hostname, email, and provider destination. Keep example values out of executed commands. Use an existing `silon` CLI where possible. If setup needs a bootstrap download, show the official URL to the user, obtain approval to install software in that environment, and download it to a file for review before running it. Both Silon bootstraps verify the checksums of their release downloads.

## New cloud VM

Use the cloud's authenticated CLI on a laptop or in its browser shell. The browser shells are [Google Cloud Shell](https://shell.cloud.google.com), [AWS CloudShell](https://console.aws.amazon.com/cloudshell), and [Azure Cloud Shell](https://shell.azure.com). If needed, the official CLI bootstrap is `https://get.silon.tech/go/install-cli.sh`. It installs to `~/.local/bin` for a regular user and `/usr/local/bin` for root. Add the applicable directory to `PATH` if needed.

The default VM has 2 vCPU, 8 GB RAM, and a 50 GB disk. The deployment creates a static IP and opens ports 80 and 443. Use `-project` and `-zone` for Google Cloud, `-region` for AWS, or `-location` and optionally `-resource-group` for Azure. Verify the active identity and subscription or AWS account in the provider CLI. For an example plan on Google Cloud, after the cost is approved, run the following with the user's values.

```sh
silon deploy gcp -project my-project -zone europe-west1-b \
  -host chat.example.com -email ops@example.com -agent-trial -yes -json
```

Use `aws` or `azure` and the corresponding provider flags for those clouds. For a purchased licence, omit `-agent-trial` and have the user supply `SILON_INSTALL_TOKEN` from their Silon licence page, or use `-approve` for browser approval. Without a domain, supply `-name` and omit `-host` and `-email`. The server then uses its IP and a temporary certificate. If the user has an external PostgreSQL 17 database, pass its connection URL through `SILON_DATABASE_URL` only after checking the VM can reach it. For a new Cloud SQL database on GCP, use `-database cloudsql` after confirming the extra cost.

When an agent runs a deploy over a short-lived shell connection, start it with `nohup`, redirect both output streams to a fresh log in a private directory, and follow that log. A normal run can take around ten minutes and longer with Cloud SQL. A `links` event includes the setup and activation links and can arrive before `done`. The DNS step shows the A record to create for a domain unless DNS was set up through `-dns-zone`. Use DNS-only mode for a Cloudflare A record during certificate issuance. `done` means the install is ready. DNS and the certificate may still be pending.

## Existing Linux VM

On the target VM, have Docker Engine 24+ and Compose 2.24+ available and make ports 80 and 443 reachable. Download the on-prem bootstrap from `https://get.silon.tech/install.sh`. After review and authorisation, run the saved script with `sudo env SILON_HOME=/opt/silon sh ./silon-bootstrap.sh` if it was saved as `silon-bootstrap.sh`. Use `sudo /opt/silon/bin/silon -C /opt/silon install -y -agent-trial -host chat.example.com -email ops@example.com` with the actual user values.

Before running the install, generate a random URL-safe secret on the VM for first-owner setup. Compute the SHA-256 of the secret without a trailing newline and add `-setup-hash` with the hexadecimal hash to the command. The owner opens `https://<actual-host>/setup/#<secret>` privately after HTTPS is ready. If setup instead uses `silon admin create`, let the owner enter their own password. Use `sudo --preserve-env=SILON_INSTALL_TOKEN` for a purchased licence with the token supplied by the user and omit `-agent-trial`.

Use `silon status` or `silon doctor` in the installation directory to verify a VM installation. When helping with a partially installed VM, preserve the existing installation directory and configuration.
