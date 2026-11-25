---
name: silon-install
description: Install self-hosted Silon for a user on Google Cloud, AWS, Azure, or an existing Linux VM. Use for new server deployments, agent trials, licensed installs, and first-time owner setup after an installation.
---

# Install Silon

Use the bundled [deployment reference](references/deploy.md) for commands, browser handoff, and prerequisites. For version-specific questions, use the installed CLI's help and verify which options are available before proceeding.

Use values the user already provided. Ask for missing details together: the provider and intended project, account, or subscription for a new VM, the region or zone, a hostname if wanted, and an email for a trusted certificate if using a domain. Use the server's bundled database by default. If the user wants a managed database, decide before the first install and consult the guide for supported options. Offer **Free trial** for an unlicensed install and the user's chosen licence flow for a purchased licence. Keep the licence option labelled **Free trial**. The owner activates the trial by email, and Silon shows the current duration during activation. Cloud infrastructure is billable during the trial.

## Choose where it runs

- **New VM with an authenticated terminal or approved access to the user's cloud shell.** Use `silon deploy gcp`, `silon deploy aws`, or `silon deploy azure` in that environment. Bootstrap the CLI from `https://get.silon.tech/go/install-cli.sh` if needed. Verify the active account and resource destination before running the deploy. Check the provider's pricing in the chosen region and confirm approval for billable resources, including the VM, disk, static IP, and any managed database, before using `-yes`. For trials use `-agent-trial -yes -json`. For purchased licences follow the guide's token or device approval flow. Default to the provider CLI's existing sign-in. Keep install tokens and external database URLs in `SILON_INSTALL_TOKEN` and `SILON_DATABASE_URL` and let the user supply their values outside the chat.
- **Existing Linux VM with authorised shell access.** Verify the target host, Docker Engine 24+, Compose 2.24+, and access to ports 80 and 443. Use the on-prem bootstrap at `https://get.silon.tech/install.sh` and run noninteractive `silon install -y` on the VM. For a trial pass `-agent-trial`. For a token use `SILON_INSTALL_TOKEN` and preserve it if using `sudo`. Follow the guide to generate the first-owner setup secret on the host and pass only its hash as `-setup-hash`. If the box is already running Silon, check its status and clarify the intended change before installing over it. A cloud shell or chat sandbox can prepare a deployment command, but Silon belongs on the target VM.
- **Chat without access to the user's authenticated terminal.** Give the user a handoff they can run. For a Google Cloud trial with a bundled database, use the browser deployment page if it is available. Prefill the known, URL-encoded values in its fragment as documented. The user signs in to Google and confirms resource creation there. For AWS, Azure, a GCP flow that needs CLI options, or when the browser deployment is unavailable, provide the corresponding Cloud Shell bootstrap and deploy commands with the known values filled in. For an existing VM, give the documented instructions to run over SSH. Keep credentials out of pasted commands and chat.

## Monitor and hand over

For an agent-run cloud deploy, use the documented background command with a private per-run log. Follow JSON progress until `step: done` or a reported failure. The `links` event can arrive before the install is ready. If the monitor drops, inspect the existing log and cloud resources before considering a retry. If the install fails, report the relevant error with secrets redacted, whether resources remain, and the cleanup guidance printed by the CLI.

Give the owner the resulting URL or IP, the DNS A record if applicable, and the remaining certificate or first-boot status. Share the one-time setup link and trial activation link privately with the owner. The owner opens both links and completes account creation and activation. Offer the server's read-only API documentation MCP URL once its certificate is trusted. After a chat-only handoff, report the installation as pending until the user or an available status check confirms it.
