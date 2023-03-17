# Requirements

- Install docker and docker-compose

# Quickstart

Note : I used port 2222, feel free to change that in `docker-compose.yml`

## Create the server's identity

Generate ed25519 key:

`ssh-keygen -t ed25519 -f ssh_host_ed25519_key`

(empty passphrase)

## Authorizing clients

Add the public keys you want to authorize in the `allowed-clients-publickeys` directory (don't forget to `chmod 600` the keys)

## Start and test the sftp server

Important: make sure you have `fail2ban` installed on your server to prevent bruteforce password attacks

`docker-compose up -d`

## Test the connection locally:

Authorize your computer using a ssh key:

`cp ~/.ssh/id_rsa.pub allowed-clients-publickeys`

Test :

`sftp -P 2222 backupuser@127.0.0.1`

## Test and use from another computer or server

- Optional, but to prevent an interactive "do you trust this server" prompt, keyscan your backup server : `ssh-keyscan -p <port> <host> >> ~/.ssh/known_hosts`
- Try sending a file:

```bash
  echo "test" > test.txt
  sftp -P 2222 backupuser@<server ip>:upload <<< $'put test.txt'

```

You should see the file pop up in the `./files` at the same level than `docker-compose.yml`
