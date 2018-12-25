# JS Client API

## Creating content 

#### uploadStatus(data: Object, token: String)

Create a new status using the data provided. The provided data can include

- `public` (boolean) : Whether the new status should be public or
not.
- `status` (string): The caption for the status
- `sensitive` (boolean): Whether the status contains sensitive
images. Basically a NSFW alert.
- `media_ids` (string): A list of media files represented by their id to add to the status.

`token` is the string containing the user token.

Returns an axios promise

#### uploadComment(data: Object, token: String)

Create a new comment. The data provided may include:

- `public` (boolean): Whether the comment is public or not.
- `status` (string): Actual message for the comment.
- `sensitive` (boolean): Whether the message includes some dangerous content.
- `spoiler_text` (string): A spoiler text to use when sensitive is true.
- `in_reply_to_id` (string): Mandatory. The id of the status we are commenting.

`token` is the string containing the user token.

#### uploadMedia(data: Object, token: String)

Create a media object on the server. Data includes:

- `file` (blob): Mandatory. The image file.
- `description` (string): A description for the media.

`token` is the string containing the user token.

## Retrive Data

#### retriveImages(id: String)

Retrive all the statuses for a given user. `id` is the identifier for the user.

#### retriveStatus(id: String)

Retrive the status with the given id.

#### retriveUser(id: String)

Retrive the profile info for the user with the given `id`.

#### getFollowers(id: String)

Retrive a list of user's profiles follwing the user with the given `id`.

#### getFollowing(id: String)

Retrive a list of user's profiles that the user with the given `id` is following.

## Actions

#### removeStatus(id: String, token: String)

Remove the status with the `id` given. `token` must be the owner's token.

#### likeStatus(id: String, token: String)

Like the status with the given `id` with the `token` credentials of the current user.

#### dislikeStatus(id: String, token: String)

Undo a like action over the status with the given `id` 

#### verifyCredentials(token: String)

Verify that the current user matches the correct profile. 
This is used to to check if the token is still valid.

## Accounts 

#### passwordReset(data: Object)

Change password for the current user. Data is an object with the following attributes:

- `password` (string): The new password
- `password_confirmation` (string): A confirmation for the password
- `token` (string): The token to auth the user
