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

The music handler will emit an event whenever something happens (eg. when the current song is finished or when the playlist is empty), and throw errors when it's trying to do something impossible. (eg. joining someone who isn't in a voice channel)

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

Emits an event ``added``, with the guild where the file was added along information about the file.

### Remove a Youtube video/file from the playlist
```js
music.removeMusic(guild, index);
```
``index`` represents the index of the music in the playlist. (cf ``music.playlistInfo()``)

Emits an event ``removed``, with the guild from where the music was removed along information about the music.

### Skip the current music
```js
music.nextMusic(guild);
```
If the playlist is empty, the bot will stop playing music.

Emits an event ``skipped``, with the guild where the current music was skipped along information about the next music.

### Shuffle the playlist
```js
music.shufflePlaylist(guild);
```
Emits an event ``shuffled``, with the guild where the playlist was shuffled.

### Clear the playlist
```js
music.clearPlaylist(guild);
```
Emits an event ``shuffled``, with the guild where the playlist was cleared.

### Pause/resume the music
```js
music.pauseMusic(guild);
```
Emits an event ``paused``, with the guild where the music was paused.
```js
music.resumeMusic(guild);
```
Emits an event ``resumed``, with the guild where the music was resumed.
```js
music.toggleMusic(guild);
```
Alternates between resume and pause, either emits ``paused`` or ``resumed`` with the guild.

### Set the volume
```js
music.setVolume(guild, volume);
```
``volume`` must be >= 0. By default, it's set to 100.

Emits an event ``volumechange`` along the guild where the volume was changed, the new volume and the old one.

### Set a music to loop
```js
music.toggleLooping(guild);
```
Whether or not the current music must repeat itself upon end.

Emits an event ``looping``, along the guild where it was toggled, the current music and whether or not looping is toggled.

## Useful commands
Those commands are used to ask something to the handler.

### Is the bot connected ?
```js
music.isConnected(guild);
```
Returns a boolean, ``true`` if the bot is connected to a voice channel, ``false`` otherwise.

### Is the bot playing a music ?
```js
music.isPlaying(guild);
```
Returns a boolean, ``true`` if the bot is playing a music, ``false`` otherwise. Throws an error if the bot is not connected.

### Is the bot paused ?
```js
music.isPaused(guild);
```
Returns a boolean, ``true`` if the bot is paused, ``false`` otherwise. Throws an error if the bot is not playing.

### Is the bot looping the current music ?
```js
music.isLooping(guild);
```
Returns a boolean, ``true`` if the bot is looping the current music, ``false`` otherwise. Throws an error if the bot is not playing.

### What is the playlist ?
```js
music.playlistInfo(guild);
```
Returns an array containing information about every music in the playlist, ordered by queue order. Throws an error if the bot is not playing.
