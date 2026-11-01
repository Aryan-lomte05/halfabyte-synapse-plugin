# Privacy policy

This covers the Synapse Compiler plugin for Claude. Last updated 1 November 2026.

## On your machine

The plugin runs on your computer. It stores workflow graphs and measurements (time, tokens, cost) in a `data` folder in your working directory. A run's outputs are saved as a fingerprint, not as text, unless you turn on full capture. Nothing in that folder is sent anywhere by the plugin.

A workflow you run may pass personal data through memory, if that is what you give it. Synapse doesn't look for it or keep it by default.

## What leaves your machine

- **Your model provider.** A workflow you run calls the provider that workflow is set up for, on your own key. That provider's terms apply.
- **Synapse Cloud, only if you set `SYNAPSE_API_KEY`.** The `cloud_dashboard` and `cloud_run` tools read your project over HTTPS. They send your key and, for `cloud_run`, a run ID. They send no prompts or answers, and nothing they send is stored as data.

The plugin sends no telemetry, and it sends nothing to Synapse Cloud unless you set that key.

## Server logs

The Synapse Cloud server's access logs record each request's path, which can include a run ID, with keys removed.

## The Synapse Cloud website

This policy covers the plugin. If you make an account on the website, it holds your email, phone number, a hashed password and the measurements your projects report. Those measurements stay until you delete them.

## Contact

Open an issue at <https://github.com/Aryan-lomte05/halfabyte-synapse-plugin/issues>.
