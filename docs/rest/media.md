# Media

## POST /api/v1/media

Upload a media attachment to be used in a new status.

Returns an [Attachment](/entities/#Atachment)

### Resource information

|        Property         |         Value         |
| ----------------------- | --------------------- |
| Response format         | JSON                  |
| Requires authentication | Yes                   |
| Requires user           | Yes                   |
| Scope                   | `write` `write:media` |
| Available since         | 0.0.0                 |

### Parameters

|     Name      |                       Description                       | Required | Default |
| ------------- | ------------------------------------------------------- | -------- | ------- |
| `file`        | Media file encoded using `multipart/form-data`          | True     | -       |
| `description` | A plain-text description of the media for accessibility | False    | empty string      |
| `focus`       | Two floating points, comma-delimited.                   | False    | '0,0'   |

