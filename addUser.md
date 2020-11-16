# Adding New User

## Add User

```bash
useradd --create-home nasermirzaei89
```

## Set Password

```bash
passwd nasermirzaei89
```

## Make User a Sudoer

```bash
pacman -S sudo
groupadd sudo
usermod --append --groups sudo nasermirzaei89
```

Edit `/etc/sudoers` and uncomment this line:

```bash
# Uncomment to allow members of group sudo to execute any command
%sudo ALL=(ALL) ALL
```

## Login as This User

```bash
su - nasermirzaei89
```
