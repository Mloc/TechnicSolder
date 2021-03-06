TechnicSolder
=============

What is Solder?
--------------

TechnicSolder is an API that sits between a modpack repository and the launcher. It allows you to easily manage multiple modpacks in one single location. It's the same API we use to distribute our modpacks!

Using Solder also means your packs will download each mod individually. This means the launcher can check MD5's against each version of a mod and if it hasn't changed, use the cached version of the mod instead. What does this mean? Small incremental updates to your modpack doesn't mean redownloading the whole thing every time!

Solder also interfaces with the Technic Platform using an API key you can generate through your account there. When Solder has this key it can directly interact with your Platform account. When creating new modpacks you will be able to import any packs you have registered in your Solder install. It will also create detailed mod lists on your Platform page! (assuming you have the respective data filled out in Solder) Neat huh?

Right now Solder is in its very early stages. We will not be offering any help setting up Solder and we recommend you stick to normal custom zips until it becomes more user-friendly. If you want to brave the waters though, be our guest!

Installation
-------------

First thing is first, clone Solder into a directory on your web box. You will want to setup Apache, Nginx, or whatever you are using to use the "public" folder as the document root. This way the main Solder operating files are not publically available through the web.

Next, copy **application/config-sample** to **application/config** and **public/.htaccess-sample** to **public/.htaccess**. Edit the files in the config directory to your liking. The only two files that currently *require* editing are **database.php** and **solder.php**. You will also want to give your box permissions to write to the **storage** folder. You can do this with a chmod or whatever you prefer.

Solder.php has 3 values that need to be filled in:

* **repo_location:** This points to the location of your repo files either locally or remotely. If your Solder install is on the same box as your mod repository, you can fill in the absolute path to those files. This will greatly increase the speed in which md5's are calculated for your database. If your mod repository is hosted elsewhere, you can simply put the web accessible URL in for this value. Make sure to include a trailing slash in *both* cases.
* **mirror_url:** The mirror url is the web accessible url to your mod repository. If you are using a URL for repo_location you can just fill in the same value here.
* **platform_key:** This is the API key given to you through the "Configure Solder API" option on the Platform. When this key is validated by the Platform, it will allow you to directly import your Solder packs to the Platform.

Once you have everything filled in for your configuration, you will need to migrate your database. Doing this is *really* easy, just run the following two commands in order from the root Solder folder.

1. php artisan migrate:install
2. php artisan migrate

This will configure your database and get it ready to handle your first import.

Finally, you will need to just run your cache update script. This can take awhile on its first run depending on the size of your mod repository. Adjust the URL below to reflect your own install and enter it into your browser. Let it do its thing!

> http://yoursolderinstall/cache/update

That's it! Your Solder is configured and ready to rock. Whenever you make changes to your mod repository make sure to run the cache update or your players won't see it.

Updating Solder
---------------

Solder waits for no one! If you use Solder you need to make sure it's up to date. Issues you may be having may have already been resolved in a recent commit. Who knows! Updating is simple.

1. Pull in changes from your origin (git pull origin)
2. Check if any config files were changed (usually not the case) and make adjustments if necessary
3. Run your migrations again (You don't need to do migrate:install, only migrate)

That's it. Your API is now on the latest version.

Troubleshooting
---------------
If you are having an issue, chances are it's something wrong with your YAML files in your repository. The easiest way to check is by looking in your log files found in **storage/logs/**.

If you are *still* having issues and can't seem to figure out what's going on, come ask in irc @ **irc.synirc.net #technic** or open an issue here on Github.

Future of Solder
----------------

Right now as it stands, Solder is very error-prone because it relies heavily on user input into YAML files. If users make simple mistakes with version numbers and such, Solder may not insert data properly.

The goal of Solder is to eliminate the need for YAMLs completely. As time progresses a user interface is planned to manage and control your modpacks. This will elminate user errors and make sure a modpack is always configured properly.