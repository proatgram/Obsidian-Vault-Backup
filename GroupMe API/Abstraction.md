Mock

Main Class
	Functions
 - getGroups() returns: vector<Group>
 - getChats() returns: vector<Chat>
 - createChat(User) returns: Chat
 - createGroup(string) returns: Group
 - changeName(string) returns: bool
 - changeAvatar(string) returns: bool

AuthenticateUser Class
	Functions
 - setCallbackListener(string) returns: string
 - setAppURL(string) returns: string

Group Class
	Functions
 - getName() returns: string
 - getNumberUsers() returns: int
 - getLastMessage() returns: uint
 - getMember(uint userID) returns: User Class
 - getMembers() returns: Users Class
 - getAccessType() returns: uint8
 - getDescription() returns: string
 - getImageURL() returns: string
 - getUpdatedTime() returns: uint
 - getCreatedTime() returns: uint
 - getShareURL() returns: string
 - getLatestMessages(uint) returns: Messages
 - getMessagesAfterMessage(Message, uint) returns: Messages
 - getMessagesBeforeMessage(Message, uint) returns: Messages
 - addMember(User) returns: bool
 - removeMember(User) returns: bool
 - update() returns: bool
 - updateLatestMessage() returns: bool
 - updateName() returns: bool
 - updateImageURL() returns: bool
 - updateDescription() returns: bool
 - updateMembers() returns: bool
 - updateMessages() returns: bool
 - static create(string, string, filesystem::file) returns: Group

Chat Class
	Functions
 - getName() returns: string
 - getOtherUserID() returns: string
 - getMessagesBeforeMessage(Message) returns: Messages
 - getMessagesAfterMessage(Message) returns: Messages
 - getLatestMessages(uint) returns: Messages
 - sendMessage(string, bool = false, Attatchments = null) returns: bool
 - update() returns: bool
 - updateMessages() returns: bool

User Class
	Functions
 - getID() returns: string
 - getNickname() returns: string
 - getImageURL() returns: string
 - getPhoneNumber() returns: string
 - getEmail() returns: string
 - getGUID() returns: string
 - static create() returns: User

Users Class
	Functions
 - addUser(User) returns: bool
 - removeUser(User) returns: bool 
 - renameUser(User) returns: string
 - lookupUser(string) returns: User
 - static create()

Message Class
	Functions
 - getPosted() returns: uint
 - getPoster() returns: User
 - hasAttatchments() returns: bool
 - getAttatchments() returns vector<Attatchment>
 - getMessageID() returns: string
 - static create(json maybe) returns: Message

Messages Class
 	Functions
 - push_back(Message) returns: bool
 - push_forward(Message) returns: bool
 - setOverflowLimit(int) returns: int
 - getOverflow() returns: int
 - at(int) returns: Message
 - latestIn() returns: Message
 - oldestIn() returns: Message

Attatchment Class
	Functions
 - getType() returns: uint
 - getContent() returns: string
 - static create(filesystem::path) returns: Attatchment
 - static create(string) returns: Attatchment
