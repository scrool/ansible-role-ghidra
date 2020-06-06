# Ansible Role: `ghidra`

Installs [Ghidra](https://ghidra-sre.org/) - a software reverse engineering
(SRE) framework. Extracts a single icon from the release, creates XDG desktop
file to launch the application in GUI mode.

## Requirements

- A recent version of Ansible. Tested on 2.9. It might work on previous versions.
- Fedora 31. It might work on other versions.
- Access to an archive of Ghidra release. Either URL to download or path to a
  file.
- Package `desktop-file-utils` installed or role installs this from configured
  repositories which might connect to internet.
- Ghidra requires a supported version of a Java Runtime and Development Kit on
  the PATH to run. Role does not install it. For detailed information see [Java
  Notes section of Installing
  Ghidra](https://ghidra-sre.org/InstallationGuide.html#Install).

## Role Variables

All variables which can be overridden are stored in
[defaults/main.yml](defaults/main.yml) file and listed in a table bellow as
well.

| Name                            | Default Value | Description |
| ------------------------------- | ------------- | ------------|
| `ghidra_archive_src`            | ""            | Path to an release archive of Ghidra on a control host or URL to download the archive from. |
| `ghidra_state`                  | present       | By default installs the program. Set to "absent" to uninstall. |
| `ghidra_top_dest_name_override` | Ghidra        | Base name of installation directory and name part of a desktop file. If set to empty, top level of release archive is used instead. |
| `ghidra_install_parent_dir`     | /opt          | Parent directory in which program will be installed. See also `ghidra_top_dest_name_override`. |
| `ghidra_desktop_parent_dir`     | /usr/local/share/applications | Parent directory in which desktop file will be installed. See also `ghidra_top_dest_name_override`. |
| `ghidra_force_remove`           | no            | If set to "yes", contents of installation directory and desktop file are not compared against contents of `ghidra_archive_src` when uninstall, upgrade or downgrade. |

`ghidra_archive_src` must be overriden from its defaults. For URL to a latest
release see [Ghidra home page](https://ghidra-sre.org/).

## Example Playbook

### Install

To install the program, define URL to download Ghidra release archive from the
internet:

```yaml
- hosts: all
  roles:
    - role: scrool.ghidra
      vars:
        ghidra_archive_src: https://ghidra-sre.org/ghidra_9.1.2_PUBLIC_20200212.zip
```

### Uninstall

To uninstall, set same values as for install. By default role removes content
only if installed directory, extracted icon and desktop file matches what it
would install. Finally set state variable to "absent".

Specify local path to a downloaded release archive file:

```yaml
- hosts: all
  roles:
    - role: scrool.ghidra
      vars:
        ghidra_archive_src: /tmp/ghidra_9.1.2_PUBLIC_20200212.zip
        ghidra_state: absent
```

Alternatively you can enable option `ghidra_force_remove` and again set state
variable to "absent". In this case role won't need nor even check
`ghidra_archive_src`. This effectively removes installation dir, extracted icon
and desktop entry file. Example:

```yaml
- hosts: all
  roles:
    - role: scrool.ghidra
      vars:
        ghidra_state: absent
        ghidra_force_remove: yes
```

## License

This project is licensed under MIT License. See [LICENSE](LICENSE) for more
details.

## Author Information

- [Pavol Babinčák](https://github.com/scrool)
