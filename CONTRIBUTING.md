# Contributing to Hand Control Camera

Thank you for helping improve Hand Control Camera. Contributions that make gesture recognition more reliable, improve accessibility, fix bugs, improve documentation, or strengthen privacy and security are welcome.

## Before you begin

- Use the official repository as the source of truth.
- Open an issue before starting a large feature or major redesign.
- Keep changes focused and explain what problem they solve.
- Never commit passwords, API keys, local settings, private certificates, or personal camera recordings.
- Do not add images or recordings of another person unless you have their clear permission to publish them.

## Development contributions

When the source tree is available in the repository:

1. Fork the repository.
2. Create a branch with a clear name, for example `fix/gesture-release-delay`.
3. Make a small, focused change.
4. Run the available tests and explain the result in your pull request.
5. Update the README or other documentation when user-visible behavior changes.
6. Open a pull request describing the problem, the solution, and how it was tested.

## Gesture and privacy principles

Contributions should preserve these project goals:

- Camera frames are processed locally by default.
- The app must not silently record or upload webcam content.
- Avoid false media or volume actions; deliberate gesture holds are safer than instant triggering.
- Keep the app usable without Administrator privileges during normal use.
- Document any new system permission, internet feature, or data collection clearly before it is added.

## License for contributions

By intentionally submitting a contribution to this repository, you agree that your contribution is provided under the [Apache License 2.0](LICENSE), unless you clearly state otherwise in writing before it is accepted.

## Security reports

Do not report security vulnerabilities through a public issue. Follow the instructions in [SECURITY.md](SECURITY.md).
