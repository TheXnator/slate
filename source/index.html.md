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

At this point, you should be able to load up xAdmin ingame on your server without any issues. The rest of this guide will include how to use xAdmin and API documentation for developers.

# xAdmin Group Structure

Groups in xAdmin consist of 4 simple features: ID, Power, Col and Perms.

Name | Description
--------- | -----------
ID | The name / unique ID of the group
Power | The power of the group, used for inheritance and targeting (users cannot target those of higher power)
Col | Display colour of the group, used for the menu and admin chat
Perms | Permissions that the group has access to

All of these fields can be easily modified for all groups ingame via the xAdmin menu

# xAdmin Menu

The xAdmin menu contains all of the features of xAdmin, including commands, ranks, bans, utilities and the in-game config.

Each main section of the menu serves a purpose for a section of the addon, with the first being for commands. This tab contains a list of every command in xAdmin and a brief description, sectioned into their relevant categories. By clicking on a command, you can easily input the necessary arguments and execute commands from the menu.
Alternatively, every command can also be executed in chat using one of the configured prefixes (```!``` or ```/``` by default), followed by the ID of the command and the arguments, ie. ```!ban TheXnator 1440 Mass RDM```. When running commands in chat, you can use TAB to autocomplete to the SteamID of the user, and all commands support input of a SteamID in place of the name of the user to target, including the wildcards * (all players), ^ (yourself) and !^ (all players but yourself).
Every command also has a console variant, used with ```xadmin COMMAND_ID ARGS```, ie. ```xadmin ban TheXnator 1440 Mass RDM```.
All these methods of running commands function in the same manner, and it is down to personal preference for which you would like to primarily use.

The ranks section of the xAdmin menu contains easy configuration of all ranks, including creation/deletion of new groups, editing existing groups and their permissions and viewing members of groups.
You can modify a group by clicking on it in the secondary navigation, and can view all permission categories by clicking on the permission category headers to add or remove permissions for the group.

The bans section of the menu contains a list of all existing bans (if there are any). If you click on a ban here, you can modify and/or remove the ban, and view information on it such as the banning admin, the reason, the time of the ban and the length of the ban.

The utilities section includes all of the various extra utilities beyond basic administration that xAdmin has to offer, outlined in the ```xAdmin Utilities``` section of this guide.

The settings tab includes the ingame configuration, with easy access to modify and save any changes to the config, with all the options and their descriptions outlined above.

# xAdmin Utilities

Alongside being a basic admin mod, xAdmin also has a range of extra utilities to offer which server to enhance and improve the administrative experience, whilst also trying to prevent the need to purchase extra addons and attempt to integrate these with xAdmin.

By default, the base version of xAdmin includes 5 different extra utilities.

## Activity

The activity utility allows you to easily view the game time of players. Activity information is stored in the database and can be easily viewed on the client through this section of the menu, with the name of the user, their weekly time and their total time split apart.

## Calendar

The calendar utility allows for staff and users to create public / private events on specific dates (a permission node is needed to make public events). Server Owners could use this feature to publicise when their updates or various events on their server will be, and anyone could use this to set deadlines or keep track of the time.
Calendar entries can be made at any point on the calendar by simply clicking on the day and clicking ```Add Event```. You can click through the months with the navigation buttons on either side of the month and year.

## Rapsheet

xAdmin also includes a rapsheet utility which allows you to easily keep track of any previous punishments of users on your server. Simply click on this utility and select an online player, or search for the SteamID of any online or offline player to view their punishments, including the reason, type of punishment, date and information about the admin.
Click on a punishment to either delete it or copy the full log

## To-Do List

The to-do list utility allows for anyone to create private goals and personal reminders of tasks that they need to complete. Tasks can be easily swapped between being Incomplete, In Progress or Completed, and new tasks can be easily created in this tab. Simply click on a task to edit it or move it to a different section.

## Tickets

The tickets utility adds a built-in report system to xAdmin. This allows your users to easily let staff know when they need assistance with anything. Users can use a configured chat prefix to create a report, which will then pop up on admins' screens. Admins can easily claim tickets, teleport to or bring the player to them through the popup, or they can go into the xAdmin menu and navigate to the tickets section to claim tickets, view tickets which aren't on their screen (popups will fade after \~10 seconds) and delete or mark tickets as finished.

