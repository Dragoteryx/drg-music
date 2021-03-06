# drg-music
![Drg Music](https://nodei.co/npm/drg-music.png?downloads=true&stars=true)

A single file module to easily manage Discord music bots using Discord.js

## How to use ?
First, you need to require the module.
```js
const drgMusic = require("drg-music");
```
Then, you must create a new MusicHandler.
```js
const handler = new drgMusic.MusicHandler(client);
```
``client`` represents your Discord.js client (your bot).

Then, you'll need to interact with the MusicHandler you just created.
However some functions do not require the use of a MusicHandler.

#### What can it do ?
With this module, you can ask the bot to join and leave a voice channel, to request and play a Youtube video, to pause/resume, to set the volume, etc.
Most commands will require you to specify the guild where you want to execute the action and return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).
The music handler will emit an event whenever something happens. (eg. when the current song is finished or when the playlist is empty)

##### Join a voice channel
```js
handler.join(member);
```
``member`` is the guild member that the bot will join.

Returns a resolved Promise, or a rejected Promise with an error if something goes wrong.

#### Leave a voice channel
```js
handler.leave(guild);
```

Returns a resolved Promise, or a rejected Promise with an error if something goes wrong.

#### Add a Youtube video or a local file to the playlist
```js
handler.pushMusic(options);
```
``options`` is an object containing the following information:
``options.link`` is a Youtube link.
``options.path`` is a path to a local file.
``options.query`` is a Youtube query.
``options.member`` is a GuildMember (the one who requested the music)
``options.passes`` is the number of passes. (optional)
``options.props`` is whatever you want it to be, it's an object where you can store any data you want and access it when you get info from a music. (optional)

Returns a Promise resolved with the music that was added.

#### Remove a Youtube video/file from the playlist
```js
handler.removeMusic(guild, index);
```
``index`` represents the index of the music in the playlist.

Returns a Promise resolved with the music that was removed.

#### Skip the current music
```js
handler.nextMusic(guild);
```
If the playlist is empty, the bot will stop playing music.

Returns a Promise resolved with the music that was skipped.

#### Shuffle the playlist
```js
handler.shufflePlaylist(guild);
```

Returns a resolved Promise.

#### Clear the playlist
```js
handler.clearPlaylist(guild);
```

Returns a resolved Promise.

#### Pause/resume the music
```js
handler.pauseMusic(guild);
```
Returns a resolved Promise.
```js
handler.resumeMusic(guild);
```
Returns a resolved Promise.
```js
handler.toggleMusic(guild);
```
Returns a Promise resolved with a boolean set to ``true`` if the music is paused, ``false`` otherwise.

#### Set the volume
```js
handler.setVolume(guild, volume);
```
``volume`` must be >= 0. By default, it's set to 100.

Return a Promise resolved with the old volume.

#### Set a music to loop
```js
handler.toggleLooping(guild);
```
Whether or not the current music must repeat itself upon end.

Returns a Promise resolved with a boolean set to ``true`` if the music is looping, ``false`` otherwise.

### Useful commands
Those commands are used to ask something to the handler.

#### Is the bot connected ?
```js
handler.isConnected(guild);
```
Returns a boolean, ``true`` if the bot is connected to a voice channel, ``false`` otherwise.

#### Is the bot playing a music ?
```js
handler.isPlaying(guild);
```
Returns a boolean, ``true`` if the bot is playing a music, ``false`` otherwise. Throws an error if the bot is not connected.

#### Is the bot paused ?
```js
handler.isPaused(guild);
```
Returns a boolean, ``true`` if the bot is paused, ``false`` otherwise. Throws an error if the bot is not playing.

#### Is the bot looping the current music ?
```js
handler.isLooping(guild);
```
Returns a boolean, ``true`` if the bot is looping the current music, ``false`` otherwise. Throws an error if the bot is not playing.

#### What is the playlist ?
```js
handler.playlistInfo(guild);
```
Returns an array containing information about every song in the playlist, ordered by queue order. Throws an error if the bot is not playing.

#### What is the size of the playlist ?
```js
handler.playlistSize(guild);
```
Returns the number of songs in the playlist. Throws an error if the bot is not playing.

#### Is the playlist empty ?
```js
handler.isPlaylistEmpty(guild);
```
Whether or not the playlist is empty. Throws an error if the bot is not playing.

#### How many time remaning before the end of the playlist ?
```js
handler.remainingTime(guild);
```
The time remaining before the playlist ends, in milliseconds.

