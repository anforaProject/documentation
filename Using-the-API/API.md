# API overview

## Methods

### Accounts

- Retrive basic information from an user

    GET /api/v1/accounts/:username

Returns the [Account](#Account) of the user with the given username.

### Statuses

- Create a new status (**Authentication required**)

    POST /api/v1/statuses

Upload a new status to the server. Form fields:

| Field         | Description                                      | Optional | Default |
| ------------- | -------------------------------------------------| -------- | ------- |
| `public`      | Set is a as a public posts                       | yes      | false   |
| `message`     | Test displayed next to the photo                 | yes      | empty   |
| `description` | A description for the media uploaded             | yes      | empty   |
| `sensitive`   | Set this to mark the media of the status as NSFW | yes      | false   |


## Entities

#Account

| Attribute                | Description                                                                        | Nullable |
| ------------------------ | ---------------------------------------------------------------------------------- | -------- |
| `id`                     | The ID of the account                                                              | no       |
| `username`               | The username of the account                                                        | no       |
| `acct`                   | Equals `username` for local users, includes `@domain` for remote ones              | no       |
| `display_name`           | The account's display name                                                         | no       |
| `locked`                 | Boolean for when the account cannot be followed without waiting for approval first | no       |
| `created_at`             | The time the account was created                                                   | no       |
| `followers_count`        | The number of followers for the account                                            | no       |
| `following_count`        | The number of accounts the given account is following                              | no       |
| `statuses_count`         | The number of statuses the account has made                                        | no       |
| `note`                   | Biography of user                                                                  | no       |
| `url`                    | URL of the user's profile page (can be remote)                                     | no       |
| `avatar`                 | URL to the avatar image                                                            | no       |
| `moved`                  | If the owner decided to switch accounts, new account is in this attribute          | yes      |
| `fields`                 | Array of profile metadata field, each element has 'name' and 'value'               | yes      |
| `bot`                    | Boolean to indicate that the account performs automated actions                    | no       |
