# ✨🤖AudioBook Bay Discord Bot🤖✨

## Overview

This discord bot serves a very niche function: to interactively scrape audiobooks from AudioBook Bay from within discord. 

To utilize this bot you need:
1. To serve audiobooks via Plex
2. Use Qbitorrent to handle your downloads (I specifically use rdt-client with Debrid)
3. Use Discord

## 🏁 Install

1. Create a Discord Bot
   - Go to https://discord.com/developers/applications
   - Create a new application (you can name it whatever, but AudiobookRequester is nice.) Feel free to use this icon: ![](./docs/images/ABB-Discord.png)
   - Go to OAuth2 > URL Generator page, select bot under Scopes. Then, at minimum, select Embed Links, Use Slash Commands, and Send Messages under Bot Permissions.
   - Click Copy to get the URL and go to that URL.
   - Under Add to Server select your server.
   - Authorize the permissions.
   - Go back to the Discord Developer Portal and go to the Bot page and click Add Bot
   - Turn on Message Content Intent.
   - Save the changes.
   - Then copy the Token. You will need to provide this token in your .env file.

2. Create a directory of your choice (e.g. ./abb-discord-bot) to hold the docker-compose.yml and .env files:

```shell
mkdir ./abb-discord-bot
cd ./abb-discord-bot
```

  3. Download docker-compose.yml and example.env with the following commands:

```
wget https://github.com/jamcalli/ABB-Discord-Bot/releases/latest/download/docker-compose.yml
wget -O .env https://github.com//jamcalli/ABB-Discord-Bot/releases/latest/download/example.env
```
  4. Populate the .env file with your parameters:

Set the following environment variables:

