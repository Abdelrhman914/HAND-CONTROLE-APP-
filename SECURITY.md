# Security Policy

## Official download location

Download Hand Control Camera only from the **Releases** page of this official GitHub repository. Do not trust installers received through unknown websites, chat messages, email attachments, file-sharing sites, or reposted links.

The installer name for this release is:

```text
Hand-Control-Camera-v0.1-Setup.exe
```

## Verify the version 0.1 installer

The official SHA-256 value is published in [`SHA256SUMS.txt`](SHA256SUMS.txt).

In PowerShell, run:

```powershell
Get-FileHash .\Hand-Control-Camera-v0.1-Setup.exe -Algorithm SHA256
```

For the current v0.1 installer, the result must be:

```text
d4874e23555ce6cef7e0ae38a2687df1b34e5cefc92f6fac4a4e36699442781c
```

If the value is different, stop using the file, delete it, and download a fresh copy from the official GitHub Release.

## Digital signatures

### Current v0.1 status

The current v0.1 installer is **not yet Authenticode code-signed**. Its SHA-256 checksum can help identify accidental corruption or a changed file when compared with this official repository, but it is not a substitute for a trusted publisher signature.

### Future signed releases

Future releases should be signed with an Authenticode code-signing certificate and timestamped. For a signed installer, check:

1. Right-click the installer and select **Properties**.
2. Open the **Digital Signatures** tab.
3. Confirm that the signature is valid and the publisher matches the official publisher stated in that release’s notes.

Do not bypass a Windows warning that says the publisher is unknown or the signature is invalid.

## Threat model and limits

A code signature proves that a file was signed by the stated publisher and has not been modified after signing. It does not make unsafe software safe, and it cannot protect a device that has already been compromised by malware or an administrator-level attacker.

An in-app integrity warning alone is not a reliable security control: an attacker who can modify the application could remove that warning. Windows verification of a trusted digital signature is the preferred protection.

## Safe-use guidance

- Keep Windows and Microsoft Defender up to date.
- Run the app as a standard user; do not run it as Administrator unless Windows installation requires it.
- Install only the release file published by this repository.
- Do not replace files inside the installed application folder with files from an untrusted source.
- Do not enter passwords, API keys, or personal credentials into the app. Hand Control Camera does not require them.

## Reporting a vulnerability

Do not publish suspected security vulnerabilities in a public GitHub issue.

When this repository is published, the maintainer should enable **Private vulnerability reporting** in the repository’s GitHub security settings. Use the repository’s **Report a vulnerability** button to send a private report. Include:

- The app version and Windows version
- Clear steps to reproduce the issue
- Expected and actual behavior
- Screenshots or logs only after removing personal information
- A safe contact method for follow-up

## Maintainer release requirements

Before publishing a new release, follow [`docs/RELEASE_CHECKLIST.md`](docs/RELEASE_CHECKLIST.md).
