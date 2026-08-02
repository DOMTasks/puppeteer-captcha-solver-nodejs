# Changelog

All notable changes to this project will be documented in this file.

The format is based on Keep a Changelog.

## [1.0.0] - 2026-07-30

### Added

- Initial public release.
- Text CAPTCHA automation.
- Google reCAPTCHA v2 automation.
- Username/password authentication.
- Authentication Token (2FA) support.
- Automatic challenge submission to DeathByCaptcha.
- Automatic solution injection into supported automation frameworks.
- Captcha reporting (`report()`).
- Solution metadata:
  - success
  - challenge
  - duration
  - id
  - balance
- HTTP proxy support for reCAPTCHA.
- Typed error hierarchy.
- Comprehensive documentation.
- TypeScript typings.
- ESM support.

### Security

- Credentials may be supplied through environment variables.
- Authentication Token (2FA) supported.

### Notes

Initial stable release.