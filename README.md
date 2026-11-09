# Titanite Link

Titanite Link is the Titanite SMP's fork of [DiscordSRV](https://github.com/DiscordSRV/DiscordSRV) 1.30.5. It links Minecraft chat to Discord and Discord accounts to Minecraft accounts, and it runs on Paper 26.3 with Java 25.

## What this fork changes

The `titanite` branch starts at upstream tag `v1.30.5`. Only these things differ, so upstream fixes still merge cleanly.

- **Plugin name.** The plugin is called `TitaniteLink` and declares `provides: [DiscordSRV]`, so plugins that depend on DiscordSRV still load. The Java packages keep their upstream names, so the DiscordSRV API works unchanged. The `/discord` command stays. The data folder is `plugins/TitaniteLink`, so an old `plugins/DiscordSRV` folder is not picked up.
- **No console channel.** `getConsoleChannel()` always returns nothing, whatever `DiscordConsoleChannelId` says. A message in a console channel can never run a command, and the server console is never forwarded to Discord. The console shows player IP addresses when people log in, and a console channel is a way for anyone with channel access to run commands as the server.
- **Discord messages only reach the game while Titanite is running.** `broadcastMessageToMinecraftServer` drops the message unless a plugin named `Titanite` is installed and enabled. Titanite holds the gate on `DiscordGuildMessagePreProcessEvent`, so without it there is no gate and nothing may pass.
- **Safe defaults in the default config.** A fresh install writes these values (the same in every language file). `DiscordChatChannelAllowedMentions: []` and `DiscordChatChannelTranslateMentions: false` stop game chat from pinging anyone on Discord. `DiscordChatChannelBlockBots: true`. `DiscordChatChannelBroadcastDiscordMessagesToConsole: false`. `DiscordChatChannelRolesAllowedToUseColorCodesInChat: []`. `GroupRoleSynchronizationGroupsAndRolesToSync: {}`, which switches group and role sync off and ships no sample groups. `DiscordConsoleChannelId: ""`. Both ban sync switches (`BanSynchronizationDiscordToMinecraft`, `BanSynchronizationMinecraftToDiscord`) were already `false` upstream and stay that way. These are defaults only, so an existing config keeps whatever it already says.
- **Java 25.** The build targets Java 25 and uses Lombok 1.18.42, because the older Lombok cannot read Java 25 class files.
- **Version and jar name.** The jar is named `TitaniteLink-<version>.jar` (for example `TitaniteLink-1.30.5-titanite.1.jar`). The version is the upstream version plus `-titanite.N`.

Not changed, so you know: the chat-channel console command feature (`DiscordChatChannelConsoleCommandEnabled`, a `!c` prefix) is still on by default upstream, and the update checker still talks to upstream’s GitHub (`UpdateCheckDisabled: true` in `config.yml` turns it off). Turn the console command feature off in `config.yml` unless you want it.

## Licence

DiscordSRV is GPL-3.0 and so is this fork. The [LICENSE](LICENSE) file and the upstream copyright headers stay as they are.

## Building

You need JDK 25. The Gradle wrapper does the rest.

```
./gradlew build -x test
```

The jar lands in `build/libs/`. Use the one without `-original`, `-sources` or `-javadoc` in its name.

## The upstream README

What follows is the upstream README.

![](https://lol.scarsz.me/AiKvTS/Logo-filled-stroke.png "DiscordSRV Logo")

[![](https://discord.com/api/guilds/135634590575493120/widget.png)](https://discordsrv.com/discord "Discord")
[![](https://img.shields.io/github/release/DiscordSRV/DiscordSRV.svg)](https://github.com/DiscordSRV/DiscordSRV/releases/latest "Latest release")
[![](https://img.shields.io/github/downloads/DiscordSRV/DiscordSRV/total.svg)](https://github.com/DiscordSRV/DiscordSRV/releases/latest "GitHub downloads")
![](https://img.shields.io/github/languages/code-size/DiscordSRV/DiscordSRV.svg "Code size in bytes")
[![](https://img.shields.io/github/contributors/DiscordSRV/DiscordSRV.svg)](https://github.com/DiscordSRV/DiscordSRV/graphs/contributors "GitHub contributors")
[![](https://img.shields.io/github/license/DiscordSRV/DiscordSRV.svg)](https://github.com/DiscordSRV/DiscordSRV/blob/master/LICENSE "License")

A Minecraft to Discord and back link plugin

[![](https://bstats.org/signatures/bukkit/DiscordSRV.svg)](https://bstats.org/plugin/bukkit/DiscordSRV/387 "DiscordSRV on bStats")

## Thanks to:
<table>
    <tr>
        <td align="center" width="50%">
            <a href="https://www.jetbrains.com/idea/"><img src="https://resources.jetbrains.com/storage/products/company/brand/logos/jetbrains.svg" alt="Jetbrains" width="400px"></a>
            <p><strong>IntelliJ IDEA - The Leading Java and Kotlin IDE</strong></p>
        </td>
        <td align="center" width="50%">
            <a href="http://www.ej-technologies.com/products/jprofiler/overview.html"><img src="https://www.ej-technologies.com/images/product_banners/jprofiler_large.png" alt="JProfiler" width="150px"></img></a>
            <p><strong>JProfiler's intuitive UI helps you resolve performance bottlenecks, pin down memory leaks and understand threading issues.</strong></p>
        </td>
    </tr>
</table>
