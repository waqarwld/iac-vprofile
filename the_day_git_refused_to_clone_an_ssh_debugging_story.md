# The Day Git Refused to Clone: An SSH Debugging Story

This repository documents a real-world debugging journey of an SSH authentication issue with GitHub. It demonstrates common pitfalls, their causes, and professional solutions.

## Overview
A seemingly simple command:

```bash
git clone git@github.com:waqarwld/iac-vprofile.git
```

failed with:

```
Permission denied (publickey).
```

This repository explains why it failed and how it was resolved.

## Problems Encountered

### 1. Empty or Corrupted SSH Key
- Public key file `~/.ssh/id_ed25519.pub` was empty.
- GitHub had no valid key to authenticate.

**Solution:**
```bash
ssh-keygen -t ed25519 -C "waqarwld@github"
# Add the new public key to GitHub
```

### 2. Corrupted known_hosts File
- Previous GitLab usage corrupted `~/.ssh/known_hosts`.
- SSH refused to connect.

**Solution:**
```bash
mv ~/.ssh/known_hosts ~/.ssh/known_hosts.old
ssh -T git@github.com # accept the fingerprint
```

### 3. Wrong SSH Identity Being Used
- SSH worked when explicitly specifying the key:
```bash
ssh -i ~/.ssh/actions -T git@github.com
```
- Git was not using the correct key by default.

**Solution:** Pin the key in SSH config.

### 4. Permanent Fix via SSH Config
Create or edit `~/.ssh/config`:
```ini
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/actions
  IdentitiesOnly yes
```
- SSH now always uses the correct key.
- `git clone` works without flags.

### 5. Optional Git-Level Override
For per-repo or CI/CD scenarios:
```bash
git config core.sshCommand "ssh -i ~/.ssh/actions -F /dev/null"
```
- Forces Git to use one key and ignore SSH config.
- Useful in pipelines, not recommended for daily dev machines.

## Lessons Learned
- SSH success does not automatically mean Git success.
- `ssh -i` is a diagnostic tool to identify identity issues.
- `known_hosts` corruption is common on Windows or reused machines.
- Use `~/.ssh/config` for scalable, long-term key management.
- CI/CD pipelines and multiple SCM accounts require careful SSH configuration.

---

This repository can serve as a **reference for debugging SSH authentication issues** in Git and as a **storytelling example** for interviews or educational purposes.

 ## then added global user name and email to use all the commit for this computer 
 --git config --global user.name waqarwld 
 --git config --global user.email waqarwld@gmail.com
