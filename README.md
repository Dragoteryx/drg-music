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
``client`` is your Discord.js client (your bot).

Then, you'll need to interact with the MusicHandler you just created.

## What can it do ?
This module lets you
You can ask the bot to join.
```js
music.join(member);
```
You need to specify which guild member the bot must join.
