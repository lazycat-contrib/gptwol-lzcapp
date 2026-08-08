# GPTWOL LazyCat App

This repository packages [GPTWOL](https://github.com/Misterbabou/gptwol) for LazyCat. GPTWOL wakes and shuts down computers on a local network, checks their status, and manages cron schedules.

## Authentication

Two authentication methods are enabled by default:

- LazyCat OIDC: select `Login with SSO` and use the current LazyCat account.
- GPTWOL local login: use the username and generated password configured by the deployment wizard. The login form is filled automatically but is not submitted automatically.

Either method can be disabled independently during deployment. Keep at least one method enabled to prevent unauthorized access to LAN power controls.

## Wake-on-LAN

GPTWOL uses host networking so it can send Wake-on-LAN packets to the LAN. Wake-on-LAN must be enabled in the target computer's BIOS, network adapter, and operating system, and the computer must be reachable from the LazyCat device.

UDP broadcast is used by default. Layer 2 WOL and manually configured ARP scanning require the actual host LAN interface name; the container default `eth0` may not be the physical LAN interface.

Sleep-on-LAN sends a reversed-MAC packet to UDP port 9. Configure [sleep-on-lan](https://github.com/SR-G/sleep-on-lan) on each target computer as documented by the upstream project.

## Persistence

The computer database under `/app/db` and cron configuration under `/etc/cron.d` are persisted in LazyCat application data and survive upgrades and restarts.

## Automated Publishing

GitHub Actions tracks stable semantic-version tags from `misterbabou/gptwol`, copies the `linux/amd64` image to the LazyCat registry, builds a versioned LPK, and publishes the same verified artifact to the official and private stores.
