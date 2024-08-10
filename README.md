# bw-ssh-add

**Add SSH keys to your agent using passphrases stored in Bitwarden.**

---

A script to automate adding SSH keys to the local [SSH agent][1] using
passphrases stored in [Bitwarden][2]. It retrieves passphrases securely using
the [Bitwarden CLI][3], then uses [`expect`][4] to interact with [`ssh-add`][5].

[1]: https://www.ssh.com/academy/ssh/agent
[2]: https://bitwarden.com/
[3]: https://github.com/bitwarden/clients/tree/main/apps/cli
[4]: https://core.tcl-lang.org/expect/home
[5]: https://www.ssh.com/academy/ssh/add-command

## Usage

    bw-ssh-add <BITWARDEN-ITEM-ID> [SSH-ADD-ARGUMENTS...]

- The first argument is passed to `bw` as either a search term or an item's
  globally unique identifier to retrieve the passphrase.
- All additional arguments are passed through to `ssh-add` unchanged. Refer to
  the [`ssh-add` man page][6] for details on available options.

[6]: https://man.openbsd.org/ssh-add.1

The script sets an expiration time for the added key:

- Default: **17:00:00** (5:00 PM local time)
- If it's already past 5:00 PM: **3 hours**
- Customize the end-of-day time using the `BW_SSH_ADD_EOD` environment variable
  (format: **HH:MM:SS**)
- To remove the maximum lifetime, set `BW_SSH_ADD_EOD` to an **empty string**

### Examples

    bw-ssh-add "My SSH Key"
    bw-ssh-add 99ee88d2-6046-4ea7-92c2-acac464b1412
    bw-ssh-add "Work Laptop Key" -t 3600
    BW_SSH_ADD_EOD="18:30:00" bw-ssh-add "Custom EOD Key"
    BW_SSH_ADD_EOD="" bw-ssh-add "No Expiry Key"

## Installation

1. Ensure you have the required dependencies installed and configured:

   - Bitwarden CLI (`bw`)
   - `expect` command
   - SSH agent

2. Add the _bw-ssh-add_ script to your \$PATH:

   ```bash
   git clone https://github.com/elasticdog/bw-ssh-add.git
   cd bw-ssh-add/
   sudo ln -s ${PWD}/bw-ssh-add /usr/local/bin/bw-ssh-add
   ```

## License

bw-add-ssh is released under the [Zero Clause BSD License][0BSD] (SPDX: 0BSD).

Copyright &copy; 2024 [Aaron Bull Schaefer][EMAIL] and contributors

[0BSD]: https://github.com/elasticdog/bw-ssh-add/blob/main/LICENSE
[EMAIL]: mailto:aaron@elasticdog.com
