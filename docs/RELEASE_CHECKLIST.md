# Release and Code-Signing Checklist

This checklist is for the maintainer of Hand Control Camera. It helps prevent accidental publication of modified, untested, or untrusted builds.

> A digital signature is the main protection against repacked or altered copies. Do not describe a release as signed unless Windows can validate the published signature.

## One-time setup

- [ ] Create the official GitHub repository and use its **Releases** page as the only public download source.
- [x] The project uses the Apache License 2.0. Keep `LICENSE` and `NOTICE` with every source distribution; installer Terms and Conditions are not a source-code license.
- [ ] Enable GitHub **Private vulnerability reporting** under repository security settings.
- [ ] Obtain an Authenticode code-signing certificate from a trusted certificate authority.
- [ ] Decide which legal name or company name will appear as the Windows publisher.
- [ ] Keep the signing certificate/private key in a hardware token or approved cloud-signing service. Never commit it, email it, or store it in the repository.
- [ ] Install the Windows SDK `signtool.exe` on the secure build machine.
- [ ] Make sure the build machine has current Windows and antivirus updates.

## Before building

- [ ] Use a clean, trusted source checkout.
- [ ] Review dependency versions; do not install packages from unknown download links.
- [ ] Check for accidental secrets: `.env` files, API tokens, passwords, private keys, personal settings, or test recordings.
- [ ] Confirm that generated folders such as `build/`, `dist/`, virtual environments, and local uploads are ignored by Git.
- [ ] Update the version number, README, known limitations, and release notes.
- [ ] Run the automated tests.

## Build and sign

1. Build the application EXE from the trusted source.
2. Sign the application EXE **after** it has been built.
3. Build the installer using the signed application EXE.
4. Sign the final installer **after** Inno Setup creates it.
5. Timestamp both signatures so they remain verifiable after the certificate expires.

Example commands — update the certificate selection and timestamp service to match the certificate provider’s instructions:

```powershell
signtool sign /fd SHA256 /tr http://timestamp.digicert.com /td SHA256 /a .\HandControlCamera.exe
signtool sign /fd SHA256 /tr http://timestamp.digicert.com /td SHA256 /a .\Hand-Control-Camera-v0.1-Setup.exe
```

Verify both signatures:

```powershell
Get-AuthenticodeSignature .\HandControlCamera.exe | Format-List Status,StatusMessage,SignerCertificate
Get-AuthenticodeSignature .\Hand-Control-Camera-v0.1-Setup.exe | Format-List Status,StatusMessage,SignerCertificate
```

The `Status` must be `Valid`. Confirm the displayed publisher is correct.

> For Inno Setup, configure its `SignTool` integration so the setup executable and uninstaller are signed during the release build. Keep the actual certificate command outside the repository if it includes account identifiers, token paths, or secrets.

## Test the release artifact

- [ ] Test installation on a separate Windows user account or clean Windows virtual machine.
- [ ] Confirm the installer’s **Digital Signatures** tab shows a valid signature.
- [ ] Confirm Terms and Conditions acceptance is required.
- [ ] Confirm the camera permission and camera selection behavior.
- [ ] Test volume up, volume down, pause, play, settings, and `Q`/`Esc` exit.
- [ ] Confirm no app files are writable by standard users after installation in Program Files.
- [ ] Confirm the app does not require Administrator privileges while running.
- [ ] Test uninstalling the app.

## Publish to GitHub Releases

- [ ] Create an annotated version tag, for example `v0.1.0`.
- [ ] Create a GitHub Release from that tag.
- [ ] Upload the signed setup EXE as a release asset; do **not** commit it to normal repository history.
- [ ] Create a fresh `SHA256SUMS.txt` using:

```powershell
Get-FileHash .\Hand-Control-Camera-v0.1-Setup.exe -Algorithm SHA256
```

- [ ] Upload `SHA256SUMS.txt` as a release asset and commit the same checksum file to the tagged source release.
- [ ] State the exact expected publisher name and checksum in the release notes.
- [ ] Download the release from GitHub and verify its signature and checksum once more.

## Never upload

- Signing certificates, private keys, PINs, passwords, or token configuration
- `.env` files or credentials
- `%APPDATA%` settings and test data
- Camera recordings or screenshots without permission
- Build caches, virtual environments, `dist/`, `build/`, and temporary output
- The installer EXE in ordinary Git history

## If a release key is lost or suspected compromised

1. Stop publishing new releases with that certificate.
2. Revoke the certificate through its provider.
3. Announce the affected versions through GitHub Releases and the security policy.
4. Build and sign a replacement release with a new certificate.
5. Update the expected publisher and verification instructions if they changed.
