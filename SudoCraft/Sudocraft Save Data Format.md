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
4. Player level
5. Player experience

### Second Set of Entries
The second set of entries contains some more advanced stuff
1. Player inventory
2. Player armor set

## Level data file
This file contains all of the level data, like generation data, and the tile data.

### First Set of Entries
The first set of entries contains some basic stuff about the level.
1. Seed for height
2. Seed for blocks
3. Seed for etc
4. Level Size

### Second Set of Entries
The second set of entries contains the actual terrain data.
Each subset will be an individual bock.

#### subset x...
1. Coordinate Data
2. Block Data
	1. Block ID
	2. Extra Block Data

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
- `0x04` : This value refers to the world data file. This file contains the tile data and world data for a specific level
- `0x05` : This value refers to the mains file. This is the starting file.
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