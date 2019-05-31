Ansible Role: GitHub Release
=========

A generic role to install a binary application released on GitHub in a Linux x86_64 based distro.

Requirements
------------

The role targets Debian and RHEL based systems build on the `x86_64` architecture.

The role is intended to run on the remote machine, which means that internet connectivity on remote is required.

This role is quite generic, which means that caution has to be taken as to what packages are installed.

__Important:__ Always review the packages you are trying to install since there is no way to validate the checksum of the download generically.

Role Variables
--------------

Required:

```yaml
ghr_org_name: # The name of a valid GitHub organisation
ghr_app_name: # The name of a valid GitHub repository, belonging to the organisation
```

Default:

```yaml
ghr_app_version: "latest" # A valid released version from: https://github.com/{{ ghr_org_name }}/{{ ghr_app_name }}/releases/

ghr_app_binary_dest: "/opt/{{ ghr_app_name }}" # The destination directory where the `ghr_app_name` binary will be placed

ghr_app_cleanup_after: false # If set to true, it will cleanup all downloaded files

ghr_app_configure_system_path: true # Whether the `ghr_app_binary_dest` directory should be added to the system `PATH`
ghr_app_system_path_prepend: false # Whether to append or prepend the `ghr_app_binary_dest` directory into the `PATH`, IF (ghr_app_configure_system_path is True).

ghr_app_tmp_dir: # Temporary folder to store the downloaded archive

ghr_app_releases_url: # The URL of github releases.
ghr_app_archive: # The name of the archive.
```

Dependencies
------------

None

Example Playbook
----------------

Running the role multiple times will lead to variable merge issues.

Recommended usage is to run one role per host execution.

```yaml
    - hosts: localhost
      roles:
        - role: nioniosfr.github_release
          vars:
            ghr_org_name: "digitalocean"
            ghr_app_name: "doctl"
            ghr_app_version: "1.18.0"
            ghr_app_tmp_dir: "/mnt/nfs_share/downloads" # Store the downloaded archive in a more persistent path than '/tmp'

    - hosts: localhost
      roles:
        - role: nioniosfr.github_release
          vars:
            ghr_org_name: "stedolan"
            ghr_app_name: "jq"
            ghr_app_version: "1.6"
            ghr_app_releases_endpoint: "{{ ghr_app_name }}-{{ ghr_app_version }}"
            ghr_app_archive: "{{ ghr_app_name }}-linux64"
            ghr_app_is_binary: true

    - hosts: localhost
      roles:
        - role: nioniosfr.github_release
          vars:
            ghr_org_name: "digitalocean"
            ghr_app_name: "doctl"
            ghr_app_version: "1.18.0"
            ghr_app_binary_dest: "/usr/local/bin" # Installs in a common user path
            ghr_app_configure_system_path: false # Do not manipulate the system path for users
            ghr_app_tmp_dir: "/mnt/nfs_share/downloads" # Change the folder used for the downloads
            ghr_app_cleanup_after: true # Remove both the downloaded file, as well as the system profile.d for `app` if it was already created from a previous run
```

License
-------

MIT

Author Information
------------------

[NioniosFr](https://github.com/NioniosFr)
