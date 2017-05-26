### set up nginx to read a local cert for https://dev.udacity.com

```sh
brew install nginx
      # OSX ships with outdated version of nginx
cp ./nginx.conf /usr/local/etc/nginx/
      # copy the given nginx.conf with server set up in 3 location blocks to match 3 services used: classroom-content (backend), coco-web, classroom-web
mkdir ssl
openssl dhparam -out /usr/local/etc/nginx/ssl/dhparam.pem 2048
openssl req -x509 -extensions v3_req -nodes -days 3650 -newkey rsa:2048 -keyout nginx.key -out nginx.crt -subj "/C=US/ST=CA/L=San Fransisco/O=Udacity/OU=Workspaces/CN=udacity.com" -config "./openssl.cnf"
      # creating a new cert the host name is set to: *.udacity.com
      # after creating the cert, browse to https://dev.udacity.com and get served an untrusted cert from nginx
      # from the danger page select advanced view cert, drag to desktop, open in keychain, save as system cert
      # set trust level on cert to system wide. reload browser and it should be trusted and green.
openssl x509 -text -noout -in ./nginx.crt
      # verify cert is valid and check SubjectAltNames came out ok
mv nginx.crt ssl/
mv nginx.key ssl/
cd ssl
chmod 400 *
sudo nginx
cd /usr/local/var/log/nginx
cat *
cat /etc/hosts
   # verify you are redirecting for every server block in nginx.conf
   # 127.0.0.1  cc.udacity.com 		      # classroom-content
   # 127.0.0.1  dev.udacity.com       	# web-terminal
   # 127.0.0.1  web.udacity.com 	      # classroom-web
sudo nginx -s reload
    # to apply changes to nginx.conf, such as adding new location blocks
    # added reference to include mime.types to fix this note in dev tools
    # Resource interpreted as Stylesheet but transferred with MIME type text/html
```
