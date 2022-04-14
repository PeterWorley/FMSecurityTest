# SecurityTest
### A simple application for testing Account-based record security in FileMaker

This is a proof-of-concept application to test limiting "client" record visibility based on the logged-in Account.

It uses two additional tables: "user" and "userClient", a "UserClientIDs" value list, and a "RestrictedAccess" privilege set with custom record access. 

##Tables

###"user" table
The "user" table contains a record for each restricted user, storing the name of the FileMaker Account.

- pkUserID: unique ID
- UserAccount: stores the name of the corresponding FileMaker Account

###"userClient" table
The "userClient" table is a join table for creating a many-to-many relationship between "user" and "client" tables, for filtering user-client access.

- pkUserClientID: unique ID
- fkUserID: corresponds to "pkUserID" in the "user" table
- fkClientID: corresponds to "pkClientID in the "client" table

###"client" table
The "client" table contains client information.

- pkClientID: unique ID
- ClientName: the client name (representing real information about the client that users need to access)

##Value List

###"UserClientIDs" value list
From "userClient::fkClientID"

##Privilege Set

###"RestrictedAccess" privilege set
Has the following record privileges:

- user: limited view access: UserAccount=Get(AccountName)
- userClient: limited view access: user::UserAccount=Get(AccountName)
- client: limited view access: Position (ValueListItems (Get (FileName); "UserClientIDs"); pkClientID; 1; 1)
(Note: the UserClientIDs value list gets filtered via the userClient limited view privilege)
