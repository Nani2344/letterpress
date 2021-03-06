Letterpress
-----------

This is a web-based clone of the popular iOS word-game Letterpress using
purely PHP and Javascript.


Dev Setup Instructions
----------------------

### Minimum Requirements

* Running Apache Server
* PHP 5.3.6+. May work on PHP 5.2.x for now, but no guarantees it will continue to work going forward
* MySQL Server + Client
* git client v1.7+

### Setup Instructions

1. Fork and Clone GitHub repo
    * Fork the repo on github by going to
      https://github.com/meetrajesh/letterpress and clicking 'Fork' on the
      top-right-hand corner.
    * Change into your phpwebroot folder (where you store all your PHP
      projects) and git clone your new fork: <code>git clone
      https://github.com/[username]/letterpress.git</code>. Remember where
      you do this checkout. You will need it later. Recommended location is
      ~/phpwebroot

1. Setup Apache VHost
    * Setup a vhost in your apache config file as shown below. On Mac, the
      config file lives inside /etc/apache2/httpd.conf. On Linux, it's
      typically at /etc/httpd/conf/httpd.conf. Depends on your distro.
 
      <pre>
      ### for letterpress ###
      &lt;VirtualHost *:80>
        ServerName localhost
        DocumentRoot "[webroot]>"
        &lt;Directory "[webroot]">
          Options +FollowSymlinks
          Order allow,deny
          Allow from all
          AllowOverride All
        &lt;/Directory>
      &lt;/VirtualHost>
      </pre>
    * Replace <code>[webroot]</code> with the full path of the parent
      directory that contains your letterpress git repo that you checked out
      in the previous step. So if you checked out your repo at
      <code>/home/john/phpwebroot/letterpress</code>, your [webroot] will be
      <code>/home/john/phpwebroot</code>
    * The <code>AllowOverride All</code> line is important. Don't forget
      it. It allows you to setup URL rewriting rules via an Apache-specific
      .htaccess config file which you can view here:
      https://github.com/meetrajesh/letterpress/blob/master/.htaccess
    * Restart Apache for the changes to take effect: <code>sudo
      /etc/init.d/httpd restart</code> on Linux, and <code>sudo
      /usr/sbin/apachectl restart</code> on Mac OS X.

1. Setup MySQL DB
    * Import the MySQL database schema locally. I've assumed you have a blank
      user with a blank password that has admin (or at least database
      creation) privileges:
 
      <pre>
      $ mysqladmin create letterpress
      $ mysql letterpress &lt; /home/john/phpwebroot/letterpress/schema.sql
      </pre>

1. Override Config
   * Create a file called <code>init.local.php</code> in your letterpress root
     directory and copy+paste this in there:

     <pre>
     &lt;?php
     
     define('BASE_URL', 'http://localhost');
     define('IS_DEV', true);
     define('CSRF_SECRET', '&lt;put some random string here thats about 40 chars long>');
     
     // database credentials
     define('DBHOST', 'localhost');
     define('DBUSER', '');
     define('DBPASS', '');
     define('DBNAME', 'letterpress');
     </pre>
   * Replace your local database credentials if different from the constants above.
   * Replace the CSRF secret with a randomly generated string. Use this PHP
     code if you want to generate a 32-character random string:

     <pre>
     $secret = '';
     foreach (range(1, 32) as $i) {
         $secret .= chr(rand(33, 126));
     }
     echo $secret;
     </pre>

1. Navigate to http://localhost/letterpress on your browser. If everything
   was set up properly, the game should load successfully. If not, ping me at
   rajesh@meetrajesh.com and we'll work it out together.

### Pull Request

1. Once you're done committing and pushing all your changes to your fork,
   submit a pull request using github from your master branch to my master
   branch.

1. Please try to adhere to the coding style already found in the project
   before you submit your pull request. Most notably:
    * Tabs instead of spaces for indentation (except in README.md where we use spaces)
    * Opening brace on same line. Closing brace on new line.
    * Braces mandatory even for 1-line cases
    * Exactly 1-space before opening brace
    * Exactly 1-space after these keywords: if, elseif, for, foreach, while, and switch
    * Exactly 1-space after commas in argument lists