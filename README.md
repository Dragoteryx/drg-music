# drg-music
A single file module to easily manage Discord music bots using Discord.js

## How to use ?
First, you need to require the module.
```js
const drgMusic = require("drg-music");
```
Then, you must create a new MusicHandler.
```js
const music = new drgMusic.MusicHandler(client);
```
``client`` represents your Discord.js client (your bot).

Then, you'll need to interact with the MusicHandler you just created.

## What can it do ?
With this module, you can ask the bot to join and leave a voice channel, to request and play a Youtube video, to pause/resume, to set the volume, etc.
Most commands will require you to specify the guild where you want to execute the action.

### Join a voice channel
```js
music.join(member);
```
``member`` is the guild member that the bot will join.
Emits an event ``joined``, with the guild that the bot joined.

### Leave a voice channel
```js
music.leave(guild);
```
Emits an event ``leaved``, with the guild that the bot leaved.

### Add a Youtube video to the playlist
```js
music.addVideo(member, youtubeLink);
```
``member`` represents the guild member that requested the music.
``youtubeLink`` must be a Youtube video link.
Emits an event ``added``, with the guild where the music was added along information about the music. (cf ``music.musicInfo(index)``)

### Add a local file to the playlist
```js
music.addFile(member, filePath);
```
``member`` represents the guild member that requested the file.
``filePath`` represents the path of the file to play.
Emits an event ``added``, with the guild where the file was added along information about the file. (cf ``music.musicInfo(index)``)

### Remove a Youtube video/file from the playlist
```js
music.removeMusic(guild, index);
```
``index`` represents the index of the music in the playlist. (cf ``music.playlistInfo()``)
Emits an event ``removed``, with the guild from where the music was removed along information about the music. (cf ``music.musicInfo(index)``)

### Skip the current music
```js
music.nextMusic(guild);
```
If the playlist is empty, the bot will stop playing music.
Emits an event ``next``, with the guild where the music was added along basic information about the music. (cf ``music.musicInfo(index)``)
