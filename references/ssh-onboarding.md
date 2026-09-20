# Windows SSH Onboarding for a Local Agent

Passwordless SSH is the recommended remote-GPU route unless the user chooses another. Reuse an existing verified Windows alias and key without reauthorization; read setup steps only when no usable alias exists. Complete setup before class when possible. Jupyter remains convenient for file teaching and as a fallback.

The human establishes trust and authentication once. The Agent uses only the resulting alias in non-interactive mode. The Agent never receives a password, private-key passphrase, or private-key contents.

## 1. Check the Windows client and public key

Run in the user's local PowerShell:

```powershell
ssh -V
Get-ChildItem "$env:USERPROFILE\.ssh" -Filter *.pub -ErrorAction SilentlyContinue
Test-Path "$env:USERPROFILE\.ssh\id_ed25519.pub"
```

These commands check the OpenSSH client and list only public-key files. They do not read a private key.

If the public key is absent, the human may run:

```powershell
ssh-keygen -t ed25519
```

The human chooses the save path and passphrase and must not overwrite an existing key without investigating it. If a custom path is chosen, use that same path in `Test-Path`, `IdentityFile`, `Get-Content`, and `ssh-add` below.

## 2. Merge a host alias

Windows OpenSSH reads `%USERPROFILE%\.ssh\config`. Merge this block manually. `replace-me.invalid` is a failure-safe placeholder and must be replaced with the hostname from the user's platform console. Replace the port as well.

```sshconfig
Host distill-lab
    HostName replace-me.invalid
    User root
    Port 22
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    StrictHostKeyChecking ask
    ServerAliveInterval 30
    ServerAliveCountMax 3
```

The `ServerAlive*` options keep an idle connection active. They do not keep a remote training process alive after the SSH session exits; use an explicitly approved process manager for long post-class work.

## 3. Install the public key once

Run this pipeline in local PowerShell:

```powershell
Get-Content "$env:USERPROFILE\.ssh\id_ed25519.pub" |
  ssh distill-lab "umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys; chmod 700 ~/.ssh; chmod 600 ~/.ssh/authorized_keys"
```

On the first connection:

1. the human compares the displayed SHA256 host fingerprint with a value supplied through a trusted platform view, instructor channel, or other independent channel;
2. the human types `yes` only after the fingerprint matches;
3. the human enters the remote account password once.

Run the append command only once for a key. Repeating it creates duplicate authorized-key lines.

If no independent fingerprint is available, describe the action as first-use trust, not verified identity. Do not hide that distinction.

## 4. Verify non-interactive access

```powershell
ssh distill-lab "printf 'SSH_OK\n'; hostname; whoami"
ssh -o BatchMode=yes distill-lab "printf 'PUBLICKEY_OK\n'; hostname; whoami"
```

The second command disables password and fingerprint prompts. It must succeed before an Agent is allowed to use the alias.

If the private key has a passphrase, the human may load it into the Windows SSH agent:

```powershell
ssh-add "$env:USERPROFILE\.ssh\id_ed25519"
```

The human may need to enable the Windows `ssh-agent` service first. The Agent must not change that system service or request the passphrase on the user's behalf.

## 5. Agent command pattern

Keep `BatchMode=yes` on Agent-initiated calls so an authentication failure returns an error instead of waiting for secrets:

```powershell
ssh -o BatchMode=yes distill-lab "hostname"
```

Replace `hostname` with the intended remote command. Preserve the alias and non-interactive option.

## Safety boundaries

Never:

- read, upload, paste, or transmit the private-key file;
- ask the Agent to type a password or key passphrase;
- set `StrictHostKeyChecking no`;
- set `UserKnownHostsFile NUL`;
- use unverified `ssh-keyscan` output as proof of server identity;
- automatically accept a changed host key;
- delete `known_hosts` or overwrite an existing private key as a repair.

If `REMOTE HOST IDENTIFICATION HAS CHANGED` appears, stop. The human must verify the new platform instance and fingerprint through a trusted channel before removing a specific old host-key entry.
