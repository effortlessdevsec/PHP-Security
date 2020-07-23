# PHP-Security
For PHP Security

## .htaccess
* [Custom Error Page](#Custom-Error)
* [Disable Directory Listing](#Disable-Directory)
* [ADD Custom Headers](#Custom-Headers)
* [Restrict File to Access](#forbidden)
* [Remove the Page File Extension From the URL](#RewriteUrl)


## 
* [Broken Session Managment](#Session-fix)


## Custom-Error
How to implement custom error page in PHP website?

```ErrorDocument 404 http://127.0.0.1/FIXING/404.html
   ErrorDocument 403 http://127.0.0.1/FIXING/404.html
  
  ```
  
## #Disable-Directory

```
Options -Indexes
  
  ```
## Custom-Headers

```
Header set X-XSS-Protection "1; mode=block"
Header always append X-Frame-Options SAMEORIGIN
Header set X-Content-Type-Options nosniff
Header unset X-Powered-By
Header add Strict-Transport-Security "max-age=157680000"
  
  ```
  
## forbidden

```
<Files .htaccess>
Order allow,deny
Deny from all
</Files>
  
  ```
  
 ## RewriteUrl
 ```
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}\.html -f
RewriteRule ^(.*)$ $1.html [NC,L]
RewriteRule ^([^\.]+)$ $1.php [NC,L]
  
  ```
 
  
## .htaccess

  ```
AddDefaultCharset UTF-8

<IfModule mod_rewrite.c>

# Enable rewrite rules
RewriteEngine on
Header set X-XSS-Protection "1; mode=block"
Header always append X-Frame-Options SAMEORIGIN
Header set X-Content-Type-Options nosniff
Header unset X-Powered-By
Header add Strict-Transport-Security "max-age=157680000"

ErrorDocument 404 http://127.0.0.1/FIXING/404.html
ErrorDocument 403 http://127.0.0.1/FIXING/404.html

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}\.html -f
RewriteRule ^(.*)$ $1.html [NC,L]
RewriteRule ^([^\.]+)$ $1.php [NC,L]


<Files .htaccess>
Order allow,deny
Deny from all
</Files>

Options -Indexes


</IfModule>
  
  ```
  
 ## Session-fix
