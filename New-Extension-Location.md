## NEW FOR SPRINT 16

The location of Brackets extensions has changed in Sprint 16. 

### User Extensions
User extensions now live in the user's home folder. 

* Mac: `~/Library/Application Support/Brackets/extensions/user`
* Windows XP: `C:\Documents and Settings\<username>\Application Data\Brackets\extensions\user` -- (aka `%appdata%\Brackets\extensions\user`)
* Windows 7 & 8: `C:\Users\<username>\AppData\Roaming\Brackets\extensions\user` -- (aka `%appdata%\Brackets\extensions\user`)

### Extension Development
When developing extensions and running from a [local copy] (https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-setup_for_hacking) of the Brackets source code, you can put your extension in the `src/extensions/dev` directory. This is not required (you can develop extensions in the user extension directory), but is available as a convenience.