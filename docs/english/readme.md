# 🌍 Welcome to the English Guide!

✨ **Version 1.3.0**

```
npm i djs-bot-base
```

# ⏰ Long-term Plans

- Preparations will be made for Version 2. Slash and message commands will be merged, and a few new features will be added!

# 🚀 New Features

- `<CommandHandler>.getCommands()`, `<CommandHandler>.getSlashCommands()`, and `<EventHandler>.getEvents()` have been added. I don't know why I didn't add them before, but let's keep that between us 😓
- The warning message that appeared when starting the bot with suppressed warnings has been removed.

# 🧰 Bug Fixes

- Clean for now!

# 🏅 Example Bot Main File (e.g., index.js)

ESM:

```js
import { CommandHandler, EventHandler } from "djs-bot-base";
import { Client, Partials } from "discord.js";
// Refer to the sample code
```

CommonJS:

```js
const { CommandHandler, EventHandler } = require("djs-bot-base");
const { Client, Partials } = require("discord.js");
// Refer to the sample code
```

Sample code:

```js
const commands = new CommandHandler({ prefix: "!" });
const events = new EventHandler();

const client = new Client({
  intents: ["MessageContent", "GuildMessages", "Guilds", "DirectMessages"],
  partials: [Partials.User, Partials.Channel, Partials.Message],
});
const token = "bot token";

(async () => {
  await commands.setCommands();
  await commands.setSlashCommands();
  await events.setEvents(client);
  commands.setDefaultHandler(client).setDefaultSlashHandler(client);
  await client.login(token);
  await commands.registerSlashCommands(client);
})();
```

# 📦 Module Contents

- **Note**: The export of command and event classes should be set as default export.

# 🛠️ Commands

## 🗝️ Command Class

```js
export default new Command({
  name: "test", // Required, command name
  aliases: ["demo"], // Optional, command aliases
  guildOnly: false, // Optional, whether the command can only be used in guilds
  dmOnly: false, // Optional, whether the command can only be used in DMs
  developerOnly: true, // Optional, whether the command is developer-only
  async run(message) {
    // Required, code to run when the command is triggered
  },
});
```

- **Note**: If you plan for your command to run only in guilds/DMs, enabling the `guildOnly/dmOnly` variable will provide more accurate information from your code editor.
  <br>⚠️ Enabling both at the same time will have no effect.

## 🗝️ Slash Command Class

```js
export default new SlashCommand({
  slashCommandData: (builder) =>
    builder.setName("command-name").setDescription("Command description"), // Required, slash command data
  async run(interaction) {
    // Required, code to run when the command is triggered
  },
});
```

## 💻 Command Handler Class

```js
const commands = new CommandHandler({
  commandsDir: "./commands", // Optional, default: "commands", folder name for commands to be scanned
  slashCommandsDir: "./slashCommands", // Optional, default: "slashCommands", folder name for slash commands to be scanned
  suppressWarnings: true, // Optional, suppresses warnings in the command handler
  prefix: "!", // Optional, only needed if you set a custom prefix
  developerIds: ["developer ids"], // Optional, developer IDs
});
```

- **Note**: Having the same folder for slash commands and normal commands will cause conflicts and errors.

- The command system is divided into slash commands and normal commands:

  ### <u>Normal Commands</u>

  - `<CommandHandler>.setCommands()`

    Reads the specified folder and registers normal commands to the handler.
    <br>💡 It is recommended to use `await` to wait for the commands to be read.

  - `<CommandHandler>.getCommand(<string>)`

    Returns the normal command that matches the given name from the handler.

  - `<CommandHandler>.getCommands()`

    Returns all normal commands in the handler.

  - `<CommandHandler>.getCommandOrAliases(<string>)`

    Returns the normal command that matches the name or aliases in the handler.

  - `<CommandHandler>.clearCommands()`

    Clears all normal commands in the handler.

  - `<CommandHandler>.removeCommand(<string>)`

    Removes the normal command that matches the given name from the handler.

  - `<CommandHandler>.setDefaultHandler(<Client>)`

    Registers the default normal command handler as an event in the module.
    <br>⚠️ Does not support features like server-specific prefixes.

  - `<CommandHandler>.runDefaultHandler(<Message>, <string?>)`

    Runs the default normal command handler in the module.
    <br>💡 It is recommended to use this in an event instead of `setDefaultHandler(<Client>)`.

  - `<CommandHandler>.removeDefaultHandler(<Client>)`

    Removes the registered default normal command handler from the bot.

  ### <u>Slash Commands</u>

  - `<CommandHandler>.setSlashCommands()`

    Reads the specified folder and registers slash commands to the handler.
    <br>💡 It is recommended to use `await` to wait for the commands to be read.

  - `<CommandHandler>.getSlashCommand(<string>)`

    Returns the slash command that matches the given name from the handler.

  - `<CommandHandler>.getSlashCommands()`

    Returns all slash commands in the handler.

  - `<CommandHandler>.clearSlashCommands()`

    Clears all slash commands in the handler.

  - `<CommandHandler>.removeSlashCommand(<string>)`

    Removes the slash command that matches the given name from the handler.

  - `<CommandHandler>.setDefaultSlashHandler(<Client>)`

    Registers the default slash command handler as an event in the module.

  - `<CommandHandler>.runDefaultHandler(<Message>, <string?>)`

    Runs the default slash command handler in the module.
    <br>💡 It is recommended to use this in an event instead of `setDefaultSlashHandler(<Client>)`.

  - `<CommandHandler>.removeDefaultHandler(<Client>)`

    Removes the registered default slash command handler from the bot.

  - `<CommandHandler>.registerSlashCommands(<Client>, <string?>)`

    Registers the saved commands in the handler to the bot, making them available for use. Server-specific commands can be registered.

# 📻 Events

## 🗝️ Event Class

```js
export default new Event({
  categoryName: "ready", // Required, category of the event
  runOrder: 1, // Optional, execution order of the event, the smallest number runs first (minimum is 0), if not specified, it runs after specified values
  async run(client) {
    // Required, code to run when the event is triggered
  },
});
```

- **Note**: If several events have the same or unspecified execution order, they will be sorted by their reading order.
- **Note**: Events with specified execution order are prioritized.

## 💻 Event Handler Class

```js
const events = new EventHandler({
  eventsDir: "./events", // Optional, default: "events", folder name for events to be scanned
  suppressWarnings: false, // Optional, suppresses warnings in the event handler
});
```

- `<EventHandler>.setEvents(<Client>)`

  Reads the specified folder, registers the events to the handler, and adds them to the bot.
  <br>💡 It is recommended to use `await` to wait for the events to be read.

- `<EventHandler>.getEventCategory(<string>)`

  Returns the events and additional data in the specified event category from the handler.

- `<EventHandler>.getEvents()`

  Returns all events in the handler.

- `<EventHandler>.clearEvents()`

  Clears all events in the handler and the bot.

# 🪰 Found a Bug?

- 🐜 If you find a bug and know how to fix it, you can open a [pull request](https://github.com/Wyntine/DjsBotBase/compare)!
- 📱 If you want to reach me, you can contact me on [Discord](https://discord.com/users/920360120469311578)!

---

<p align="center">Developed by Egecan Akıncıoğlu & Orkun Demirci</p>
