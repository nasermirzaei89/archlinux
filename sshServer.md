# Install SSH Server

```bash
sudo pacman -S openssh
sudo systemctl enable sshd
sudo systemctl start sshd
sudo systemctl status sshd
```

Now you can connect to your Arch Linux from another OS through ssh.
