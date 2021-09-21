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
* [SQL Injection](#SQL-Injection)
* [Unresticted File Upload](#File-Upload)
* [Implement Hidden Google reCAPTHA](#reCAPTHA)

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
 I am taking an example:
 ### login.php
 ```
            session_start();
            session_regenerate_id();
            $_SESSION['logged_in'] = true;
            header("Location: welcome");
  
  ```
  ### welcome.php
  
  ```
            <?php
session_start();
if (! empty($_SESSION['logged_in']))
{
    ?>
    <!DOCTYPE html>
    <html>
    <head>
    	<title></title>
    	<script type="text/javascript" src="welcome.js"></script>
    	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
    	<link rel="stylesheet" type="text/css" href="welcome.css">
    </head>
    <body>

<html>
<head>
    <title>My Web Page</title>
</head>
<body>
  <div class="tree"></div>
</body>
</html>

    </body>
    </html>
    <a href='logout.php' style="margin-left: 200px">log out</a>
    <?php
}
else
{
    header("Location: login");
}
   ```
 ### logout.php
 
 ```
            <?php
session_start();
session_destroy();
header("Location: login");
echo 'You have been logged out. <a href="login">Go back</a>';
  
  ```

## SQL-Injection

Using mysql_real_escape_string()

 ```
    $username = mysqli_real_escape_string($con, $name);  
    $password = mysqli_real_escape_string($con, $pass);
    $sql = "select *from login where name = '$username' and pass = '$password'";
  
  ```
  For Newer PHP VERSIONS Use Perpared statement for example!!
  ```
  $sql = $conn->prepare("SELECT * from exams  where id=?");
                        $sql->bind_param("i", $nid);
                        $sql->execute();
                        $result = $sql->get_result();
                        $count=$result->num_rows;
  ```
  
  
 ## File-Upload
 
 
 ```
<?php
   if(isset($_FILES['image'])){
      $errors= array();
      $file_name = $_FILES['image']['name'];
      $file_size =$_FILES['image']['size'];
      $file_tmp =$_FILES['image']['tmp_name'];
      $file_type=$_FILES['image']['type'];
      $file_ext = pathinfo($file_name, PATHINFO_EXTENSION);
      
      //echo $file_name;
      //echo $file_type;
      //echo $file_name;
      
      $extensions= array("jpeg","jpg","png");
      if(in_array($file_ext,$extensions)=== false){
         $errors[]="extension not allowed, please choose a JPEG or PNG file.";
        
         
      }

      if(empty($errors)==true){
         $newfilename = md5($file_name);
         //echo $newfilename.".".$file_ext;
         $file_path = "images/". $newfilename.".".$file_ext;
         move_uploaded_file($file_tmp,$file_path);
         //echo "<img src=".$file_path." height=200 width=300 />";
         echo "Success";
      }else{
         print_r($errors);

    }

  }
   ?>
  
  ```
 ## reCAPTHA
  ```
  
<?php

if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['recaptcha_response']) && !empty($_POST["recaptcha_response"]) && isset($_POST['recaptcha_response']))
{
// Build POST request:
  $recaptcha_url = 'https://www.google.com/recaptcha/api/siteverify';
  $recaptcha_secret = 'key';
  $recaptcha_response = $_POST['recaptcha_response'];
  echo $recaptcha_response ;

// Make and decode POST request:
  $recaptcha = file_get_contents($recaptcha_url . '?secret=' . $recaptcha_secret . '&response=' . $recaptcha_response);
  $recaptcha = json_decode($recaptcha);

// Take action based on the score returned:
  if ($recaptcha->score >= 0.5) 
  {
    //insert code
  }
}

?>

//client side//

<html>
  <head>
    <title>Google recapcha demo - Codeforgeek</title>
<script src="https://www.google.com/recaptcha/api.js?render=site-key"></script>
<script>
  grecaptcha.ready(function () {
    grecaptcha.execute('site-key', { action: 'contact' }).then(function (token) {
      var recaptchaResponse = document.getElementById('recaptchaResponse');
      recaptchaResponse.value = token;    
    });
  });
</script>
  </head>
  <body>
    <h1>Google reCAPTHA Demo</h1>
    <form id="comment_form" action="" method="post">
      <input type="email" placeholder="Type your email" size="40"><br><br>
      <textarea name="comment" rows="8" cols="39"></textarea><br><br>
      <input type="submit" name="submit" value="Post comment"><br><br>
       <input type="hidden" name="recaptcha_response" id="recaptchaResponse">
    </form>
  </body>
</html>



  ```
