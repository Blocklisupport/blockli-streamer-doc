# Blockli Streamer

## How to use
- Install the plugin via the WP plugin installer.
- Activate the plugin.
- Navigate the plugin to see options

## Features

### Rest API Fields
How to access the REST custom fields, visit one of the rest endpoints like `domain_name/wp-json/wp/v2/featured_cards` look for the `blockli_fields`.

If the field is not set on the particular post, the result will come back as `null`

#### Creators Fields
Access: `{domain}/wp-json/wp/v2/blockli_creators`

|Field  | Api Value   | More Details for ID  |
|---|---|---|
| title  | `title -> rendered`  |   |
| bio  | `content -> rendered`  |   |
| image  |   `featured_image_url` |   |

![Creators Fields](./docs/creators-fields.png)

#### Collections Fields
Access: `{domain}/wp-json/wp/v2/blockli_collections`


|Field  | Api Value   | More Details for ID  |
|---|---|---|
| title  | `title -> rendered`  |   |
| description  | `content -> rendered`  |   |
| image  |   `featured_image_url` |   |
| attached videos  | `blockli_fields -> attached_videos_list_ids` | `{domain}/wp-json/wp/v2/blockli_videos/{videos_id e.g. 83}`  |
| attached creators  | `blockli_fields -> attached_creators_list_ids` | `{domain}/wp-json/wp/v2/blockli_creators/{creator_id e.g. 83}`   |
| Trailer source  | `blockli_fields -> videos_trailer_options` |   |
| Bunny CDN URL  | `blockli_fields -> bunny_cdn_upload_link` |   |
| Vimeo URL  | `blockli_fields -> youtube_upload_link` |   |
| Youtube URL  | `blockli_fields -> vimeo_upload_link` |   |
| Tags  |   `tags` | `{domain}/wp-json/wp/v2/blockli_videos_tag/{tag_id e.g. 83}`  |
| Categories  |   `categories` |  |
| Categories Details |   `categories_details` | |
   
![Collections Fields](./docs/collections-fields.png)
![Collections Tags or Categories](./docs/collections-tags.png)
![Collections Tags or Categories](./docs/categories-details.png)

#### Videos Fields
Access: `{domain}/wp-json/wp/v2/blockli_videos`

|Field  | Api Value   | More Details for ID  |
|---|---|---|
| title  | `title -> rendered`  |   |
| description  | `content -> rendered`  |   |
| image  |   `featured_image_url` |   |
| attached collections  | `blockli_fields -> attached_collections_list_ids` | `{domain}/wp-json/wp/v2/blockli_collections/{collections_id e.g. 83}`  |
| attached creators  | `blockli_fields -> attached_creators_list_ids` | `{domain}/wp-json/wp/v2/blockli_creators/{creator_id e.g. 83}`   |
| Trailer source  | `blockli_fields -> videos_trailer_options` |   |
| Bunny CDN URL  | `blockli_fields -> bunny_cdn_upload_link` |   |
| Vimeo URL  | `blockli_fields -> youtube_upload_link` |   |
| Youtube URL  | `blockli_fields -> vimeo_upload_link` |   |
| Tags  |   `tags` | `{domain}/wp-json/wp/v2/blockli_videos_tag/{tag_id e.g. 83}`  |

![Videos Fields](./docs/videos-fields.png)

## Functions

## Action and Filter Hooks

### Action Hooks

## Rest API
### /blockli/v1/streamer/watched
#### Description
The /blockli/v1/streamer/watched endpoint is a custom REST API route in WordPress that allows clients to submit data related to the percentage of a video watched and whether it was completed. This data can be used to track user engagement with video content on the website. The data is sent to the server using a POST request in JSON format.

#### Endpoint Details
- URL: https://example.com/wp-json/blockli/v1/streamer/watched
- Method: POST
- Permission Callback: The endpoint has unrestricted access. Any authenticated or unauthenticated user can make requests to this endpoint.

#### Request Parameters
The endpoint expects the following JSON parameters to be included in the request:

- video_id (required): The id of the video being played.
- user_id (required): The id of the user watching the video.
- collection_id (optional): The current collection id to which the video belongs.
- percentage (required): The percentage of the video that the user has watched (float value from 0 to 100).
- completed (required): A boolean value indicating whether the user has completed watching the video (true if completed, false otherwise).

#### Response
Success Response
- Status: 200 OK
- Content: JSON object with a success property set to true.
Error Response
- Status: 404 Not Found
- Content: JSON object with a failed property set to true.

#### Action Hooks
Blockli Streamer has two action hooks that other plugins or themes to extend and perform additional processing or actions based on the submitted video or collection data

`do_action( 'blockli_streamer_video_progression', $video_id, $user_id, $percentage, $completed );`
The function triggers a custom action named blockli_streamer_watched_submission with the $video_id, $user_id, $percentage and $completed values as parameters. .

`do_action( 'blockli_streamer_collection_progression', $collection_id, $user_id, $percentage, $completed );`
The function triggers a custom action named blockli_streamer_watched_submission with the $collection_id, $user_id, $percentage and $completed values as parameters.

Usage
```
add_action( 'blockli_streamer_video_progression', 'custom_plugin_additional_video_processing', 10, 4 );

function custom_plugin_additional_video_processing( $video_id, $user_id, $percentage, $completed ) {
    ...Add custom implementation.
}
```
```
add_action( 'blockli_streamer_collection_progression', 'custom_plugin_additional_collection_processing', 10, 4 );

function custom_plugin_additional_processing( $id, $percentage, $completed ) {
    ...Add custom implementation.
}
```
