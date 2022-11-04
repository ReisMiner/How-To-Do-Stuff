# How To export GPG key as SSH key

## Steps

1. Open the terminal
2. get the key id by using the following command: `gpg --list-key`
3. copy the correct key id. it looks about like this: `91516D83AEA3AB7345702EF4560`
4. use command `gpg --export-ssh-key <KEY_ID>`
