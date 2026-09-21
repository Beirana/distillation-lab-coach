# Cross-platform SSH Onboarding for a Local Agent

Passwordless SSH is the recommended remote-GPU route unless the user chooses another. Reuse an existing currently verified alias and key without reauthorization. Recheck when the instance or client context changes: a reused alias is not the same-host guarantee. Complete setup before class when possible. Jupyter remains useful for file teaching and as a fallback. Local computation does not require SSH back into the same machine.

The Windows/remote-image route has course rehearsal evidence; macOS/Linux client instructions below are documentation-informed and still need actual-host verification. Windows PowerShell, macOS/Linux shells, WSL and the Agent's sandbox can have different SSH executables, config files and agents. Confirm the actual execution context before copying commands. Only an SSH client is needed on the control computer; do not install a local SSH server or enable Mac Remote Login for outbound connections.

The human establishes trust and authentication once. The Agent uses only the resulting alias in non-interactive mode. The Agent never receives a password, private-key passphrase, or private-key contents.

## 1. Check the actual client and public identity

### Windows PowerShell

Run in the user's local PowerShell:

```powershell
ssh -V
Get-Command ssh
Get-ChildItem "$env:USERPROFILE\.ssh" -Filter *.pub -ErrorAction SilentlyContinue
Test-Path "$env:USERPROFILE\.ssh\id_ed25519.pub"
```

These commands check the OpenSSH client and list only public-key files. They do not read a private key.

If the public key is absent, the human may run:

```powershell
ssh-keygen -t ed25519
```

The human chooses the save path and passphrase and must not overwrite an existing key without investigating it. A missing default `.pub` does not prove no identity exists: check with the user for a custom path, managed identity or agent-held key. If a custom path is chosen, use it consistently in checks, `IdentityFile`, key installation and `ssh-add`.

### macOS / Linux / WSL POSIX shell

Run locally, not on the GPU server:

```bash
ssh -V
command -v ssh
for public_key in "$HOME"/.ssh/*.pub; do
    if test -f "$public_key"; then printf '%s\n' "$public_key"; fi
done
```

This lists public filenames only. Reuse an appropriate identity; otherwise the human can run `ssh-keygen -t ed25519` interactively, choosing a nonconflicting path and passphrase. Respect organization-managed keys or certificates instead of forcing an algorithm. If the client is missing, choose an approved OS-specific installation, not an automatic package-manager install. Never copy a long-lived private key onto a shared classroom computer.

## 2. Merge a host alias

Windows native OpenSSH uses `%USERPROFILE%\.ssh\config`; macOS/Linux/WSL use that user's `~/.ssh/config`. WSL config is not automatically Windows config. Merge a block without overwriting other entries or colliding with existing aliases. `distill-lab` is an example, never a default instance name. Replace hostname, account, port and identity using the actual platform or administrator's information; `22` is not an inferred cloud port. `replace-me.invalid` intentionally fails until replaced.

```sshconfig
Host distill-lab
    HostName replace-me.invalid
    User replace-me
    Port 22
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    StrictHostKeyChecking ask
    ServerAliveInterval 30
    ServerAliveCountMax 3
```

The `ServerAlive*` options keep an idle connection active. They do not keep a remote training process alive after the SSH session exits; use an explicitly approved process manager for long post-class work.

On an applicable POSIX filesystem, inspect ownership and permissions: the selected `.ssh` directory is normally mode 700, config/private key 600. Adjust only verified relevant paths if needed; do not recursively chmod the home directory or apply POSIX permissions on Windows. Managed ACLs, links and hardware-backed identities require their own handling.

## 3. Install the public key once

### Windows PowerShell

The human runs this pipeline once, after confirming the alias and intended remote account:

```powershell
Get-Content "$env:USERPROFILE\.ssh\id_ed25519.pub" |
  ssh distill-lab "umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys; chmod 700 ~/.ssh; chmod 600 ~/.ssh/authorized_keys"
```

On the first connection:

1. the human compares the displayed SHA256 host fingerprint with a value supplied through a trusted platform view, instructor channel, or other independent channel;
2. the human types `yes` only after the fingerprint matches;
3. the human enters the remote account password if that authentication method is available. MFA or disabled password login may require the platform/admin's key-enrollment route instead; one password is not a universal guarantee.

