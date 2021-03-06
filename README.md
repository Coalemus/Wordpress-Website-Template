# Website-Templates

## Server

Apache

## Dev Logs

### Wordpress setup

- `wget https://wordpress.org/latest.tar.gz` to initialize Wordpress in the repository

- `tar xvzf latest.tar.gz` to extract compressed Wordpress contents

- Applied new theme template to repository named "BaseWebsite0.1"

- Edited `/etc/hosts` to have sites for Wordpress

- **mod_rewrite** module ￼enabled￼ by ￼uncommenting `LoadModule rewrite_module modules/mod_rewrite.so`￼ in ￼`/etc/httpd/conf/httpd.conf`￼.

- Created config file `/etc/httpd/conf/extra/httpd-wordpress.conf`

- Initialized wordpress at `usr/share/webapps`for `/etc/httpd/conf/extra/httpd-wordpress.conf` file.

- Added `Include conf/extra/httpd-wordpress.conf` to `/etc/httpd/conf/httpd.conf`.

- Added `DirectoryIndex index.php` within the `<IfModule dir_module>` block of `/etc/httpd/conf/httpd.conf`.

- Installed `MariaDB` and `phpmyadmin`.

 To install mariaDB

```
mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```

- Enabled and started `mariadb.service`

- Improved initial security `sudo mysql_secure_installation`

- Logged in as root in `MariaDB` and added a user.

- User is `joey@localhost` pass is the usual. Followed guide and added permissions for database `mydb`.

```
CREATE USER 'joey'@'localhost' IDENTIFIED BY '***';
```

```
GRANT ALL PRIVILEGES ON mydb.* TO 'joey'@'localhost';
```

```
FLUSH PRIVILEGES;
```

```
quit
```

- Added `wordpress` database and added access to user `joey`.

```
CREATE DATABASE wordpress;
```

> note that the Identified by parameter is the database password.

```
GRANT ALL PRIVILEGES ON wordpress.* TO "joey"@"localhost" IDENTIFIED BY "***";
```

```
FLUSH PRIVILEGES;
```

```
quit
```

