## Table of Contents
```toc
```

## Overal Format
There should be a main file that redirects to other files for other save data.

The way the file structure is organized is to achive the most orginization possible, and only have files be big if they need to be. So, there should be a bunch of small files pointing to a bunch of bigger files that contain the actual data.
Ex.

Main file
	2nd set:
		Entity Folder location -> Location to the folder containing the files shown in this set.
		PlayersFile -> Location to the players data file.
		ImportantEntitiesFile -> Location to the important entities file.

Each file should contain in the begining, along with all of the other data, a filetype description

## Saves File
This file just stores the location and name of each level that the game displayes in the load screen.
### First Set of Entries
This set contains each level, its name, and its location.
#### Level x...
1. Level Name
2. Level Folder Location

## Main File
Each entry dealing with file locations should, at the beggining of the entry set, should have the location to the folder that holds the data being named.
Ex.

Location={LevelName}/Entity/     # /users/user/.local/SudoCraft/saves/{LevelName}/Entity Where level name is the level name that is saved.
PlayerLocation=player.dat      # ${Location}/Entity/player.dat

### First Set of Entries
The begining file structure will have the basic level data stored.
1. Level Name
	- String with null termination.
2. Level Size
	- Unsigned int 64
3. Last Date Played
	- Readable format able to be changed.
	- Must include month, day, year, hour, and minute.

### Second Set of Entries
The second set of the entries should hold the entities file location.
1. Entity folder location
2. Players file
3. Important entities file

### Third Set of Entries
The third set of all of the entries should hold the level data location.
1. Level data folder location
2. Level data file



## Players List File
This file contains the locations to the each player data file for every player that has played the level. The first set always has one entry and it includes the folder path to the players folder. All of the sets after that have the player name and the player file name.

### Set 1
1. Players file folder location
### Set 1+x...
1. Player name
2. Player file name

## Player File
This file contains the player data for a player

### First Set of Entries
The first set of entries just contains some basic data for the player.
1. Player name
2. Player ID
3. Player health
4. Player hunger
5. Player thirst
6. Player level
7. Player experience

### Second Set of Entries
The second set of entries contains the players inventory
1. Player inventory

### Third Set of Entries
1. Player armor set

## Level data file
This file contains all of the level data, like generation data, and the tile data. The game will not store all of the generated terrain, but only the terrain that differs from the original generation. For example, if a player destroys a tile, the destoryed tile will be saved as air in the save.

The save file will have a slightly different way of storing the data, and instead of sets, it uses the actual corrdinates of the chunks as "A set". So the first set might look like 37 81 1 in decimal. Each "Coordinate Set" will hold a string with the filename for the chunk data file.

### First Set of Entries
The first set of entries contains some basic stuff about the level.
1. Seed for height (int64)
2. Seed for blocks (int64)
3. Seed for etc (int64)
4. Level Size (uint64)
5. Chunks folder path (string)

### X Set of Entries
1. Chunk Filename (string)

## Chunk File
This file contains all of the data for the changed blocks in one chunk. Each "Level" will take up two Y coordinate, the first one being for changed blocks, and the second one being for changed objects. For example on the first Y level for ground level would be some grass that has been mined, while on the next one there would be trees and bushes that have been mined. Each chunk is 32x32, with two Y coordinates for each level.

Like the Level Data file, the "Set" system is replaced by coordinate numbers. Each coordinate number is each coordinate in the chunk. 0x, 0y, 0z to 32x, 32y, 1z.

If the player is in a cave, then each level the character goes down, will decrease the Y level by 2.

### First Set of Entries
The first set of entries contains some basic chunk information.
1. Chunk coordinate (3 int64)

### X Set of Entries
1. Block ID (uint16)

## Data Explanation
This section explains the data values or each file and what they mean. We will be refering to data in hexadecimal. Each data structure, (eg. string) will have a type descriptor, a byte, and then after that, a size value, which is four bytes. And then the data. The amount of data contained in the data section must have the same size as the size descriptor.
Ex.
`00050000000EE39F4AA7`
A type descriptor of 0, a size of 5 bytes, and then the data, consisting of five bytes.
### File Types Definitions
- `0x00` : This value referes to the saves file that the game refers to in order to get the name and folder of the level for the level load screen.
- `0x01` : This value referes to the players file that the game refers to in order to retrieve all of the player data files.
- `0x02` : This value refers to a player data file. This is the file that stores all of the player data for a specific player.
- `0x03` : This value refers to an important entities file. This file contains all of the entity data for important, non auto-despawnable entities.
- `0x04` : This value refers to the world data file. This file contains the tile data and world data for a specific level.
- `0x05` : This value refers to the mains file. This is the starting file.
- `0x06` : This value refers to the Level Save file.
- `0x07` This value refers to the Chunk file.
### Data Type Definitions
These may not be used.
* `0x00` : int8
* `0x01` : int16
* `0x02` : int32
* `0x03` : int64
* `0x04` : uint8
* `0x05` : uint16
* `0x06` : uint32
* `0x07` : uint64
* `0x08` : char
* `0x09` : char* (string)
* `0x0A` : float
* `0x0B` : double

### Global Tag Definitions
- `0x01` : The start of the header section
- `0x02` : Next Set
- `0x03` Next Sub-Set

### File Format Explanations
#### Saves File
The saves file is a basic file and will be relatively small, and it contains information for the game to read to put on the save data screen, and a path to the level folder on the filesystem.

##### File Contents
The file contents are laid out below.
1. **HEADER**
	1. File type indicator (uint8)
	2. Save data version (double)
2. **SET X...**
	1. Name (string)
	2. Save Location (string)
