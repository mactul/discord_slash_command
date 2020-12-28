# discord_slash_command

Discordslashcommand is an extension library to discord.py
It allows you to easily manipulate discord slash (/) commands.
You can create commands that will follow a particular patern and that will be easy to use on the discord application.

## installation

This package is on pypi.
You can install it via pip

In a command prompt, run this command:
```
pip3 install discordslashcommands
```
If it doesn't work, try all this commands, maybe one can work.
```
pip install discordslashcommands
py -m pip install discordslashcommands
python3 -m pip install discordslashcommands
py -m pip3 install discordslashcommands
python3 -m install discordslashcommands
py3 -m pip install discordslashcommands
py3 -m pip3 install discordslashcommands
python -m pip install discordslashcommands
python3 -m pip install discordslashcommands
python-pip install discordslashcommands
python3-pip install discordslashcommands
python-pip3 install discordslashcommands
python3-pip3 install discordslashcommands
```

## Documentation

The main class of this libary is the Manager class,
you have to call it at the bot's start.
You can call it again after if you need it.

The minimal code is like this :
```py
import discord
import discordslashcommands as dsc

client = discord.Client()

@client.event
async def on_ready():
    manager = dsc.Manager(client) # this is the code of discordslashcommands libary


client.run("XXXXXXXXXXXXXXXXXXXXXXXXXX")
```


### Create a global slash command

To create a slash command, we need to create this command in a local object and put it on discord.
The local object is call Command()

a simple command without arguments looks like this
```py
command = dsc.Command(name="help", description="display help")
```
To put in on discord, we have to use the manager
```py
manager = dsc.Manager(client)
command = dsc.Command(name="help", description="display help")
manager.add_global_command(command)
```

Here is the full code
```py
import discord
import discordslashcommands as dsc

client = discord.Client()

@client.event
async def on_ready():
    manager = dsc.Manager(client) # create the manager
    command = dsc.Command(name="help", description="display help") # create the command
    manager.add_global_command(command) # put it on discord

client.run("XXXXXXXXXXXXXXXXXXXXXXXXXX")
```


### Create a guild slash command

To create the command localy, it's the same way that for global commands.
The change is only with the manager.
The name of the put function is different and it takes one more argument, the id of the guild.

```py
manager = dsc.Manager(client)
command = dsc.Command(name="help", description="display help")
manager.add_guild_command(REPLACE_WITH_THE_ID_OF_YOUR_GUILD, command)
```

The full code:
```py
import discord
import discordslashcommands as dsc

client = discord.Client()

@client.event
async def on_ready():
    manager = dsc.Manager(client) # create the manager
    command = dsc.Command(name="help", description="display help") # create the command
    manager.add_guild_command(REPLACE_WITH_THE_ID_OF_YOUR_GUILD, command) # put it on discord

client.run("XXXXXXXXXXXXXXXXXXXXXXXXXX")
```


### Command object

For all next steps, we need to know better the Command class.
We have seen a simple command like that:
```py
command = dsc.Command(name="help", description="display help")
```
But we can imagine that the help command can takes arguments, to print parts of help for example.
We will add an argument, name category, it represents the part of the help that we want print.
This argument has several predefined values.
3 for example, an help for premium, for moderation and for music

We need to add an argument (an option), with a name, a description and predefined values

To do that, we need a new object, call Option
```py
command = dsc.Command(name="help", description="display help") # create the main object

option = Option(name="category", description="the part of the help you want to display", type=dsc.STRING, required=False)

option.add_choice(name="premium part", value="premium")
option.add_choice(name="moderation part", value="moderation") # if you don't understand the difference between name and value, put the same string into
option.add_choice(name="music part", value="music")

command.add_option(option) # add the new option created in the command
```

To use this version of the help command, this is the full code
```py
import discord
import discordslashcommands as dsc

client = discord.Client()

@client.event
async def on_ready():
    manager = dsc.Manager(client) # create the manager
    
    command = dsc.Command(name="help", description="display help") # create the commandcommand = dsc.Command(name="help", description="display help") # create the main object
    option = Option(name="category", description="the part of the help you want to display", type=dsc.STRING, required=False)
    option.add_choice(name="premium part", value="premium")
    option.add_choice(name="moderation part", value="moderation") # if you don't understand the difference between name and value, put the same string into
    option.add_choice(name="music part", value="music")
    command.add_option(option) # add the new option created in the command
    
    manager.add_guild_command(REPLACE_WITH_THE_ID_OF_YOUR_GUILD, command) # put it on discord

client.run("XXXXXXXXXXXXXXXXXXXXXXXXXX")
```


### Get all global slash commands

Maybe, you need to have a full list of the commands of your application.
To get it, you can use the manager with the `get_all_global_commands()` function
This function takes no arguments and return a list of Command objects.

```py
commands = manager.get_all_global_commands()
```

They Command objects returned are like this:
```
Command
    .name: name of the command
    .description: description of the command
    .id: id of the command, to make actions on it
    .options: a list of Option objects
        list
            .name: name of the option
            .description: description of the option
            .type: type of the option (dsc.STRING, dsc.INTEGER, etc...)
            .required: boolean represent if the option is required
            .choices: a list of choices
                list
                    dictionnary
                        key "name": name of the choice
                        key "value": value of the choice

    .add_option(Option): function detailed above

    .delete(): delete the Command directly from discord, without manager object
    
```

documentation is coming...
Refer to the test.py file for examples of the undocumented part


