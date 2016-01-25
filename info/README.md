
# UltraEdit/UEStudio Wordfiles
---- 
### Who can use wordfiles.uew as is, without any modification?
The file `wordfiles.uew` can be used as is if you are using:

   - UE  - UltraEdit for Windows     v15.00 or any later 1)
   - UEP - UltraEdit Portable (UEm)  v15.00 or any later
   - UE3 - UltraEdit for U3 drives   v15.00 or any later 2)
   - UEX - UltraEdit for Linux       v2.2.0 or any later
   - UEM - UltraEdit for Apple MAC   v2.2.0 or any later
   - UES - UEStudio  for Windows     v9.10  or any later

AND you can see in the Syntax Highlighting configuration dialog the path to a directory containing 1 or more `*.uew` files each containing exactly 1 syntax highlighting language definition.

Extract or copy `wordfiles.uew` into the directory displayed in the Syntax Highlighting configuration dialog and next time a `*.uew` file is opened, it should be syntax highlighted with code folding for the color groups and displaying the defined color group names in the Function List view.

You can edit the third line in "wordfiles.uew" and change it to:

`/Function String = “%/^(C[1-9][0-9]++”*”^)”`

to see not just the name of a color group, but also the number of the color group at beginning. That is helpful to see quickly when a color group number is used twice by mistake within a wordfile with just a single syntax highlighting language definition.


---- 


### Who must modify wordfiles.uew before it can be used?

Versions of UltraEdit prior the versions listed above *do not* support unlimited wordfiles with file extension UEW stored in a directory.

If such a version of UltraEdit is used, or in a later version there is used still a single wordfile with multiple syntax highlighting language definitions inside, it is necessary to first open and edit `wordfiles.uew` before the content can be copied into the wordfile specified in the Syntax Highlighting configuration dialog.


#### What do I edit?

##### 1. The language number `/L20` at top of `wordfiles.uew`.

   A wordfile with multiple syntax highlighting language definitions can contain only up to 20 languages. (UltraEdit for Windows prior v9.00 support even only up to 10 languages.) Search in the currently used wordfile for lines starting with `/L` and use next free number. For example if the currently used wordfile contains `/L1` to `/L13`, the number on first line of "wordfiles.uew" should be modified to:

`/L14"IDM Wordfiles"`


##### 2. The file extension UEW at top of "wordfiles.uew".

   The file `wordfiles.uew` is defined to syntax highlight all files with file extension UEW in any case. But if the wordfile currently used by your version of UltraEdit has for example the original default file name `wordfile.txt`, the string

`File Extensions = UEW`

at end of first line of "wordfiles.uew" must be replace by

`File Names = wordfile.txt wordfile.new wordfile.old`

to apply the syntax highlighting language "IDM Wordfiles" only to files with the non case-sensitive name `wordfiles.txt`, `wordfiles.new`, or `wordfiles.old`.

`wordfiles.new` was created on update of UltraEdit by the installer when not replacing the currently used `wordfile.txt` and instead installs the new default wordfile as `wordfile.new` into the program files directory of UltraEdit. You can omit file name `wordfile.new` if you don't have such a file in the program files directory of UltraEdit.

`wordfiles.old` was created as backup of currently used `wordfiles.txt` on update of UltraEdit by the installer in the program files directory of UltraEdit when replacing the currently used wordfile. If you don't have such a file in program files directory of UltraEdit, you can omit that file name on first line of `wordfiles.uew`.


##### 3. The `/Function` String regular expression search string on third line.

The UltraEdit regular expression search string on third line of `wordfiles.uew` is designed for displaying the names of the up to 20 color groups in the Function List view. UltraEdit prior v17.00 and UEStudio prior v11.00 support only up to 8 color groups and therefore it makes sense to adapt also this search string for the lower number of color groups. Further for a wordfile containing multiple syntax highlighting language definitions, it is very useful to get the names of the languages listed in the Function List view. For details on how to change the function string, please read also the next chapter.

After making the changes as suggested, the content of `wordfiles.uew` can be copied and appended to currently used wordfile best opened by opening the Syntax Highlighting configuration dialog, pressing button Open and closing the configuration dialog with button Cancel.


---- 


#### How to customize the function list view

##### Modify expression for finding only up to 8 color groups.

UltraEdit prior v17.00 and UEStudio prior v11.00 support only up to 8 color groups for a syntax highlighting language, but later versions support up to 20 color groups.

The UltraEdit regular expression

`/Function String = "%/C[1-9][0-9]++"^(*^)""`