##### Example
`0108003FF000000000000002364E657720576F726C642F75736572732F757365722F2E6C6F63616C2F5375646F43726166742F53617665732F4E65775F576F726C642F`
- `01` Header start
- `08` Header Size
- Header Data for 8 bytes...
- `02` Next Set
- `36` Set Size (54 Bytes)
- Set Data for 54 bytes...

#### Mains File
The main file is the file that will first be read to get the world data. This file contains basic level data, like the names, size, last play time, etc. It also points to different important files that include the entities folder, the players file, the important entities file, the level data folder location, and the level data file.
##### File Contents
The file contents are laid out below.
1. **HEADER**
	1. File type indicator (uint8)
	2. Save data version (double)
2. **SET 1**
	1. Level Name (string)
	2. Level Size (uint64)
	3. Last Date Played
		1. Time in minutes (uint16)
		2. Number of days since January 1st 1900 (uint32)
3. **SET 2**
	1. Entity folder location (string)
	2. Players file (string)
	3. Important entities file (string)
4. **SET 3**
	1. Level data folder location (string)
	2. Level data file (string)

##### Example

`0108053FF000000000000002174E6577204C6576656C00000000000000000003840000AEC00229456E746974792F00506C61796572732E64617400496D706F7274616E74456E7469746965732E6461740002104C6576656C2F004C6576656C2E64617400`
- `01` Header start
- `08` Header Size
- Header Data for 8 bytes...
- `02` Next Set
- `17` Set size (23 bytes)
- Set Data for 23 bytes...
- `02` Next Set
- `29` Set size (41 bytes)
- Set Data for 41 bytes...
- `02` Next set
- `10` Set size (16 bytes)
- Set Data for 16 bytes..

#### Players File
The Players file stores the location of each of the player files for each player that has ever played in the level. 
##### File Contents
The file contents are laid out below.
1. **HEADER**
	1. File type indicator (uint8)
	2. Save data version (double)
2. **SET 1**
	1. Player folder name
3. **SET 1+X...**
	1. Player name
	2. Player file name

##### Example

`0108013FF00000000000000208506C61796572732F000211417A6465616C746800303030302E64617400020E4A756D6D6900303030312E64617400`
- `01` Header start
- `08` Header size
- Header data for 8 bytes...
- `02` Next set
- `08` Set size (8 bytes)
- Set Data for 8 bytes...
- `02` Next set
- `11` Set size (17 bytes)
- Set data for 18 bytes...
- `02` Next set
- `0E` Set size (14 bytes)
- Set data for 14 bytes...

#### Level Data File
The Level Data File contains the the filenames for each chunk file that exists.
##### File Contents
1. **HEADER**
	1. File type indicator (uint8)
	2. Save data version (double)
2. **SET 1**
	1. Seed for height (int64)
	2. Seed for blocks (int64)
	3. Seed for etc (int64)
	4. Level Size (uint64)
	5. Chunks Folder (string)
3. **X, Y, Z SET** (int64)
	1. Chunk File Name (string)

##### Example

`0108053FF000000000000002271ED4C80D00E60EEFECF5CCFFEEAAEECC1ED4C80D00E60EEF00000000000000014368756E6B732F000000000000000001000000000000000100000000000000000D414A534E32316177322E64617400`
- `01` Header start
- `08` Header size
- Header data for 8 bytes...
- `02` Next Set
- `27` Set size (39 bytes)
- Set data for 39 bytes...
- `000000000000000100000000000000010000000000000000` Set 1x, 1y, 1z
- `0D` Set size (13 bytes)
- Set data for 13 bytes...

#### Chunk File
This is the file used to save modified block and objects.
##### File Contents
1. **HEADER**
	1. File type indicator (uint8)
	2. Save data version (double)
2. **SET 1**
	1. Chunk Coordinates (3x int64)
3. **SET X, Y, Z** (uint8)
	1. Block ID (uint16)

##### Example

`0108073FF0000000000000021700000000000000010000000000000001000000000000000000000002000E00000102000E`
- `01` Header start
- `08` Header size
- Header data for 8 bytes...
- `02` Next set
- `17` Set size (23 bytes)
- Set data for 23 bytes
- `000000` Set 00x, 00y, 00z
- `02` Set size
- Set data for 2 bytes...
- `000001` Set 00x, 00y, 01z
- `02` Set size
- Set data for 2 bytes...

#### Player File
This file is dedicated to storing player data.
##### File Contents
1. **HEADER**
	1. File type indicator (uint8)
	2. Save data version (double)
2. **SET 1**
	1. Player Name (string)
	2. Player ID (uint32)
	3. Player Health (uint8)
	4. Player Hunger (uint8)
	5. Player Thirst (uint8)
	6. Player Level (uint32)
	7. Player Experience (uint64)
3. **SET 2**
	1. Subset X...
		1. Item ID (uint16)
		2. Inventory Slot Number (uint8)
		3. Item Durability (uint8)
		4. Item Modifications (string)
4. **SET 3**
	1. Subset 1 (Helmet)
		1. Item ID (uint16)
		2. Item Durability (uint8)
		3. Item Modifications (string)
	2. Subset 2 (Tunic)
		1. Item ID (uint16)
		2. Item Durability (uint8)
		3. Item Modifications (string)
	3. Subset 3 (Leggings)
		1. Item ID (uint16)
		2. Item Durability (uint8)
		3. Item Modifications (string)
	4. Subset 4 (Boots)
		1. Item ID (uint16)
		2. Item Durability (uint8)
		3. Item Modifications (string)

##### Example
`0108023FF000000000000002_417A6465616C7468000EE4F607`