# API / Documentation

## Register Permission

```lua
xAdmin.RegisterPermission(permission, name, category)
```

Register a new permission node with xAdmin.

Parameter | Data Type | Description
--------- |--------- | -----------
permission | String | The unique ID of the permission node
name | String | The neater display name of the permission
category | String | The category to sort the permission under

<aside class="notice">
Permissions registered with CAMI will automatically be added to xAdmin
</aside>

## Register Command

```lua
xAdmin.RegisterCommand(command, name, description, notifystr, category, action, argtypes)
```

Register a new command with xAdmin.

Parameter | Data Type | Description
--------- |--------- | -----------
command | String | The unique ID of the command, used for chat and console commands
name | String | The neater display name of the permission
description | String | The description / help text for the command
notifystr | String | The string used for formatting notifications of command use
category | String | The category to sort the command under
action | Function | The function to run when the command executes (Player, Arguments)
argtypes | Table | The argument types for the command to use and the associated display text

Valid argument types: ```xAdmin.ARG_PLAYER```, ```xAdmin.ARG_NUM```, ```xAdmin.ARG_STRING```, ```xAdmin.ARG_MAP```, ```xAdmin.ARG_GAMEMODE```.

## Register Command Alias

```lua
xAdmin.RegisterCommandAlias(alias, command)
```

Register a new alias for an xAdmin command.

Parameter | Data Type | Description
--------- |--------- | -----------
alias | String | The alias to register
command | String | The ID of the command to register the alias for

<aside class="notice">
A command can have any number of aliases associated to it
</aside>

## Register Group

```lua
xAdmin.RegisterGroup(name, col, power, perms)
```

Register a new xAdmin group.

Parameter | Data Type | Description
--------- |--------- | -----------
name | String | The display name / ID of the group
col | Color | The display colour of the group
power | Int | The power of the group (used for inheritance and targeting)
perms | Table | A table of the group's allowed permission nodes

## Register Config Option

```lua
xAdmin.RegisterConfigOption(name, configID, parseFunc, parseToSQLFunc, inputType)
```

Register a config option to be modifiable ingame.

Parameter | Data Type | Description
--------- |--------- | -----------
name | String | The display name of the config option
configID | String | The ID of the config option to modify
parseFunc | Function | A function for parsing the data from its stored form to the config option
parseToSQLFunc | Function | A function for parsing the config option's data for SQL/MySQL storage (as a string)
inputType | String | The input type required for the option

## Get Rank Members

```lua
xAdmin.GetMembersByRank(rank)
```

Get all members of a given rank (primary members)

Parameter | Data Type | Description
--------- |--------- | -----------
rank | String | The rank to check

### Returns:
Data Type | Description
--------- | -----------
Table | Table of players with the rank

## Get Secondary Rank Members

```lua
xAdmin.GetSecondaryMembersByRank(rank)
```

Get all members of a given rank (secnondary members)

Parameter | Data Type | Description
--------- |--------- | -----------
rank | String | The rank to check

### Returns:
Data Type | Description
--------- | -----------
Table | Table of players with the rank

## Check Permissions

```lua
PlayerMeta:xAdminHasPermission(node)
```

Check if a user has a given permission node

Parameter | Data Type | Description
--------- |--------- | -----------
node | String | The permission to check for

### Returns:
Data Type | Description
--------- | -----------
Boolean | Whether or not the user has the permission node

## Check Targeting

```lua
PlayerMeta:CanTargetGroup(group)
```

Check if a user is able to target a group

Parameter | Data Type | Description
--------- |--------- | -----------
group | String | The group to check for

### Returns:
Data Type | Description
--------- | -----------
Boolean | Whether or not the user is able to target the group

## Check Group

```lua
PlayerMeta:IsUserGroup(group)
```

Check if a user is a member of a group (checks primary and secondary groups)

Parameter | Data Type | Description
--------- |--------- | -----------
group | String | The group to check for

### Returns:
Data Type | Description
--------- | -----------
Boolean | Whether or not the user is a member of the group

# Support

For support, please feel free to open up a support ticket via Gmodstore or join my Discord server here: [https://discord.gg/9YhGQaS](https://discord.gg/9YhGQaS).
If you're joining my Discord server for support, please ensure you read the ```#info``` channel before making a request.