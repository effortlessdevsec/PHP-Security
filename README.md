# PHP-Security
For PHP Security

## .htaccess
* [Custom Error Page](#Custom-Error)
* [Disable Directory Listing](#Disable-Directory Listing)
* [ADD Custom Headers](#Add Custom Headers)

## Custom-Error
How to implement custom error page in PHP website?

```ErrorDocument 404 http://127.0.0.1/FIXING/404.html
   ErrorDocument 403 http://127.0.0.1/FIXING/404.html
  
  ```
  
## Disable-Directory Listing

```
Options -Indexes
  
  ```
## Add Custom Headers

```
Header set X-XSS-Protection "1; mode=block"
Header always append X-Frame-Options SAMEORIGIN
Header set X-Content-Type-Options nosniff
Header unset X-Powered-By
Header add Strict-Transport-Security "max-age=157680000"
  
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

Options -Indexes

</IfModule>
  
  ```
  
  
