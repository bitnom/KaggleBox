# KaggleBox 📦

A Jupyter notebook that creates secure SSH access to Kaggle notebook environments via ngrok tunneling. Connect to your Kaggle notebooks from your local terminal with full sudo privileges.

Kaggle provides (for free) a generous VM server of 4 CPU cores, 32GB RAM, and the ability to attach 2x T4 GPUs, a P100, or a TPU. This notebook allows you to use that VM as a GPU dev server via SSH. Ollama is included.

## Content Creators / Sharing Notice

Please do not share this repo on X, YouTube, etc. This repo is intended for Discord warriors. Share it on your Discord server, np.

## Features

- 🔐 **Secure SSH Access**: Dedicated user with SSH key authentication
- 🛡️ **Passwordless Sudo**: Full administrative privileges
- 🌐 **Ngrok Integration**: SSH and Ollama tunnels via ngrok
- 🤖 **Ollama Integration**: Pre-configured Ollama server with model pulling
- 💾 **Persistent Storage**: Automatic backup and restore of your development environment
- 🔄 **Auto-Backup**: Background rsync daemon with smart exclusions

## Quick Start

**Note:** GPU & TPU accelerators are disabled by default. You must manually enable it for this Kaggle notebook if/when you want to attach one.

1. **Upload to Kaggle**: Upload `kagglebox.ipynb` to your Kaggle notebook environment

2. **Set Ngrok Token**: Add your ngrok authtoken to Kaggle Secrets with the key `ngrok`

3. **Configure Settings** (Optional): Edit configuration variables in the first cell:
   - `BACKUP_KAGGLEBOX`: Enable/disable automatic backup (default: True)
   - `RESTORE_KAGGLEBOX`: Restore from previous backup (default: False)  
   - `OLLAMA_MODEL`: Ollama model to pull and use (default: "devstral")

4. **Run All Cells**: Execute the notebook to set up SSH access, Ollama, and backups

5. **Connect**: Copy and paste the generated SSH command into your local terminal

## What It Does

- Creates a `kagglebox` user with passwordless sudo access
- Generates SSH keys and configures secure authentication
- Installs and starts OpenSSH server
- Sets up Ollama server with specified model
- Creates ngrok tunnels for SSH and Ollama access
- Runs background backup daemon with smart exclusions for common cache/temp directories
- Provides ready-to-use connection commands

## Configuration

```python
BACKUP_KAGGLEBOX = True   # Enable automatic backup every minute
RESTORE_KAGGLEBOX = False # Restore from previous backup on startup  
OLLAMA_MODEL = "devstral" # Ollama model to pull and use
```

## Example Output

```
!!!!!!!!    IMPORTANT NOTES    !!!!!!!!
1. RUN THE BELOW CODE IN YOUR LOCAL TERMINAL
2. Press Ctrl+C here to stop ngrok when you're done

!!!!!!!!    SSH COMMAND    !!!!!!!!
(
echo -----BEGIN OPENSSH PRIVATE KEY-----
echo [key content...]
echo -----END OPENSSH PRIVATE KEY-----
) > ~/.ssh/rsa_kagglebox && chmod 600 ~/.ssh/rsa_kagglebox
ssh -i ~/.ssh/rsa_kagglebox kagglebox@8.tcp.us-cal-1.ngrok.io -p 17913 -o ServerAliveInterval=60

Ollama URL: https://abc123.ngrok.io
```

## Requirements

- Kaggle notebook environment
- Ngrok account and authtoken  
- Local machine with SSH client

## Troubleshooting

- **Connection issues**: Verify ngrok token is correct and tunnel is active
- **Permission denied**: Check SSH key was created with proper permissions (600)
- **Backup issues**: Check `/kaggle/working/kagglebox-rsync.log` for errors
- **Clean restart**: Set `BACKUP_KAGGLEBOX = False` to disable backups if needed

## Use Cases

- **Remote Development**: Use VS Code, IntelliJ, etc. with Kaggle's powerful hardware
- **Persistent Environment**: Your setup persists across notebook sessions
- **AI Development**: Use Ollama for local LLM development and testing  
- **Package Installation**: Install software with sudo access
- **Long-term Projects**: Work on projects spanning multiple sessions

## TODO

- **Cache Ollama models to backup/working mount**: Store downloaded models in persistent storage to avoid re-downloading across sessions
- **Fully flesh out backup & restore**: Enhance backup system with incremental backups, compression, and selective restore options
- **Integrate code-server/web-based VSCode**: Add web-based VS Code interface accessible through ngrok tunnel
- **Implement Kaggle official models feature**: Integrate access to Kaggle's official pre-trained models
- **Implement Kaggle official datasets feature**: Add functionality to easily access and mount Kaggle datasets

## License

Apache License 2.0 - feel free to modify and distribute.