Run the append command only once for a key. Repeating it creates duplicate authorized-key lines.

If no independent fingerprint is available, describe the action as first-use trust, not verified identity; let the human decide. Do not hide that distinction or accept it on their behalf.

### macOS / Linux

Where `ssh-copy-id` is installed, the human can use:

```bash
command -v ssh-copy-id
ssh-copy-id -i "$HOME/.ssh/id_ed25519.pub" distill-lab
```

Use the actual selected public key. Do not assume macOS has this helper or install Homebrew just for it. If absent, use the platform's public-key enrollment, or let the human append the selected public key's complete line to the intended account's `~/.ssh/authorized_keys` through an already trusted session/console. Preserve existing entries, avoid duplicates, and check ownership/permissions. No trusted login/enrollment path means asking the administrator, not weakening SSH. The fingerprint and credential rules above apply equally.

## 4. Verify non-interactive access

```powershell
ssh -o BatchMode=yes -o StrictHostKeyChecking=yes -o ConnectTimeout=10 distill-lab "printf 'PUBLICKEY_OK\n'; hostname; whoami"
```

Run after the human's first trust/authentication setup. This command works with native OpenSSH in the listed shells, disables interactive prompts and rejects unknown/changed host keys. Compare output to the intended current instance. Verify first in the human's terminal, then in the Agent's actual execution context. Human success is not Agent success. This establishes connection capability, not GPU/runtime readiness.

If the private key has a passphrase, the human may load it into the Windows SSH agent:

```powershell
ssh-add "$env:USERPROFILE\.ssh\id_ed25519"
```

The human may need to enable the Windows `ssh-agent` service first. The Agent must not change that system service or request the passphrase on the user's behalf.

On macOS/Linux the human may run `ssh-add ~/.ssh/id_ed25519` for an available local agent, adjusting the actual path. Prefer the OS's existing key/agent integration; do not impose Mac keychain options on Linux or a Windows service on WSL. An agent started in another shell does not automatically become visible to an already-running application. Diagnose this boundary instead of asking repeatedly for the passphrase. Agent forwarding is not needed by default.

## 5. Agent command pattern

Keep `BatchMode=yes` on Agent-initiated calls so an authentication failure returns an error instead of waiting for secrets:

```powershell
ssh -o BatchMode=yes -o StrictHostKeyChecking=yes -o ConnectTimeout=10 distill-lab "hostname"
```

Replace `hostname` with the intended remote command. Preserve the alias and non-interactive option.

For authorized local-browser access to an existing loopback-bound remote service, an optional narrow tunnel is:

```bash
ssh -o BatchMode=yes -o StrictHostKeyChecking=yes -o ExitOnForwardFailure=yes -N -L 127.0.0.1:18001:127.0.0.1:8001 distill-lab
```

Check actual ports first. The tunnel remains running; stop only this task's tunnel when done. It does not stop the remote model service. Do not open public listeners or change firewalls as a convenience.

## Safety boundaries

Never:

- read, upload, paste, or transmit the private-key file;
- ask the Agent to type a password or key passphrase;
- set `StrictHostKeyChecking no`;
- use an empty known-hosts destination such as `NUL` or `/dev/null`;
- use unverified `ssh-keyscan` output as proof of server identity;
- automatically accept a changed host key;
- delete `known_hosts` or overwrite an existing private key as a repair.

If `REMOTE HOST IDENTIFICATION HAS CHANGED` appears, stop. The human must verify the new platform instance and fingerprint through a trusted channel before removing a specific old host-key entry.

On temporary/shared computers use approved temporary identities, not shared private keys; arrange human-controlled revocation and logout after class. Respect school network/device policies. An approved browser/Jupyter route is a valid fallback when local installation or SSH access is unavailable.

## Official references

- [OpenSSH client configuration](https://man.openbsd.org/ssh_config): aliases, identities, host checking and batch behavior.
- [Apple Terminal connections](https://support.apple.com/guide/terminal/connect-to-servers-trml1018/mac): using the Mac as a client.
- [Ubuntu OpenSSH guidance](https://ubuntu.com/server/docs/how-to/security/openssh-server/) and [ssh-copy-id manual](https://manpages.ubuntu.com/manpages/jammy/man1/ssh-copy-id.1.html): public-key enrollment. These are not evidence that the current course host has been tested.