#### Information about a music in the playlist
```js
handler.musicInfo(guild, index);
```
Returns information about a music in the playlist. eg.
```js
{
  title: 'Me at the zoo', // the title of the video
  description: 'The first video on YouTube. Maybe it\'s time to go back to the zoo? The name of the music playing in the background is Darude - Sandstorm.', // the description of the video
  author: // the channel that uploaded the video
   { name: 'jawed',
     avatarURL: 'https://yt3.ggpht.com/-5rLAp8qGoEY/AAAAAAAAAAI/AAAAAAAAAAA/LtzVhVaf_do/s88-c-k-no-mo-rj-c0xffffff/photo.jpg',
     channelURL: 'https://www.youtube.com/channel/UC4QobU6STFB0P71PMvOGN5A' },
  thumbnailURL: 'https://i.ytimg.com/vi/jNQXAC9IVRw/default.jpg', // the thumbnail of the video
  length: 19000, // the length of the video (in milliseconds)
  link: 'https://www.youtube.com/watch?v=jNQXAC9IVRw', // the link of the video
  member: SOMEONE // the guild member that requested the video
  file: false, // whether or not this music is a local file
  time: 2500 // the time since this video has been playing (in milliseconds)
}
```
``index`` is the index of the music in the playlist. (cf ``handler.playlistInfo(guild)``)
Local file will have most of those properties ``undefined``. (only ``title``, ``link`` and ``member`` are available on a local file)

#### Information about the current music
```js
handler.playingInfo(guild);
```
Returns information about the current music.

### Other functions
Those functions do not require the use of a MusicHandler.

#### Convert milliseconds to a timer
```js
let timer = drgMusic.millisecondsToTime(milliseconds);
```
For example:
```js
let milliseconds = 1000;
let timer = drgMusic.millisecondsToTime(milliseconds);
console.log(timer); // 0:01
```

#### Play a Youtube video without the use of a playlist
```js
let dispatcher = drgMusic.playYoutube(ytblink, voiceConnection, passes);
```
``voiceConnection`` => https://discord.js.org/#/docs/main/stable/class/VoiceConnection
``dispatcher`` is an instance of ``StreamDispatcher`` => https://discord.js.org/#/docs/main/stable/class/StreamDispatcher
``passes`` represents the number of passes. Set it to a high number to prevent packet loss.
I do not recommend the use of this function, but you can use it.

####

### Events
* When the current music is finished, an event ``finished`` is emitted with the guild and the music.
* When playing the next music, an event ``next`` is emitted with the guild and the next music.
* When the current music is finished and the playlist is empty, an event ``empty`` is emitted with the guild.

### Errors
* memberNotInAVoiceChannel : when the bot is trying to join a member who is not connected in a voice channel.
* voiceChannelNotJoinable : when the bot is trying to join a voice channel it can't join.
* voiceChannelNotSpeakable : when the bot is trying to join a voice channel in which it's not allowed to speak.
* voiceChannelFull : when the bot is trying to join a voice channel which is full.
* clientAlreadyInAVoiceChannel : when someone requested the bot to join but it's already connected in a voice channel.
* clientNotInAVoiceChannel : when the bot tries to do something requiring being connected while it's not. (eg. leaving a voice channel)
* unknownOrNotSupportedVideoWebsite : trying to request a link not from Youtube.
* notPlayingMusic : when the bots tries to do something requiring playing while it's not. (eg. pausing)
* invalidVolume : volume must be at least 0, no upper limit.
* emptyPlaylist : when the bot is trying to do something which requires the playlist not to be empty.
* invalidPlaylistIndex : trying to access a bigger index than the playlist's size. (eg. trying to delete the 3rd music when there are only 2 songs in the playlist)

## Example
Here is an example of a Discord bot using this module. You can also check my bot DraBOTeryx which is using this plugin here : https://github.com/Dragoteryx/draboteryx
```js
const Discord = require("discord.js");
const drgMusic = require("drg-music");

const client = new Discord.Client();
const handler = new drgMusic.MusicHandler(client);
let musicChannels = new Map();

handler.on("next", (guild, music) => {
	musicChannels.get(guild.id).send("Now playing: " + music.title + " by" + music.author.name + ". (requested by " + music.member +")");
});
handler.on("empty", guild => {
	musicChannels.get(guild.id).send("The playlist is empty.");
});

client.on("message", message => {

  if (message.content == "/join") {
    musicChannels.set(msg.guild.id, msg.channel);
    handler.join(message.member).then(() => {
      message.channel.send("Hello, I'm here to play you some music :)");
    });
  }

  if (message.content == "/leave") {
    handler.leave(message.guild).then(() => {
      message.channel.send("Goodbye :)");
    });
    musicChannels.delete(msg.guild.id);
  }

  if (message.content.startsWith("/request ")) {
    let options = {
      member: message.member,
      link: message.replace("/request ",""),
      props: {time: new Date()}
    }
    handler.addMusic(options).then(added => {
      message.channel.send("The music " + added.title + " was added to the playlist. (requested at " + added.props.time + ")");
    }, err => {
      message.channel.send("Sorry but I can't play that music for unknown reasons.");
      console.error(err);
    });
  }

  // ETC (other commands)

});

client.login(MYBOTTOKEN);
```
