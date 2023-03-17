# Requirements

- Install docker and docker-compose

# Quickstart

Generate ed25519 key:

`ssh-keygen -t ed25519 -f ssh_host_ed25519_key`

(empty passphrase)

Add the public keys you want to authorize in the `allowed-clients-publickeys` directory (don't forget to `chmod 600` the keys)

Start the sftp server :

`docker-compose up -d`

Important: make sure you have fail2ban installed on your server to prevent bruteforce password attacks

Test the connection locally :

`sftp -P 2222 backupuser@127.0.0.1`

On the remote server from which you want to backup :

- keyscan your backup server : `ssh-keyscan -p <port> <host> >> ~/.ssh/known_hosts`
- test the connection :

```bash
  echo "test" > test.txt
  sftp -P 2222 backupuser@<server ip>:upload <<< $'put test.txt'

```

You should see the file pop up in the `./files` at the same level than `docker-compose.yml`
