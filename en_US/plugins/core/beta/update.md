# Update Center
**Settings → System → Update Center**


THE **Update Center** allows you to update all of Jeedom&#39;s functionality, including the core software and its plugins.
Other extension management functions are available (delete, reinstall, check, etc.).


## Functions of the page

At the top of the page, independent of the tab, are the control buttons.

Jeedom periodically connects to the Market to see if updates are available. The date of the last check is indicated at the top left of the page.

At the opening of the page, if this verification is more than two hours old, Jeedom automatically redo a verification.
You can also use the button **Check for updates** To do it manually.
If you want to perform a manual check, you can press the button "Check for updates".

The button **To safeguard** is to be used when you change the options in the table below, to specify not to update certain plugins if necessary.

## Update the Core

The button **Update** allows you to update the Core, plugins, or both.
Once you click on it, you get these different options :
- **Pre-update** : Allows you to update the update script before applying the new updates. Generally used at the request of the support.
- **Save before** : Back up Jeedom before updating. The backup is performed locally only (neither Market nor Samba).
- **Update plugins** : Allows you to include plugins in the update.
- **Update the Core** : Allows you to include the Jeedom kernel (the Core) in the update.

- **Forced mode** : Perform the update in forced mode, that is to say that, even if there is an error, Jeedom will continue and will not restore the backup. (This mode disables saving !).
- **Update to reapply** : Allows you to re-apply an update. (NB : Not all updates can be re-applied.)

> **Important**
>
> Before an update, by default, Jeedom will make a backup. In the event of a problem when applying an update, Jeedom will automatically restore the backup made just before. This principle is only valid for Jeedom updates and not for plugin updates.

> **Advice**
>
> You can force an update of Jeedom, even if it does not offer one.

## Core and Plugins tabs

The table contains the versions of the Core and installed plugins.

The plugins have a badge next to their name, specifying their version, green in color *stable*, or orange in *beta* Or other.

- **Status** : OK or NOK.
- **Name** : Name and origin of the plugin
- **Version** : Indicates the precise version of the Core or plugin.
- **Options** : Check this box if you do not want this plugin to be updated during the global update (Button **Update**).

On each line, you can use the following functions:

- **Reinstate** : Force resettlement.
- **DELETE** : Allows you to uninstall it.
- **Check** : Query source for updates to find out if a new update is available.
- **Update** : Allows you to update the element (if it has an update).
- **Changelog** : Allows access to the list of changes in the update.

> **Important**
>
> If the changelog is empty but you still have an update, it means that the documentation has been updated. It is therefore not necessary to ask the developer for changes, since there are not necessarily any. (it is often an update of the translation, documentation).
> The plugin developer can also in some cases make simple bugfixes, which do not necessarily require updating the changelog.

> **Advice**
>
> When you launch an update, a progress bar appears above the table. Avoid other manipulations during the update.

## OS/Package tab

> **Important**
>
> This tab is reserved for advanced users and only for advanced users, the slightest bad action here can BREAK your Jeedom (without the possibility of resorting to support)

This tab allows you to see the updates available for the os (apt), python package (pip2 and pip3) as well as update the packages that require it. 

## Information tab

During or after an update, this tab allows you to read the log of this update in real time.

> **Note**
>
> This log normally ends with *[END UPDATE SUCCESS]*. There may be some error lines in this type of log, however, unless there is a real problem after update, it is not always necessary to contact support for this.

## Command line update

It is possible to update Jeedom directly in SSH.
Once connected, this is the command to perform :

```sudo php /var/www/html/install/update.php```

The possible parameters are :

- **fashion** : `force`, to launch an update in forced mode (ignores errors).
- **Version** : Version number tracking, to re-apply changes from this version.

Here is an example of syntax to make a forced update by reapplying the changes since 4.0.04 :

```sudo php  /var/www/html/install/update.php mode=force version=4.0.04```

Attention, after an update on command line, it is necessary to re-apply the rights on the Jeedom folder :

```sudo chown -R www-data:www-data /var/www/html```
