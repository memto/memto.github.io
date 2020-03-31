---
layout: post
title: Add Rename Tab Shortcut Gnome Connection Manager
categories: tooltips
tags: gcm
---

#### Ref:
	http://kuthulu.com/gcm/

Gnome Connection Manager is a good utility for you to connect to remote host using SSH. In my usage, I see that It would be nice if we have a shortcut to rename tab when you have multiple tab open. To do that I have added some code here

On my system, gnome-connection-manager is intalled into /usr/share/gnome-connection-manager/gnome_connection_manager.py. So let open it and add some code as below:

	Step1: add _RENAME_TAB var right after _CONNECT at line 246
	
	_CONSOLE_RECONNECT = ["console_reconnect"]
	_CONNECT = ["connect"]
	_RENAME_TAB = ["rename_tab"]


	Step2: add _RENAME_TAB to shortcuts table also after code for _CONNECT at line 1322
    
    try:
        scuts[cp.get("shortcuts", "rename_tab")] = _RENAME_TAB
    except:
        scuts["CTRL+T"] = _RENAME_TAB

    Here I use CTRL+T becase CTRL+R are used for search history command in linux (you can remember it with Tab or Title)

    
    Step3: Add code to handle _RENAME_TAB also after code for _CONNECT at line 586

    elif cmd == _CONNECT:
        self.on_btnConnect_clicked(None)
    elif cmd == _RENAME_TAB:
        curLabel = wMain.nbConsole.get_tab_label(widget.get_parent()).label
        curTitle = curLabel.get_text().strip()
        text = inputbox(_('Renombrar consola'), _('Ingrese nuevo nombre'), curTitle)
        if text != None and text != '':
            curLabel.set_text("  %s  " % (text))


Now you can re-run gcm and press CTRL+T to rename tab