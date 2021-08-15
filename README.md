```ccounts
This section contains all the endpoints that can be used to retrieve room information. Player/Account information isn't a part of api.rec.net, but is instead accessed through accounts.rec.net. More information on accounts and account information is given below.

Account Properties
Property	Type	Description
Account ID	Int	Each account has a unique ID associated with it. Account IDs are sequential which means that every time a new account is created, it's ID will be +1 of the previously created account.
Username	String	This is the unique name of the player. This is the name that is used in the URL of the account's RecNet page ( i.e https://rec.net/user/:username ).
Display Name	String	This is what appears in bold above the username on an account's page on RecNet. The display name is not unique unlike the username.
Profile Image	String	This is the file name of an accounts profile picture, and it can be used to get a link to the image itself from this endpoint, https://img.rec.net/:imageName.
Banner Image	String	This is the file name of an account's banner image, and it can be used to get a link to the image itself from this endpoint, https://img.rec.net/:imageName.
Junior Status	Boolean	This is true if the account is a junior account, false if the account is a non-junior account
Platforms	Int	This is a number that is representative of the platforms that an account is associated with.
Created At	String	This is the time of the account creation.
GETPlayer Information From ID
https://accounts.rec.net/account/:playerId
This takes an account ID as a path parameter, and returns an object that contains detailed information about a player.

PATH VARIABLES
playerId


Example Request
/account/1827567
curl --location --request GET 'https://accounts.rec.net/account/1827567'
Example Response
200 OK
BodyHeader
(9)
View More
{
  "accountId": 1827567,
  "username": "ColinXYZ",
  "displayName": "ColinXYZ",
  "profileImage": "d0df003353914adfaecdd23f428208b6",
  "bannerImage": "0f0a46aece6b4a40baf13c4f80a9216c.jpg",
  "isJunior": false,
  "platforms": 6,
  "createdAt": "2019-06-12T02:32:54.467Z"
}
GETPlayer Information From Username
https://accounts.rec.net/account?username={Player_Username}
This takes a username as a query parameter, and returns an object that contains detailed information about a player.

PARAMS
username
{Player_Username}


Example Request
/account?username=colinxyz
curl --location --request GET 'https://accounts.rec.net/account?username=colinxyz'
Example Response
200 OK
BodyHeader
(9)
View More
{
  "accountId": 1827567,
  "username": "ColinXYZ",
  "displayName": "ColinXYZ",
  "profileImage": "d0df003353914adfaecdd23f428208b6",
  "bannerImage": "0f0a46aece6b4a40baf13c4f80a9216c.jpg",
  "isJunior": false,
  "platforms": 6,
  "createdAt": "2019-06-12T02:32:54.467Z"
}
GETPlayer Bio
https://accounts.rec.net/account/:playerId/bio
This takes an account ID as a path parameter, and returns an object that has AccountID and Bio properties. The Bio property is the bio that is associated with the account ID.

PATH VARIABLES
playerId


Example Request
/account/489626/bio
curl --location --request GET 'https://accounts.rec.net/account/489626/bio'
Example Response
200 OK
BodyHeader
(9)
{
  "accountId": 489626,
  "bio": "Founder and CEO of Aperture Science."
}
GETPlayer Search
https://accounts.rec.net/account/search?name={Search_Query}
This takes a search query as a query parameter, and returns an array of all the account objects that are most relevant to the search query.

PARAMS
name
{Search_Query}


Example Request
/account/search?name=Colin
curl --location --request GET 'https://accounts.rec.net/account/search?name=Colin'
Example Response
200 OK
BodyHeader
(9)
View More
[
  {
    "accountId": 42413,
    "username": "colin",
    "displayName": "HiddenPotato",
    "profileImage": "5RD-rqz3y0u_7j0o5kWcYg",
    "isJunior": false,
    "platforms": 1,
    "createdAt": "2016-12-02T00:00:08.653Z"
  },
  {
POSTBulk Player Information
https://accounts.rec.net/account/bulk
This returns an array of account objects whos IDs match the IDs that were passed in to the form body. The array is in reverse order of the way the IDs appear in the body.

BODYurlencoded
id[]
1827567
id[]
271562
id[]
53208


Example Request
/account/bulk
curl --location --request POST 'https://accounts.rec.net/account/bulk' \
--data-urlencode 'id[]=1827567' \
--data-urlencode 'id[]=271562' \
--data-urlencode 'id[]=53208'
Example Response
200 OK
BodyHeader
(9)
View More
[
  {
    "accountId": 53208,
    "username": "s_trunks",
    "displayName": "Trunks",
    "profileImage": "4kqnu30V2U-BDG9EZ-toDQ",
    "isJunior": false,
    "platforms": 3,
    "createdAt": "2016-12-13T20:35:59.893Z"
  },
  {
Playlists
Playlist Properties
View More
Property	Type	Description
Playlist ID	Int	Each playlist has a unique ID associated with it. Playlist IDs are sequential which means that every time a new playlist is created, it's ID will be +1 of the previous playlist created. Playlist IDs start at 17859340 with the "FeaturedRooms" playlist, and the playlist's id corresponds to the order they are in game.
Playlist Name	String	This is the unique name of the playlist.
Playlist Description	String	This is the description of the playlist.
Image Name	String	This is the file name of the room thumbnail, and it can be used to get a link to the image itself from this endpoint, https://img.rec.net/:imageName.
Custom Warning	String	This is the warning on the playlist. If no warning was set, then this property will be null.
Creator Account ID	Int	This is the internal account ID of the player who made the playlist. For more information regarding account information look at the Accounts section.
Accessibility	Int	This property defines who is able to access the playlist. If this is equal to 1, then it's accessibility is set to public, and everyone can access it. If this is equal to 0, then it's accessibility is set to private, and only the room owner and co-owners will be able to access it.
Created At	String	This is the time the playlist was created.
Stats	Object	This is the playlist statistics. It has four properties which gives information on how people have interacted in that room. VisitCount and VisitorCount are different because VisitCount is the total number of times a room has been visited whereas VisitorCount is the number of unique players who visited a playlist.
Include Parameter
The include parameter takes an integer as an input, and returns different information based on it's value. Each value and it's corresponding information is listed below.

Value	Description
1	This returns an object array of room information for rooms that are in the playlist. For more information on rooms please refer to the Rooms section.
2	This returns an object array of room information for rooms that are in the playlist along with subroom information for each room. For more information on rooms please refer to the Rooms section.
4	This returns an object array of information on the playlist's tags. Each tag has a type, and any thing with a type of 0 are tags that were set by a player. Currently none of the playlist have tags on them.
64	This returns the score of a playlist. Currently all playlist except the featured playlist have a score of 0.
These values can be added to get more information at once.

GETGet Playlist Information From ID
https://rooms.rec.net/playlists/17859340


Example Request
Get Playlist Information From ID
curl --location --request GET 'https://rooms.rec.net/playlists/17859340'```
