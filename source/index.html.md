---
title: xAdmin Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - lua

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:

search: true
---

# Introduction

Welcome to the documentation for xAdmin v2!

This documentation contains information for users and developers of xAdmin, including setup, ingame usage and API documentation for developers.

<aside class="notice">
If you're having issues and need support, please either create a Gmodstore ticket or join my Discord server
</aside>


# Initial Setup

To install xAdmin, you simply need to extract the zip that you have downloaded from gmodstore and place the <code>xadmin</code> folder (containing the <code>lua</code> folder) to your <code>/garrysmod/addons</code> directory on your server.

Once you restart your server, xAdmin will be installed and should be functional. For the most part, xAdmin works as-is from here, but if you want to make any configuration changes (recommended), you should also follow the steps below.

<aside class="notice">
When you join the server for the first time, you (or the license holder) should already be set to the "Super Admin" group
</aside>

## Database / Serverside Configuration

By default, xAdmin will use the built-in SQLite database of your server (sv.db), however it is recommended that you set up an external MySQL database if you are able to.

You can set this up in the ```lua/xadmin/sv_config.lua``` file within the xAdmin addon. You will see a range of configuration options here, we'll start by looking at the database config.

Before you can set up MySQL for xAdmin, you ned to ensure that you have the ```mysqloo``` module installed, otherwise you will get errors when you set ```UseMySQL``` to true. You can find out how to do this here: [https://github.com/FredyH/MySQLOO](https://github.com/FredyH/MySQLOO)

Option | Default | Description
--------- | ------- | -----------
xAdmin.Config.UseMySQL | false | Whether or not to use MySQL - set this to true to use MySQL and false for SQLite
xAdmin.Config.DBINFO.host | "127.0.0.1" | The host IP of your MySQL database
xAdmin.Config.DBINFO.username | "username" | The username used to log in to your MySQL database
xAdmin.Config.DBINFO.pass | "password" | The password for the above user
xAdmin.Config.DBINFO.db | "database" | The database to use - ensure the given user has access to this database
xAdmin.Config.DBINFO.port | 3306 | The port of your database (usually 3306)
xAdmin.Config.TableNamePrefix | "xadmin_" | The prefix for xAdmin table names (you probably don't need to change this)

Alongside the database config, the serverside configuration also consists of config for both the Steam API key used for family sharing checks and Discord relay config.

If you want to use family sharing checks on your server, you will need to get a Steam dev API key from (this site)[https://steamcommunity.com/dev/apikey]. The key you generate on this site can be set as the ```xAdmin.Config.SteamAPIKey``` config option.


## Steam API Key Setup