- Set up Apache to use PHP as outlined in the [Apache HTTP Server#PHP](https://wiki.archlinux.org/title/Apache_HTTP_Server#PHP "Apache HTTP Server") article.

- Create the Apache configuration file:

```
sudo nano /etc/httpd/conf/extra/phpmyadmin.conf
```

```

Alias /phpmyadmin "/usr/share/webapps/phpMyAdmin"
<Directory "/usr/share/webapps/phpMyAdmin">
    DirectoryIndex index.php
    AllowOverride All
    Options FollowSymlinks
    Require all granted
</Directory>
```

- And include it in `/etc/httpd/conf/httpd.conf`

```
Include conf/extra/phpmyadmin.conf
```

#### phpMyAdmin configuration

- After making changes to the Apache configuration file, [restart](https://wiki.archlinux.org/title/Restart "Restart") `httpd.service`.

```
sudo systemctl restart httpd.service
```

- To allow the usage of the phpMyAdmin setup script

```
sudo mkdir /usr/share/webapps/phpMyAdmin/config
```

```
sudo chown http:http /usr/share/webapps/phpMyAdmin/config
```

```
sudo chmod 750 /usr/share/webapps/phpMyAdmin/config
```

- Went to `http://localhost/phpmyadmin/setup` to set up phpMyAdmin

- Installed `php-apache`

- edit `/etc/php/php.ini` to uncomment bz2 extension and added mariadb `extension=pdo_mysql`
`extension=mysqli` and iconv extensions.

- Setup PHP in Apache using the libphp method

```
sudo nano /etc/httpd/conf/httpd.conf
```

- commented `#LoadModule mpm_event_module modules/mod_mpm_event.so`

- uncommented `LoadModule mpm_prefork_module modules/mod_mpm_prefork.so`

Place this at the end of the `LoadModule` list:

```

LoadModule php_module modules/libphp.so
AddHandler php-script .php
```

Place this at the end of the `Include` list:

```
Include conf/extra/php_module.conf
```

```
sudo systemctl restart httpd.service
```

- Removed `/usr/share/webapps/phpMyAdmin/config.ini.php` to fix Error at setup script site that says `Configuration already exists, setup is disabled!`

- Logged in phpMyAdmin at `http://127.0.0.1/phpmyadmin` using my MariaDB credentials

- went to `http://joey/wordpress` where it redirected me to `http://joey/wordpress/wp-admin/setup-config.php` to setup Wordpress.

- `sudo nano /<https://github.com/wilsmex/blog-site-template.gitaccess> it:

```
chown http:http -R /usr/share/webapps/wordpress/
```

### Theming Setup

Cloned theme-template from `https://github.com/wilsmex/blog-site-template.git`

Added template files to new theme folder called `base-website`

- Add subfolders to the `newthemehere` folder
  - assets
    - css
    - fonts
    - images
    - javascript
  - classes (For PHP classes)
  - inc (Includes - misc files)
  - template-parts (for splitting up parts of template)
  - templates

- Setup files required for wordpress themes
  - style.css (Master style sheet for website)
  - index.php (serves as the fallback if no specified template file in the file hierarchy is found by wordpress)

- Add file for serving a Server error page
  - 404.php

- Add file responsible for deliverring an archive (Like list of blogs)
  - archive.php

- For displaying and serving up comments in theming
  - comments.php

- Bottom section file
  - footer.php

- Top secftion file
  - header.php

- file for overide and initiate different features of your theme
  - functions.php

- File for displaying static pages
  - page.php

- File to display search results
  - search.php

- File for displaying single blog posts
  - single.php

- For basic legal and reminders (and dev logs)
  - read.md

- Image for what that theme lookslike
  - screenshot.png

- File for front-page of the Template
  - front-page.php

### Template Building

- Copied `index.html` @ `blog-site-theme-template` to `front-page.php`

- Enqueued stylesheets in `functions.php`

- Replaced hardcoded stylesheets in `front-page.php` into 

```
<?php

	// wordpress injects all of the stylesheets by itself
	wp_head();

?>
```

- Added dynamic version variable in `functions.php`

- Moved footer and header to `header.php` and `footer.php` respectively.

- Added dynamic title tag and edited title and tagline at `Wordpress_Dashboard>General_Settings`

- Media added at the `Wordpress_Dashboard > Media > New` are resized automatically by Wordpress
  - You could set sizes of the small, medium, large media at `Media > Settings`

- Set dynamic Content Management System

- `Wordpress_Dashboard > Pages > Add New`
  - Added `Home Sweet Home` page with basic title and a word as content for now.

- `Wordpress_Dashboard > Settings > Reading`
  - Set to display a Static page and set home page to the `Home Sweet Home` page.

- Removed all hard coded content at the `front-page.php` file and left the article part.
  - Added Wordpress Loop inside the Article tag.
    - this displays the contents of the static page `Home Sweet Home`.

- Made the heading at `header.php` dynamic.

- modified code of Sidebar Menu from hard coded to dynamic.
  - Added a function to `functions.php`.

- `Wordpress_Dashboard > Appearance` has menu option now.
  - Go to `Menus` and create a new Menu `main-Menu`

- Add new Pages
  - `About Page`
  - `Contact Us`

- Go to `Wordpress_Dashboard > Appearance > Menus` and add these pages to Menu:
  - `Contact Us`
  - `About Us`
  - `Sample Page`
  - Toggle the Desktop Primary Sidebar Location and click Save menu.

- Add primary menu code at `header.php` to make the code dynamic.
  - At `Wordpress_Dashboard > Appearance > Menus` there is the Screen Options, 
    - toggle the Link Targets, and CSS Classes boxes.
    - Then add CSS Classes to the Menu Pages.

- Edit `style.css`
  - element `.blog-nav .nav-link` to `.blog-nav .nav-link`
  - element `.header nav-item nav-link` to `.header nav-item a`
  - element `.header nav-item nav-link:hover` to `.header nav-item a:hover`
  - element `.header .nav-item.active .nav-link` to `.header .nav-item.active a`
  - element `.header .nav-item.active .nav-link:hover` to `.header .nav-item.active a:hover`
  - element `.blog-nav .nav-link` to `.blog-nav a`
  - element `.blog-nav .nav-link:hover` to `.blog-nav a:hover`

- Edit shortcut icon href to `"/wp-content/themes/basewebsite/assets/images/logo.png"`
  - Paste icon from Blog-Site-Template to the above link.

- Add custom logo function at `functions.php`

- Go to `Wordpress_Dashboard > Themes`  go to your theme, click customize, go to Site Identity, click select logo and select file.
  - Also apply as the icon too.

- Add logo css to menu.

- Add dynamic code for Site title in the menu.

- Add new posts for blog.

- Add thumbnail theme support at `functions.php`.

- Add thumbnails in Media library.

- Add all of `front-page.php` to `single.php`.

- In `single.php` replace `the_content()` into a `get_template_part()` function.

- add `template-parts/content-article.php`.

- get elements from `post.html` and paste into `content-article.php`.

- Make date, tags, and comments dynamic in `single.php`.

- get elements from `post.html` and paste into `comments.php`.

- Make Comment body dynamic ( CUSTOMIZE IT YOURSELF BY ADDING FLAGS )

- Copy `single.php` into `archive.php`

- Duplicate `template-parts/content-article.php` into `template-parts/content-archive.php` and reduce into `the_excerpt();` only.

- Make a `Blog` page.

- Set `Blog` page into Post Page in `Reading_Settings > Your Homepage displays`

- Add `Blog` page to Menu and apply same classes as before

- Copy `archive.php` to `index.html` page

- Create more than 10 posts to have a new page (since 10 pages are default cap for each archive page.)

- Edit `content-archive.php` refering the `archive.html` to display more than the excerpt.
  - Add dynamic title, date, comment number, thumbnail, and permalink.
  - Dynamic reading time is TODO separate from tutorial.
  - Set blog post limit in `Settings > Reading` from default 10 to 4 to see pagination links.
  - Add pagination function at `index.php`

- Make a new file `template-parts/content-page.php` where it only shows `the_content();`

- Copy `single.php` to `page.php`.

- Add widgets by adding function to `functions.php`

- Add new `shortcode` widget to sidebar 1.
  - Add `dynamic_sidebar();` to `header.php`.
  - Cut out hard coded social icons from `header.php` to the `shortcode` widget

- Add new `shortcode` widget to footer 1.
  - Add `dynamic_sidebar();` to `footer.php`.
  - Cut out hard coded social icons from `footer.php` to the `shortcode` widget

- Add `404.php` template by copying `index.php`
  - delete php tags and fill in with H1 of Page not Found. And search php tag.

- Copy `archive.php` to `search.php`
