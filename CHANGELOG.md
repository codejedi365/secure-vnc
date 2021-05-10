# CHANGELOG
All notable changes to this project will be documented in this file.

*The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).*

----
## \[Unreleased]
<!-- ### Added -->
<!-- ### Changed -->
### Removed
- Unnecessary and confusing tab completion (shell emulation) when asking for user input when it is not relevant to filesystem
<!-- ### Fixed -->
<!-- ### Security -->

----
## [v1.2] - *2020-12-28*
### Added
- Automated dual/multi screen setup through a single master socket
- User review & approval prior to initial SSH/VNC connections
- Published CHANGELOG.md
### Changed
- CLI prompts & input processing order for SSH & VNC parameters
- Changed file sourcing to instead execute subsequent files
- Incrementally generates the ssh dynamic-config when making VNC connections
### Removed
- Dropped user input for remote host vnc base port. Parses TigerVNC output to obtain proper port
### Security
- Killed & removes keys of ssh-agent if VNC script exits or interrupts

----
## [v1.1] - *2020-11-07*
### Added
- Linux support for auto-opening vnc urls
- Official Readme created
- Identity key selection tab-completes inside the `~/.ssh` directory by default
### Fixed
- Fixed env variable collision of USERNAME field on Ubuntu
- Fixed relative vs absolute paths that caused identity key mixmatches

----
## [v1.0] - *2020-10-25*
### Initial Release Features
- Mac OSX Client Support to autoconfigure connection remote system
- TigerVNC far end integration to review & spawn desktop instances
- Script auto-clean upon EXIT, user termination, or error
- Dynamic port forwarding based on user and remote host configuration
- Detailed CLI for ssh & VNC connections
- Uses ssh-agent for isolated ssh connection creation
- Provides Secure VNC session through SSH port forwarding via public key infrastructure

----
## Version Diffs
\[Unreleased]: https://github.com/codejedi365/secure-vnc/compare/v1.2...HEAD

\[1.2]: https://github.com/codejedi365/secure-vnc/compare/v1.1...v1.2

\[1.1]: https://github.com/codejedi365/secure-vnc/compare/v1.0...v1.1

\[1.0]: https://github.com/codejedi365/secure-vnc/releases/tag/v1.0
