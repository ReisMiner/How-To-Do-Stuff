# How to setup mailcow-dockerized with standalone nginx proxy manager infront

So my setup is as follows: Standalone NPM infront of mailcow. NPM is used for other services like SearxNG. In NPM I added a proxy host for mail.mydomain.com and setup the ssl cert.

Now my problem: When mailcow-acme wants to renew the certs, it fails. Says something like this in the logs: `ValueError: Challenge did not pass for mail.mydomain.com:` and throws a python stacktrace.

So apparently it's not really possible to use this acme container when you have another npm infront of mailcow (mailcow has their own reverse proxy in the stack too, didnt touch it, dw :) ).

Solution (step by step):

- In NPM create the proxy host as i described at the start. Add the ssl certificate for mail.mydomain.com to the host, ofc. I used a wildcard but because i had multiple certs in NPM and didnt know which one was the wildcard in the filesystem afterwards (they're named like npm-1, npm-2, etc) i made a new one for just mail.mydomain.com so that i could just look at file creation date and know which one it is.
- Copy the SSL cert and key from NPM to mailcow, i used the following commands for that (adjust the paths for the location of your docker compose stacks)
  -  `sudo cp ~/dockers/nginxproxy/letsencrypt/live/npm-3/privkey.pem ~/dockers/mailcow-dockerized/data/assets/ssl/key.pem`
  -  `sudo cp ~/dockers/nginxproxy/letsencrypt/live/npm-3/fullchain.pem ~/dockers/mailcow-dockerized/data/assets/ssl/cert.pem`
  -  `sudo cp ~/dockers/nginxproxy/letsencrypt/live/npm-3/privkey.pem ~/dockers/mailcow-dockerized/data/assets/ssl/mail.mydomain.com/key.pem`
  -  `sudo cp ~/dockers/nginxproxy/letsencrypt/live/npm-3/fullchain.pem ~/dockers/mailcow-dockerized/data/assets/ssl/mail.mydomain.com/cert.pem`
  -  yeah for some reason i had to copy it to two locations, otherwise the mail servers would not have used it if it were just in the ssl folder and not in the domain name folder too
-  restart the services that use the certs:
  -  ```bash
     docker restart $(docker ps -qaf name=postfix-mailcow)
     docker restart $(docker ps -qaf name=nginx-mailcow)
     docker restart $(docker ps -qaf name=dovecot-mailcow)
     ```

So this should work. Don't forget to do this every 90 days. You could make a little script that checks periodically if the files in the NPM container updated and copy it.

Here is the official mailcow-dockerized documentation regarding ssl stuff: https://docs.mailcow.email/post_installation/firststeps-ssl/#how-to-use-your-own-certificate
