Install the simplesam php drupal module (simplesamlphp_auth)

composer require 'drupal/simplesamlphp_auth:^4.0'

configure the simplesaml php
  download the simplesamlphp library from the official website (https://simplesamlphp.org/download/)
  place it inside the module root directory or any place you prefer
  make sure your site is HTTPS enabled
  add the vhost as mentioned below setting
  ``
  <VirtualHost *:443>
    ##ServerAdmin webmaster@dummy-host2.example.com
    DocumentRoot "C:/drupal/site2/web"
    ServerName site2.dd
	  ServerAlias www.site2.dd
    ##ErrorLog "logs/dummy-host2.example.com-error.log"
    ##CustomLog "logs/dummy-host2.example.com-access.log" common
	  SSLEngine on
	  SSLCertificateFile "C:/wamp64/bin/apache/apache2.4.59/conf/certificate.crt"
	  SSLCertificateKeyFile "C:/wamp64/bin/apache/apache2.4.59/conf/private_key.pem"

	  SetEnv SIMPLESAMLPHP_CONFIG_DIR  "C:/drupal/site2/web/simplesamlphp/config" #add the config folder path
	  Alias /simplesaml "C:/drupal/site2/web/simplesamlphp/public" #add the public folder path
  	<Directory "C:/drupal/site2/web/">
  	  Options FollowSymLinks
        AllowOverride All
  	  Require all granted
     </Directory>
 </VirtualHost>
``
 restart the server. 
