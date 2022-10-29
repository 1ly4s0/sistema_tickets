![Node build](https://github.com/eritislami/evobot/actions/workflows/node.yml/badge.svg)

![logo](https://cdn.discordapp.com/attachments/933698201486237716/947555143795228682/Diseno_sin_titulo_22.png)

# 🤖 Tutorial Discord Bot (TECNO BROS)
> Código del bot del tutorial de TECNO BROS, el vídeo es [este](https://www.youtube.com/watch?v=H2Sg9LQo7oo)
## Requisitos

1. Tener un bot de Discord creado **[Guía](https://www.youtube.com/watch?v=qXev2kf-q_0)**
2. Tener Discord.js V12 o Discord.js V13 **[Guía](https://www.youtube.com/watch?v=qXev2kf-q_0)**
3. Tener el bot hosteado o en tu PC **[Guía](https://www.youtube.com/watch?v=0MkVTtLoMiI)**  
4.1 **(Opcional)** Tener el bot en algun host **[Guía](https://www.youtube.com/watch?v=0MkVTtLoMiI)**

## 🚀 NPM'S REQUERIDOS

```js
const transcripts = require("discord-html-transcripts");
```

## 🚀 Código de TICKET-SETUP

```js



client.on("messageCreate", async message => {
    if(message.content.startsWith("tb!ticket-setup")) {
        const canal = message.mentions.channels.first();
        const rol = message.mentions.roles.first();

        if(!canal) return message.channel.send("Pon un canal para configurarlo en el sistema de tickets.")
        if(!rol) return message.channel.send("Pon un rol para configurarlo en el sistema de tickets")

        db.set(`ticket_rol_${message.guild.id}`, rol.id);

        const Embed = new Discord.MessageEmbed()
        .setDescription(`Sistema de tickets configurado correctamente. El ticket establecido es <@&${rol.id}> y el canal es ${canal}`)

        message.channel.send({ embeds: [Embed] });

        const Embed1 = new Discord.MessageEmbed()
        .setTitle("Sistema de tickets")
        .setDescription(`Para abrir un ticket, persona el botón de 🎫`)

        const Boton = new MessageButton()
        .setCustomId("ticket")
        .setLabel("Abrir ticket")
        .setStyle("PRIMARY")

        const row = new Discord.MessageActionRow()
        .addComponents(Boton)


        canal.send({ embeds: [Embed1], components: [row] })
    }
})




```

## 🚀 Código de TICKET-TRANSCRIPTS

```js

client.on("messageCreate", async message => {
    if(message.content.startsWith("tb!transcripts-setup")) {
        const canal = message.mentions.channels.first();

        if(!canal) return message.channel.send("Pon un canal para configurarlo en el sistema de transcripts.")

        db.set(`ticket_transcripts_${message.guild.id}`, canal.id);

        const Embed = new Discord.MessageEmbed()
        .setDescription(`Sistema de transcripts configurado correctamente. El canal establecido es ${canal}`)

        message.channel.send({ embeds: [Embed] });

        const Embed1 = new Discord.MessageEmbed()
        .setTitle("Sistema de transcripts")
        .setDescription(`¡Ahora todos los transcripts de los tickets se enviarán aquí!`)

        canal.send({ embeds: [Embed1] })
    }
})

```

## 🚀 Código de EVENTOS DE BUTTONS

```js

client.on("interactionCreate", async interaction => {
    if(interaction.customId === "ticket") {
        const rol = await db.get(`ticket_rol_${interaction.guild.id}`);
        //await interaction.deferUpdate();
        const canales = interaction.guild.channels.cache.filter(c => c.name === `ticket-${interaction.user.username}`);
        if(canales.size > 0) return interaction.reply({ content: "Ya tienes un ticket abierto.", ephemeral: true });
        interaction.guild.channels.create(`ticket-${interaction.member.user.username}`, {
            type: "GUILD_TEXT",
            permissionOverwrites: [
                {
                    id: interaction.guild.roles.everyone,
                    deny: ["VIEW_CHANNEL"]
                },
                {
                    id: interaction.member.id,
                    allow: ["VIEW_CHANNEL", "SEND_MESSAGES"]
                },
                {
                    id: rol,
                    allow: ["VIEW_CHANNEL", "SEND_MESSAGES"]
                }
            ]
        }).then(async channel => {
            const Embed = new Discord.MessageEmbed()
            .setTitle("Ticket")
            .setDescription("Bienvenido al ticket espera mientras le atienden. \n Para cerrar el ticket, presiona el botón de cerrar.")

            const Boton = new MessageButton()
            .setCustomId("cerrar_ticket")
            .setLabel("Cerrar ticket")
            .setStyle("DANGER")

            const row = new Discord.MessageActionRow()
            .addComponents(Boton)

            channel.send(`<@${interaction.member.id}>, <@&${rol}>`)
            channel.send({ embeds: [Embed], components: [row] })

            await interaction.reply({ content: `Ticket <#${channel.id}> creado correctamente.`, ephemeral: true })
        })
    }

    if(interaction.customId === "cerrar_ticket") {

        const canal_transcripts = await db.get(`ticket_transcripts_${interaction.guild.id}`)

        interaction.deferUpdate().catch(() => {});

        const transcripts = require("discord-html-transcripts");

        const canal = interaction.channel;
        
        let transcript = await transcripts.createTranscript(canal)

        await client.channels.cache.get(canal_transcripts).send({ content: `Transcript del ticket:`, files: [transcript] })
       
        interaction.channel.delete();
    }
})

```
Después de modificar o añadir algo al código, recuerda o reiniciar tu bot o usar el comando `node index.js` para que los cambios se apliquen.

## ⚙️ Uso:

## LOCK
![logo](https://cdn.discordapp.com/attachments/933698201486237716/1036045350269628416/unknown.png)

## UNLOCK
![logo](https://cdn.discordapp.com/attachments/933698201486237716/1036045392997011516/unknown.png)





## 📝 Créditos
* [DISCORD](https://discord.gg/tecnobros)
* [YOUTUBE](https://youtube.com/tecnobros)

Copyright © **1ly4s0#2477** - 2022