finds lines starting with `/C` with a number in range 1 to 99 with a color group name in double quotes. But if you are using a version of UltraEdit supporting just up to 8 color groups or you plan to define a new wordfile for public usage, the regular expression search string on third line of `wordfiles.uew` should be changed to:

`/Function String = "%/C[1-8]"^(*^)""`


##### List the language names in Function List view instead of the color group names.

With 10 or more syntax highlighting languages within a wordfile listing all color group names is perhaps less useful than listing all language names. Therefore it could be better to replace the character C in the function string definition line by character L to get:

`/Function String = "%/L[1-9][0-9]++"^(*^)""`

which results in displaying the up to 20 language names (or just 10 for really old versions of UltraEdit) in Function List view.


##### Get color group and language names listed in Function List view.

Instead of listing either the color group names or the language names it could be also useful to list both. That is possible by using for example:

`/Function String = "%/^(C[1-8]"*"^)"`
`/Function String 1 = "%/L[1-9][0-9]++^("*"^)"`

The first expression without the number left of the equal sign is for displaying the color group names for `/C1"..."` to `/C8"..."` with displaying also `Cx` and the double quotes for getting the color group names always displayed at bottom of the Function List view if sorting the strings in the view alphabetically is enabled as it is by default.

The second expression with number 1 is for displaying the language names for `/L1"..."` to `/L20"..."` with displaying just the language names with the double quotes. That results in showing the language names at top of the function list view when sorted alphabetically.

If sorting of the strings in Function List View is not enabled, or you want to see also the language numbers, move `^( `in the regular expression left between `/` and character `L`.


##### Use grouped function strings for Function List view.

UltraEdit v16.00 and UEStudio v10.00 or any later version support also grouped function strings with displaying the found strings in a tree (or optionally as flat list). When using always the flat display of the Function List view, the 2 function string definitions as written above should be used. But if the tree display is used as it is default, the language and color group names could be displayed in function list view in a tree. That can be achieved by replacing the line:

`/Function String = "%/C[1-9][0-9]++"^(*^)""`

in `wordfiles.uew` by the block:

`/TGBegin "Languages"`
`/TGFindStr = "%/L[1-9][0-9]++"^(*^)""`
`/TGBegin "Color Groups"`
`/TGFindStr = "/^(C[1-9][0-9]++"*"^)[~"^p]++$"`
`/TGFindBStart = "%/L[0-9]+"`
`/TGFindBEnd = "^{^p/L[0-9]+^}^{^p$^}"`
`/TGEnd`
`/TGEnd`

Please note that there must be a blank line at end of the wordfile (file ends with 0D 0A 0D 0A - 2 DOS line terminations) to see also the color group names of last language in the Function List view. And there should be no blank lines in entire wordfile, except between last line of a language definition and first line of next language definition. In some user contributed wordfiles there are also blank lines above color group definition lines for visual grouping. With using syntax highlighting for wordfiles those blank lines can be removed because you have now a much better visual help.


---- 


### Why are some color groups not listed in the function list view?

The UltraEdit regular expression search strings above as well as the default regular expression string do not find color groups without a name. Some old user contributed wordfiles available for download on server of IDM contain just `/C1`, `/C2`, ... without a name in double quotes.

Also some of the user contributed wordfiles have color group definitions with wrongly defined names, for example a space between number and first double quote, or a name without double quotes. Before using such wordfiles you should add names for the color groups or correct the color group definition lines with not correct syntax. The correct syntax is:

`/Cx"Color Group Name"`

Please send IDM your improved version of the wordfile by email with the request to replace the existing wordfile on IDM's server with the new one or submit a pull request via the official Github repo.

For details on how to create a new wordfile or check an existing wordfile for mistakes, read the following topics in the Syntax Highlighting forum.

###### Readme for the Syntax Highlighting forum
http://www.ultraedit.com/forums/viewtopic.php?f=9&t=5720

###### The ultimate syntax highlighting tools
http://www.ultraedit.com/forums/viewtopic.php?f=9&t=443

###### Template for Language File
http://www.ultraedit.com/forums/viewtopic.php?f=9&t=4124

These topics and the HTML files in syntax highlighting tools package available for download contain additional information. You can also upload a new or improved wordfile packed with ZIP as attachment to a topic in the Syntax Highlighting user-to-user forum if you need help on creating a wordfile or when you want that an expert validates it.

1. Except special German version 15.20.1.1000 SE which works only with one wordfile with multiple languages like UE v14.20.
2. UE3 is not developed further by IDM Computer Solutions, Inc.
   because SanDisk stopped U3 smart drives development.
