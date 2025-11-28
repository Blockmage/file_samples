# Shai Hulud (v2) Samples: The Bun-Powered Supply Chain Worm

This directory contains samples related to the Shai Hulud (v2) worm, a sophisticated supply chain attack that has
compromised hundreds of npm packages and exposed secrets from thousands of GitHub repositories. This version of the worm
notably leverages the [**Bun runtime**](https://bun.com/docs/runtime) for its execution.

The worm operates by bundling a malicious `bun_environment.js` file and a loader script (`setup_bun.js`) into otherwise
legitimate Node.js packages. It then publishes infected versions, spreading its code throughout the software supply
chain and Node.js ecosystem.

## Key Characteristics of Shai Hulud v2

- **Bun Runtime Execution:** It utilizes Bun for stealthy background execution of its main payload.

- **Self-Healing Propagation:** The worm searches public GitHub repositories for a beacon phrase:

  > `"Sha1-Hulud: The Second Coming."`

  If found, it reads a stored file containing a triple base64-encoded token and uses this token as its primary
  credential for further exfiltration.

- **Credential Harvesting:** Like the previous version of the Shai Hulud malware, the worm uses
  [Trufflehog](https://github.com/trufflesecurity/trufflehog) to scan the victim's filesystem for access tokens, API
  keys, and credentials. If it discovers the necessary credentials to do so, it aggressively enumerates secrets from
  AWS, GCP, and Azure.

- **Exfiltration:** Stolen credentials are triple base64-encoded before being published to newly-created, public GitHub
  repositories on the victim accounts for exfiltration.

- **GitHub Actions Exploitation:** It targets CI/CD environments, including privilege escalation on GitHub Actions
  runners (Linux), DNS/firewall manipulation, and automated scraping of GitHub Actions workflow secrets.

  The worm creates malicious workflows on GitHub, registering compromised devices as workflow runners (self-hosted
  GitHub Actions runners), and then utilizes them for further command and control (C2) operations.

- **Worm Propagation:** Stolen npm tokens (including those mined from GitHub Actions) are used to publish numerous new,
  infected, miro-incremented (patch) versions of the packages.

- **Destructive Payload:** If no exfiltration path is found, it executes a destructive payload that shreds and deletes
  user files on Windows, Linux, and macOS systems.

## Included Samples

> [!WARNING]
> These files contain live samples of ***malicious code.*** Handle with caution! Do not execute, open, or interact with
> the samples outside of a secure, isolated environment to avoid potential harm to your system, theft of sensitive data,
> or other potential damages.

- `bun_environment.js` (Original): The obfuscated malicious payload.
- `bun_environment.cleaned.js` (Deobfuscated): A *\[lightly\]* deobfuscated version of the script. This version is
  much easier to read and analyze, though still quite mangled. It is included here in hopes it might be helpful to
  others researching the malware.
- `setup_bun.js`: A stealthy loader script that locates or downloads and installs Bun and executes `bun_environment.js`.

The samples were originally obtained from Socket's analysis of this infected package/version:

- `@ensdomains/content-hash@v3.0.1`.

More detailed analysis is available via the links referenced below.

## References

- **Package Analysis (Socket):** <https://socket.dev/npm/package/@ensdomains/content-hash/overview/3.0.1>
- **Comprehensive Overview (Socket):** <https://socket.dev/blog/shai-hulud-strikes-again-v2>
- **Post Mortem (PostHog):** <https://posthog.com/blog/nov-24-shai-hulud-attack-post-mortem>
