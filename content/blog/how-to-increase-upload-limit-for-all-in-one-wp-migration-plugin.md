---
title: "How to Increase Upload Limit for All in One Wp Migration Plugin"
date: 2019-02-03T12:03:57+09:00
draft: false
description: "A quick and nifty way to increase the All in One Wp Migration Plugin limit so that you can move larger sites without any additional cost"
---

Sometimes you need to do some work quickly, and a manual WordPress migration is not known for being easy. This is where plugins like <a href="https://en-gb.wordpress.org/plugins/all-in-one-wp-migration/" target="_blank" rel="noopener">All-in-One WP Migration</a> come in handy. However, at the moment there is a very small upload limit and if you are not in a position to pay for the upgrade or just don't have the time there is a very quick hack you can do to increase the limit.
<blockquote>This experiment was exactly that, an experiment. I do not condone the misuse of any software and I believe in paying what is due. A lot of time and effort has gone into creating this fantastic and time saving plugin. Please consider supporting the developers properly and paying for the full version.</blockquote>
<h2>What is All-in-One WP Migration</h2>
Created by <a href="https://servmask.com/" target="_blank" rel="noopener">ServMask</a>, All-in-One WP Migrations is a plugin that exports your WordPress website including the database, media files, plugins and themes with no technical knowledge required.
Upload your site to a different location with a drag and drop into WordPress.

Here is the plugin meta:
<pre><code>/**
 * Plugin Name: All-in-One WP Migration
 * Plugin URI: https://servmask.com/
 * Description: Migration tool for all your blog data. Import or Export your blog content with a single click.
 * Author: ServMask
 * Author URI: https://servmask.com/
 * Version: 7.7
 * Text Domain: all-in-one-wp-migration
 * Domain Path: /languages
 * Network: True
 *
 * Copyright (C) 2014-2019 ServMask Inc.
 *
 * This program is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program.  If not, see &lt;http://www.gnu.org/licenses/&gt;.
 *
 * ███████╗███████╗██████╗ ██╗   ██╗███╗   ███╗ █████╗ ███████╗██╗  ██╗
 * ██╔════╝██╔════╝██╔══██╗██║   ██║████╗ ████║██╔══██╗██╔════╝██║ ██╔╝
 * ███████╗█████╗  ██████╔╝██║   ██║██╔████╔██║███████║███████╗█████╔╝
 * ╚════██║██╔══╝  ██╔══██╗╚██╗ ██╔╝██║╚██╔╝██║██╔══██║╚════██║██╔═██╗
 * ███████║███████╗██║  ██║ ╚████╔╝ ██║ ╚═╝ ██║██║  ██║███████║██║  ██╗
 * ╚══════╝╚══════╝╚═╝  ╚═╝  ╚═══╝  ╚═╝     ╚═╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝
 */
</code></pre>
<h2>Increasing the limit of All in One Wordpress Migrate</h2>
This was tested with two different versions:
<ul>
 	<li><a href="https://downloads.wordpress.org/plugin/all-in-one-wp-migration.7.7.zip" target="_blank" rel="noopener">7.7</a></li>
 	<li><a href="https://downloads.wordpress.org/plugin/all-in-one-wp-migration.6.77.zip" target="_blank" rel="noopener">6.77</a></li>
</ul>
Last checked: <span class="highlight">26 Sept 2019</span>
<h3>Prerequisite</h3>
You will need to be able to access and edit the <span class="highlight">.htaccess</span> file that is found in the root directory of the WordPress installation.

Your <span class="highlight">.htaccess</span> file should look something like this:
<pre><code>RewriteEngine On
RewriteBase /client/greg/
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /client/greg/index.php [L]</code></pre>
And what we need to add is the following, between the <span class="highlight">IfModule</span> tags:
<pre><code>php_value upload_max_filesize 6000M
php_value post_max_size 6000M
php_value memory_limit 256M
php_value max_execution_time 300
php_value max_input_time 300
</code></pre>

Your full .htaccess file should now look like so:
<pre><code>RewriteEngine On
RewriteBase /client/greg/
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /client/greg/index.php [L]
php_value upload_max_filesize 6000M
php_value post_max_size 6000M
php_value memory_limit 256M
php_value max_execution_time 300
php_value max_input_time 300</code></pre>

And voila, Now head to the section of your WordPress admin and you should now see the limit has increased to 6GB.
<figure class="text-center"><img src="/img/how-to-increase-upload-limit-for-all-in-one-wp-migration-plugin/all-in-1-migrate-upload-limit.png" alt="All in one WordPress plugin increase limit image" /></figure>
<small>All images (incl logo), trademarks and other copyright content remains the sole ownership of ServMask.I am not affiliated in anyway shape or form with ServMask and this guide is for tutorial and education purposes only.</small>