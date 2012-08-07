#API Specification - LPMC Casino
##Overview
This API is a language agnostic syntax used to communicate between the client and server layers, allowing independence
of implementation between client and server. 

##General Guidelines
- This is a raw socket connection, which uses a stream of string information to communicate between client and server.
- Overall syntax considerations:
    - A distinct information block (eg: A login request or a bet from one of the games) will be bound by two pipes (||).
    - Immediately following the pipes will be the name of the block of information
    - The data of a information block will be delimited from the name by two colons (::)
    - Varying pieces of data within that command block will delimited by a pipe (|)
    - Each piece of data will be set off by a standardized identifier, and identified by a standardized naming 
    scheme followed by a colon (:)
    - The data will continue in this fashion until the next double pipe (||) is met, marking the end of that
    information block
    - All of these identifying marks, should they need to be included in a datastream, will be escaped using a 
    blackslash (\\).
    - These blocks are case insentitive, but good practices calls for all lower case

##Full Specification
This API is primarily a way for clients to issue commands and receive information from the server. The syntax
of return statements and requests are the same, with different components being sent and received.

###Handshake protocol
In order to send and receive messages properly, two identifying pieces of information must be passed with every
API call, depending on which call it is. 

All calls must have:
messID - This is a client issued message identifier used to find returned messages on the stack, that will be sent
with the server's response to a client message.

When issuing commands before a user has logged in, a call must have:
clientID - This is a client issued name that the server can use to identify an incoming client. The server will
reject a clientID if it is identical to one currently in use, and will close the socket connection.

When issuing commands after a user has logged in, a call must have:
sessID - This is a server issued identifier for a user, used to validate that they have logged in properly. 



###General Syntax Template
    ||<NameOfInfo>::<Component1>:<Info>|<Component2>:<Info>|...|<ComponentN>:<Info>||
    
####Examples
    

###Administrative Blocks
####Component Descriptions
    - clientID: A single client's unique ID
    - sessID: A user's unique session ID 
    - messID: A message's unique ID
    - user: A user's full username
    - pass: A user's full password
    - email: A user's full email
    - currency: A user's currency
    - money: A user's money (READ-ONLY)
    - userinfo: A complete list of the above (READ-ONLY)
    - new: When modifying a component, this is the new version
    - old: When modifying a component, this is the old version
    - target: When a command can be specified to multiple locations, this specifies the name of the location. 
    This target will *always* be another component name, however that component must not be read only.
    - bool: A boolean value

####Getters
Email - Returns a user's email

    Name: email
    Components: sessID, messID, user
    Returns: sessID, messID, email

Status: NOT STARTED

Currency - Returns a user's currency

    Name: currency
    Components: sessID, messID, user
    Returns: sessID, messID, currency

Status: NOT STARTED

Money - Returns the amount of money the user has

    Name: money
    Components: sessID, messID, user
    Returns: sessID, messID, money

Status: NOT STARTED

User Information - Returns a block of user information

    Name: userinfo
    Components: sessID, messID, user
    Returns: sessID, messID, email, currency, money

Status: NOT STARTED

####Setters
Change - Modifies a user's account information.

    Name: change
    Components: sessID, messID, target, old, new
    Returns: sessID, messID, bool

Status: NOT STARTED

####Functions
Login - Validates a user logging in.

    Name: login
    Components: clientID, messID, user, pass
    Returns: messID, sessID

Status: NOT STARTED

Logout - Ends a users session.

    Name: logout
    Compnents: messID, sessID
    Returns: messID, bool

Status: NOT STARTED

Create New Account - Creates a new user account.

    Name: createacc
    Components: clientID, messID, user, pass, email, currency
    Returns: messID, sessID

Status: NOT STARTED

Recover Account Information - Sends a user their username and reset their password

    Name: recover
    Components: clientID, messID, email
    Returns: messID, bool

Status: NOT STARTED

###Game Blocks - These will be added as development continues