- `DISCORD_TOKEN`: The token from above, when creating your discord bot.
- `DISCORD_CLIENT_ID`: Go to [Discord Developer Applications](https://discord.com/developers/applications) to find your bot application's ID.
- `DISCORD_GUILD_ID`: The ID of the guild where your bot will live.
- `QBITTORRENT_HOST`: qBittorrent host address (e.g., http://localhost:6500)
- `QBITTORRENT_USERNAME`: qBittorrent username
- `QBITTORRENT_PASSWORD`: qBittorrent password
- `PLEX_HOST`: Plex server address (e.g., http://localhost:32400)
- `PLEX_TOKEN`: This is your X-Plex-Token. Find out how to get yours [here](https://support.plex.tv/articles/204059436-finding-an-authentication-token-x-plex-token/).
- `PLEX_LIBRARY_NUM`: This is the library section number. It can be found by going to the following URL: `http://[PMS_IP_Address]:32400/library/sections?X-Plex-Token=YourTokenGoesHere`. Replace `YourTokenGoesHere` with your token from above. Note the `key="number"` of your audiobook library. That number goes here.

Your `.env` file should look something like this:

```env
DISCORD_TOKEN=YOUR_DISCORD_TOKEN
DISCORD_CLIENT_ID=YOUR_DISCORD_CLIENT_ID
DISCORD_GUILD_ID=YOUR_DISCORD_GUILD_ID
QBITTORRENT_HOST=YOUR_QBITTORRENT_HOST
QBITTORRENT_USERNAME=YOUR_QBITTORRENT_USERNAME
QBITTORRENT_PASSWORD=YOUR_QBITTORRENT_PASSWORD
PLEX_HOST=YOUR_PLEX_HOST
PLEX_TOKEN=YOUR_PLEX_TOKEN
PLEX_LIBRARY_NUM=YOUR_PLEX_LIBRARY_NUM
```

## 🔍 Search Commands

| Name      | Description            | Default                                               | Type   |
| --------- | ---------------------- | ----------------------------------------------------- | ------ |
| Query     | Search Query           |                                                       | String |
| Page      | Search Page            | 1                                                     | Number |
| Search In | Text content to search | `{ titleAuthor: true, content: true, torrent: true }` | Object |

<br>

```js
import { search } from "audiobookbay";

const audiobooks = await search("dune", 1, {
  titleAuthor: true,
});
```

### Response

```json
{
 "pagination": {
  "currentPage": "Current Page",
  "total": "Total Pages"
 },
 "data": [
   {
     "title": "Audiobook Title",
     "url":
       "Audiobook URL",
     "category":
       ["Array of Categories"],
     "lang": "Audiobook Language",
     "cover": "Audiobook Cover",
     "posted": "Date when Audiobook was posted",
     "info": {
         "format": "Audiobook Format",
         "bitrate": "Audiobook Bitrate",
         "size": ["Audiobook Size","Size UNIT"]
     }
   }, ...
 ]
}
```

### 🪣 Explore By Category/Tag

#### Category Options

<ul>
  <li>
    Age: children, teen-young-adult, adults, the-undead
  </li>

  <li>
    Category: postapocalyptic, action, adventure, art, autobiography-biographies, business, computer, contemporary, crime, detective, doctor-who-sci-fi, education, fantasy, general-fiction, historical-fiction, history, horror, lecture, lgbt, literature, litrpg, general-non-fiction, mystery, paranormal, plays-theater, poetry, political, radio-productions, romance, sci-fi, science, self-help, spiritual, sports, suspense, thriller, true-crime, tutorial, westerns
    </ul>
  </li>

  <li>
    Category Modifiers: anthology, bestsellers, classic, documentary, full-cast, libertarian, military, novel, short-story
  </li>
</ul>

#### Tag Options

<ul>
  <li>
  Popular Language: english, dutch, french, spanish, german
  </li>
</ul>

| Name   | Description              | Default  | Type   |
| ------ | ------------------------ | -------- | ------ |
| Type   | Explore by tag, category | category | String |
| Option | Options filter           |          | String |
| Page   | Page Number              | 1        | String |

<br>

```js
import { explore } from "audiobookbay";

const audiobooks = await explore("category", "postapocalyptic", 2);
```

### Response

```json
{
 "pagination": {
  "currentPage": "Current Page",
  "total": "Total Pages"
 },
 "data": [
   {
     "title": "Audiobook Title",
     "url":
       "Audiobook URL",
     "category":
       ["Array of Categories"],
     "lang": "Audiobook Language",
     "cover": "Audiobook Cover",
     "posted": "Date when Audiobook was posted",
     "info": {
         "format": "Audiobook Format",
         "bitrate": "Audiobook Bitrate",
         "size": ["Audiobook Size","Size UNIT"]
     }
   }, ...
 ]
}
```

### 🎵 Get Audiobook

| Name | Description  | Default | Type   |
| ---- | ------------ | ------- | ------ |
| ID   | Audiobook ID |         | String |

<br>

```js
import { audiobook } from "audiobookbay";

const audiobook = await audiobook(
  "the-road-to-dune-brian-herbert-kevin-j-anderson-frank-herbert"
);
```

### Response

```json
{
  "title": "Audiobook title",
  "category": ["Array of Categories"],
  "lang": "Audiobook Language",
  "cover": "Audiobook Cover",
  "author": "Audiobook Author",
  "read": "Audiobook Reader",
  "audioSample": "Sample of Audiobook MP3",
  "specs": {
    "format": "Audiobook Format",
    "bitrate": "Audiobook Bitrate"
  },
  "abridged": "Is the book shortened",
  "desc": "Audiobook Description",
  "torrent": {
    "hash": "Audiobook Hash",
    "trackers": ["Audiobook Trackers"],
    "size": ["Audiobook size", "Size UNIT"],
    "magnetUrl": "Magnet Link" // 🧲 Magnet Link
  },
  "related": [
    {
      "title": "Related Audiobook Title",
      "url": "Related Audiobook URL"
    }
  ]
}
```

## ⚡ Example Usage

Included is an example using the library.

[usage-example.ts](./playground/usage-example.ts)

To run it from this repo.

```bash
pnpm example
```

![](./docs/images/example-screenshot.png)
