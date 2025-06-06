1. Install the simplesam php drupal module (simplesamlphp_auth)

  ```composer require 'drupal/simplesamlphp_auth:^4.0'```

2. configure the simplesaml php
	  - download the simplesamlphp library from the official website (https://simplesamlphp.org/download/)
	  - place it inside the drupals root directory or any place you prefer
	  - make sure your site is HTTPS enabled
	  - add the vhost as mentioned below setting and adjust the saml alias and config folder path accordingly.
	  ```
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
	```
  	- add the configuration to simplesaml config file. we need to add config.php, authsource.php file in the config folder and metadata file in the metadata folder. Follow the instrution added in the drupal doc https://www.drupal.org/docs/contributed-modules/apigee-developer-portal-kickstart/integrate-simplesamlphp-authentication
    	- update config.php file with the below values. place below code just after <?php tag	
	    ```
	    	require $_SERVER['DOCUMENT_ROOT'] . '/sites/default/settings.php';
		$host = $_SERVER['HTTP_HOST'];
		$db = $databases['default']['default'];
	    ```
	 - place the below code at the bottom of the config.php file or update directly in confg array.
	    ```
	    	// Set some security and other configs that are set above, however we
		// overwrite them here to keep all changes in one area.
		$config['technicalcontact_name'] = "Your Name";
		$config['technicalcontact_email'] = "your_email@yourdomain.com";
		
		// Change these for your installation.
		$config['secretsalt'] = '[YOUR-SECRET-SALT-ANY-STRING]';
		$config['auth.adminpassword'] = '[ADMIN-PASSWORD]';
		$config['admin.protectindexpage'] = TRUE;
		
		// Base URL
		$config['baseurlpath'] = 'https://'. $host .'/simplesaml/';
		
		// SQL Connection
		$config['store.type'] = 'sql';
		$config['store.sql.dsn'] = sprintf('%s:host=%s;port=%s;dbname=%s', $db['driver'], $db['host'], $db['port'], $db['database']);
		$config['store.sql.username'] = $db['username'];
		$config['store.sql.password'] = $db['password'];
		$config['store.sql.prefix'] = 'simplesaml';
		
		// Temp directory must be writable
		$config['tempdir'] = '/path/to/tmp/dir';
	   ```
        - Update authsource.php file. Add the unique sp entity id.
	  ```$config['default-sp']['entityID'] = '[UNIQUE-ID-OFTEN-A-DOMAIN-NAME]';```
 	- add below line after the ```RewriteCond %{REQUEST_URI} !/core/modules/statistics/statistics.php$```
		```
	 	 RewriteCond %{REQUEST_URI} !^/simplesaml
		```
       - command to generate random salt
		```LC_ALL=C tr -c -d '0123456789abcdefghijklmnopqrstuvwxyz' </dev/urandom | dd bs=32 count=1 2>/dev/null;echo```
       - Create cert and private key
       		```openssl req -newkey rsa:3072 -new -x509 -days 3652 -nodes -out saml.crt -keyout saml.pem```
       - genrate metadata file from the converter for the given idp xml data simplesaml/module.php/admin/federation/metadata-converter
       - place the file in the metadata folder (metadata/saml20-idp-remote.php)
       - to point if the metadata folder is not in default place
         	```
	 	/*
	         * This option allows you to specify a directory for your metadata outside of the standard metadata directory
	         * included in the standard distribution of the software.
	         */
                'metadatadir' => 'C:/drupal/site2/web/simplesamlphp/metadata',
	 	```
3. restart the server.
4. At this moment we should able to access saml configuration by /simplesaml, and admin configuration by going to the admin configuration /simplesaml/admin
