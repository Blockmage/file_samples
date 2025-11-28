# Shai-Hulud (v2) Samples

This directory contains a sample of one of the post-install scripts associated with the Shai-Hulud (v2) worm malware.
The worm bundles this malicious `bun_environment.js` file as a post-install script into otherwise legitimate Node.js
packages. It then publishes the infected version of the package, thereby spreading its code throughout the supply chain
and Node.js ecosystem.

Additionally, a \[lightly\] deobfuscated version of the script (`bun_environment.cleaned.js`) has been included. This
version is much easier to read overall, though still quite mangled. It has been included here in hopes it might be
helpful for others researching the malware.

The sample was originally obtained from Socket.dev's website, where more detailed analysis is available for this
particular infected package (`@ensdomains/content-hash@v3.0.1`), linked here:

- <https://socket.dev/npm/package/@ensdomains/content-hash/overview/3.0.1>

Socket has also published a comprehensive overview of the worm and its ongoing campaign, including links to additional
research on the topic:

- <https://socket.dev/blog/shai-hulud-strikes-again-v2>

> [!WARNING]
> These files contain samples of malicious code (Indicators of Compromise). Handle with extreme caution. Do not execute,
> open, or interact with them outside of a secure, isolated environment to avoid potential harm to your system or data.
