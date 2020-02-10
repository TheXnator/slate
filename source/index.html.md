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

If you want to use family sharing checks on your server, you will need to get a Steam dev API key from [this site](https://steamcommunity.com/dev/apikey). The key you generate on this site can be set as the ```xAdmin.Config.SteamAPIKey``` config option.

The final part of the serverside configuration is the config for the Discord webhook relay. First of all, you will need to create a webhook in the channel on your Discord server that you want the logs to go to. You can find out how to do this on the [Discord support site](https://support.discordapp.com/hc/en-us/articles/228383668-Intro-to-Webhooks). Simply follow the two steps for ```Making a Webhook``` to create the webhook, and then you can fill in the config options.

By default, xAdmin's discord relay will use the proxy on my own webserver, however you are free to also upload a proxy to your own webserver if you wish to. Inside the download of xAdmin, there is a file called ```discordrelay.php```. If you want to have the proxy on your webserver, simply upload this file to the directory on your server that you want it to be in and copy the "privatekey" in this file, and you can then set up the Discord relay settings.

Option | Default | Description
--------- | ------- | -----------
xAdmin.Config.DoDiscordRelay | false | Whether or not to use the Discord relay
xAdmin.Config.RelayURL | "https://www.thexnator.dev/gms/discordrelay.php" | The URL of the Discord relay (only change this if you've uploaded it to your webserver)
xAdmin.Config.RelayKey | "" | The private key for the Discord relay (only change this if you've uploaded it to your webserver)
xAdmin.Config.DiscordWebhook | "" | The webhook for the relay to use - this is the one you set up in the channel you want it to log to

## First-Time Use

The first time you use xAdmin after setting up your serverside config, it is unlikely that you will need to change too much.

The license holder will automatically be added to the Super Admin group ingame initially so you shouldn't even need to add yourself to any groups from your server console - you can add any other users and make any other changes directly from ingame.

The majority of the xAdmin configuration can be done ingame including setting up and modifying groups, however some config options need to be changed in the ```lua/xadmin/sh_config.lua``` file. The table below outlines all config options and whether they can be modified ingame or not.

Option | Editable In-Game? | Description
--------- | ------- | -----------
xAdmin.Config.DefaultGroups | false | The default groups for xAdmin to use (this will only do anything before the first-time setup or if you reset your database)
xAdmin.Config.InitialUserGroups | false | Automatic initial groups to set users too - the license holder will automatically be here as a Super Admin (users can be added to groups ingame)
xAdmin.Config.BanKickMessage | true | Kick message for users who have been banned for hacking - String replacements (%s): Admin, end time, reason
xAdmin.Config.DefaultGroup | true | Default group for new users
xAdmin.Config.MenuCommand | true | Command name to open xAdmin menu
xAdmin.Config.IgnorePower | false | Commands with which users can target those of higher power regardless of rank
xAdmin.Config.BlockLogging | false | Commands which will not be logged to server/client consoles
xAdmin.Config.AllowedPrefixes | false | Allowed prefixes for xAdmin chat commands
xAdmin.Config.AllowRCon | true | Whether or not to allow the use of rcon through ingame commands (not recommended)
xAdmin.Config.TicketsShortPrefix | true | Single character prefix for ticket creation
xAdmin.Config.JailModel | true | Jail entity model filepath
xAdmin.Config.JailMaterial | true | Jail entity material filepath
xAdmin.Config.JailScale | true | Scale of jail entity prop/model
xAdmin.Config.MuteGagOnJail | true | Whether or not to automatically mute and gag users when they have been jailed
xAdmin.Config.BanSharingEvade | true | Whether to automatically ban users who are using a family shared account of a banned user
xAdmin.Config.BanSharingTime | true | How long to ban users for when auto sharing banning (0 = permanent)
xAdmin.Config.BanSharingReason | true | Reason for auto family sharing bans
xAdmin.Config.AutoCheckSharing | true | Whether or not to automatically run a family sharing check when users connect
xAdmin.Config.StaffOnDutyTeam | false | Team to use as the "Staff on Duty" team (function should return the ID of the team)
xAdmin.Config.UseStaffOnDuty | true | Whether or not to enable the default xAdmin staff on duty team
xAdmin.Config.PreventJobChangeAdminMode | true | Whether or not to prevent changing to another job whilst in admin mode
xAdmin.Config.StaffOnDutyWeapons | false | Table of weapons for the staff on duty job to have (if UseStaffOnDuty is set to true)
xAdmin.Config.OnDutyOnlyCommands | false | Groups which may only use commands whilst on duty
xAdmin.Config.GroupPropLimits | false | Specific prop limits for xAdmin groups
xAdmin.Config.CreateToolgunPerms | false | Toolgun types to create permissions for
xAdmin.Config.AdminChatPrefix | true | Prefix to use for admin chat messages
xAdmin.Config.AdminChatUseSteamName | true | Whether or not to use Steam names for admin chat for DarkRP-based gamemodes (false = RP names)
xAdmin.Config.DisabledModules | false | Disabled xAdmin modules
xAdmin.Config.ChatPrefix | true | Chat prefix for xAdmin notifications
xAdmin.Config.ChatPrefixCol | false | Chat prefix colour for xAdmin notifications
xAdmin.Config.MenuName | true | Menu display name
xAdmin.Config.UsingFastDL | true | Set this to true if you're using a FastDL for content downloads