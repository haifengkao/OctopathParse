## Octopath Asset Parser

This program is able to parse pak, uexp and uasset files, and offers way to manage them.

It offers four commands that can read these files
 * `serialize <asset_path>` will turn a uexp/uasset pair into a .json file, reading the UObject properties. `<asset_path>` has no extension.
 * `filelist <pak_path>` will create a text file, listing all of the files contained in a .pak file.
 * `extract <pak_path> <pattern>` will extract all of the files where `<pattern>` is in the internal path name, into the corresponding directory in the current working folder.
 * `texture <asset_path>` will convert a DXT5 asset file into a png image file.

Any operations on a pak file require that the `key.txt` file contains the encryption key for the pak file, as a hexadecimal string and no leading newline.

Note however that there is limited support for all of the properties that can be serialized, and the parser may panic if it attempts to parse an unknown tag type.


### Info
Octopath 1 uses Unreal Engine 4.18

To unpack .Pak of a Octopath Traveler Mod, use [UnrealPakTool](https://github.com/allcoolthingsatoneplace/UnrealPakTool)
Put the .Pat at the same folder of UnrealPakExtract.bat, then double click the bat to unpack it.

To convert GameTextEn.uexp to json
type
'john-wick-parse serialize <some path>/GameTextEN'

GameTextEN.uexp and GameTextEN.uasset should be in the same folder

[Journey end](https://www.nexusmods.com/octopathtraveler/mods/17) modified GameTextEn.uexp, but the file is corrupted. It cannot be converted into json by Octopath Asset Parser.

A quick logging shows that there are 0xC01F at location 1404842 and 1405029 of GameTextEN.uexp, 0xC01F marks the start of a new record.
The record length at 1404850 is 179(0xB3). A quick glance of other records shows 1404842 + 179 + 17 should be equal to the location of next record 1405029. But it equal to 1405038 instead. To fix the file corruption, I changed the length at 1404850 from 179 to 170(0xAA).