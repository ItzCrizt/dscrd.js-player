# dscrd.js-player

A simple and easy-to-use discord music package that uses the [@distube/ytdl-core](https://www.npmjs.com/package/@distube/ytdl-core), [@distube/ytsr](https://www.npmjs.com/package/@distube/ytsr) and [@distube/ytpl](https://www.npmjs.com/package/@distube/ytpl)

# Features

- Supports Youtube Tracks/Playlist

# Be added soon

- Spotify Plugin - can play spotify songs, might also supports tracks/playlist
- Soundcloud Plugin - same as spotify, can play soundcloud songs

# Getting Started

Install dscrd.js-player

```bash
npm i dscrd.js-player-beta@latest
```

**RECOMMENDED**

Install ffmpeg-static and opusscript or @discordjs/opus

```bash
npm i ffmpeg-static @discordjs/opus opusscript
```

# Usaage

```js
const { Client, Intents, MessageEmbed } = require('discord.js');
const MusicPlayer = require('dscrd.js-player-beta');

const client = new Client({ 
    intents: [
        Intents.FLAGS.GUILDS, 
        Intents.FLAGS.GUILD_VOICE_STATES, 
        Intents.FLAGS.GUILD_MESSAGES
    ] 
});

const musicPlayer = new MusicPlayer(client);

client.once('ready', () => {
  console.log(`Logged in as ${client.user.tag}`);
});

client.on('messageCreate', async (msg) => {
  if (msg.content.startsWith('!play')) {
    const query = msg.content.split(' ').slice(1).join(' ');
    await musicPlayer.play(msg, query);
  }
});

musicPlayer.on('trackAdd', (msg, track) => {
  msg.channel.send(`**${track.info.title}** has been added to the queue`);
});

musicPlayer.on('playlistAdd', (msg, playlist) => {
  msg.channel.send(`**${playlist.title}** playlist has been added to the queue`);
});

musicPlayer.on('trackStart', (msg, track) => {
  const embed = new MessageEmbed()
    .setTitle(track.info.title)
    .setURL(track.info.video_url)
    .setAuthor(track.info.author)
    .addField('Duration', `${Math.floor(track.info.lengthSeconds / 60)}:${track.info.lengthSeconds % 60}`)
    .setThumbnail(track.info.thumbnails[0].url)
    .setDescription('Now playing in the voice channel!');

  const textChannel = msg.channel;
  if (textChannel) {
    textChannel.send({ embeds: [embed] });
  }
});

musicPlayer.on('queueEnd', (msg) => {
  const textChannel = msg.channel;
  if (textChannel) {
    textChannel.send('The queue has ended.');
  }
});

musicPlayer.on('error', (msg, error) => {
  msg.channel.send(error);
});

client.login("BOT_TOKEN");
```

# Documentation

Coming SOON

# Others

You can also use this with [dscrd.js](https://www.npmjs.com/package/dscrd.js?activeTab=readme). I will update this time to time also just like in __dscrd.js__. For now enjoy this update. I will post the updated version of it soon!
