# Silon

The **Silon Install** skill helps you set up a self-hosted Silon messaging server on Google Cloud, AWS, Azure, or an existing Linux VM. It helps you choose the installation path, prepare the deployment, and finish first-owner setup. It supports agent trials and purchased Silon licences. Cloud infrastructure is billed by your provider, including during a trial.

Start with “Install a Silon trial on Google Cloud using Cloud Shell.” When the session can control a browser, the skill opens Cloud Shell and runs the Silon CLI there after you sign in and approve cloud costs. If access is blocked, open Google Cloud Shell in your own browser and paste the commands the skill prepares. `gcloud` is already installed in Cloud Shell. You complete first-owner setup and trial activation in your browser.

The plugin also includes **Silon API Docs**, a public MCP connector for the API reference and SDK guides. Try “Find the Silon API documentation and a Python example for sending a message.” The connector reads public docs and requires no account or API key.

In Claude Code, run `/plugin install silon --marketplace KUWAITNET/silon-plugins`.

In Codex, run the following, then start a new session.

```sh
codex plugin marketplace add KUWAITNET/silon-plugins
codex plugin add silon@silon-plugins
```

[Silon](https://silon.tech/) · [Privacy policy](https://silon.tech/privacy-policy/) · [KUWAITNET support](https://online.kuwaitnet.com/submitticket.php)
