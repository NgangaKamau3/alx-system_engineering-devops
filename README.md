# SSH Configuration and Key Management

This repository contains scripts and configurations for SSH key management and server connections.

## Files

### 0-use_a_private_key
A Bash script that connects to a server using SSH with a private key.

**Requirements:**
- Uses the private key `~/.ssh/school`
- Connects with user `ubuntu`
- Uses only single-character SSH flags
- Does not use the `-l` flag

**Usage:**
```bash
./0-use_a_private_key
```

### 1-create_ssh_key_pair
A Bash script that creates an RSA key pair.

**Requirements:**
- Private key name: `school`
- Key size: 4096 bits
- Passphrase: `betty`

**Usage:**
```bash
./1-create_ssh_key_pair
```

This will create:
- `school` (private key, protected by passphrase "betty")
- `school.pub` (public key)

### SSH Client Configuration
SSH client configuration to use the private key `~/.ssh/school` and disable password authentication.

**Location:** `~/.ssh/config`

**Configuration:**
```
Host *
    IdentityFile ~/.ssh/school
    PasswordAuthentication no
```

## Setup Instructions

1. **Create the SSH key pair:**
   ```bash
   ./1-create_ssh_key_pair
   ```

2. **Set up SSH client configuration:**
   ```bash
   mkdir -p ~/.ssh
   cp ssh_config ~/.ssh/config
   chmod 600 ~/.ssh/config
   ```

3. **Add public key to server:**
   Connect to your server and run:
   ```bash
   mkdir -p ~/.ssh
   echo "your-public-key-here" >> ~/.ssh/authorized_keys
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

4. **Connect to server:**
   ```bash
   ./0-use_a_private_key
   ```

## Security Notes

- Always protect your private keys with appropriate file permissions (600)
- Use strong passphrases for private keys
- Never share your private key files
- Regularly rotate your SSH keys
- Keep your `authorized_keys` file secure (600 permissions)

## Troubleshooting

**Permission denied errors:**
- Check file permissions on private key: `chmod 600 ~/.ssh/school`
- Verify SSH directory permissions: `chmod 700 ~/.ssh`

**Key not found:**
- Ensure the private key exists at `~/.ssh/school`
- Check the SSH client configuration

**Connection refused:**
- Verify the server is running and accessible
- Check firewall settings on the server
- Ensure SSH daemon is running on the server