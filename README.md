# sshockley.local

Baseline host configuration:

- Shell aliases in `/etc/profile.d/aliases.sh`
- Time sync via chrony or systemd-timesyncd (the other is stopped and disabled)
- Cockpit

## Requirements

- ansible-core 2.15+
- Debian/Ubuntu, EL/Fedora, openSUSE, Alpine (Alpine: chrony only)
- systemd-timesyncd on EL needs EPEL enabled

## Variables

| Variable | Default | Description |
| --- | --- | --- |
| `local_aliases` | `{l: ls -alF}` | Alias name → command |
| `time_sync_provider` | `chrony` | `chrony` or `timesyncd` |
| `time_sync_servers` | `[pool.ntp.org]` | NTP servers |
| `time_sync_fallback_servers` | `[]` | timesyncd `FallbackNTP` |
| `chrony_leapseclist` | `false` | Enable `leapseclist` (chrony >= 4.4) |
| `cockpit_manage` | `true` | Manage Cockpit settings when installed |
| `cockpit_disallowed_users` | `[root]` | Users denied Cockpit login |

OS-specific chrony settings (`chrony_package`, `chrony_service`, `chrony_config`, `chrony_driftfile`, `chrony_keyfile`, `chrony_confdirs`, `chrony_sourcedirs`) default from `vars/main.yml` and can be overridden.

Tags: `aliases`, `time_sync`, `cockpit`.

## Example

```yaml
- hosts: all
  roles:
    - role: sshockley.local
      vars:
        time_sync_provider: timesyncd
        time_sync_servers: [time.example.com]
```

## Development

```sh
pip install -r requirements.txt
yamllint . && ansible-lint
molecule test                # chrony on Fedora + Debian
molecule test -s timesyncd   # systemd-timesyncd
```
