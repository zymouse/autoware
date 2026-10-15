# jtop update

Checks the system installation with `sudo /usr/bin/python3 -I -m pip --isolated show jetson-stats`.
If the package is missing, or its version is at least `7.2.2`, no changes
are made. Other inspection errors fail the role instead of being treated
as a missing package.

For an installed version below `7.2.2`, runs:

```bash
sudo /usr/bin/python3 -I -m pip --isolated install -U jetson-stats --break-system-packages
sudo systemctl restart jtop.service
```

The service restart runs immediately after a successful upgrade. The role
does not install jetson-stats on machines where it is absent. The upgrade
uses the latest version available from the configured pip index, not a
pin to `7.2.2`. The universe playbook's proxy environment also applies to
this role.

Both inspection and upgrade explicitly run as root using `/usr/bin/python3`.
Python isolated mode (`-I`) excludes user site-packages and Python environment
overrides. Pip isolated mode ignores user pip configuration and `PIP_*`
environment overrides, so an activated virtual environment or user-level
pip configuration does not redirect this upgrade. HTTP proxy environment
variables remain available.
