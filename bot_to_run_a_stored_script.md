# node_bot

To create a Node.js bot that can run stored scripts, you can follow these steps. In this example, I'll create a simple bot that executes JavaScript code stored in text files. Please note that executing arbitrary code can be a security risk, so make sure you have proper security measures in place, such as code validation and sandboxing, if you decide to execute untrusted code.

1. Set up your Node.js environment:

Make sure you have Node.js installed on your system. You can download it from the official website: https://nodejs.org/

2. Create a project directory and initialize it with npm:

```bash
mkdir bot
cd bot
npm init
```

Follow the prompts to create a `package.json` file for your project.

3. Install required dependencies:

You'll need a few dependencies to create your bot:

```bash
npm install discord.js vm2
```

- `discord.js`: A library for interacting with the Discord API.
- `vm2`: A sandbox for running JavaScript code securely.

4. Create your bot script:

Create a JavaScript file for your bot, e.g., `bot.js`. This script will handle interacting with Discord and executing stored scripts.

```javascript
const Discord = require('discord.js');
const VM2 = require('vm2');
const fs = require('fs');

const client = new Discord.Client();
const vm = new VM2({
  sandbox: {},
  timeout: 1000, // Set a time limit for script execution (in milliseconds).
});

client.on('ready', () => {
  console.log(`Logged in as ${client.user.tag}`);
});

client.on('message', async (message) => {
  if (message.content.startsWith('!runscript')) {
    // Get the script content from a text file
    const scriptFilename = message.content.replace('!runscript', '').trim();
    try {
      const script = fs.readFileSync(scriptFilename, 'utf8');
      const result = vm.run(script);

      message.reply(`Script executed with result: ${result}`);
    } catch (error) {
      message.reply(`Error while executing the script: ${error.message}`);
    }
  }
});

// Replace 'YOUR_BOT_TOKEN' with your actual Discord bot token.
client.login('YOUR_BOT_TOKEN');
```

5. Create and store scripts:

You can create text files containing the JavaScript code you want to run. For example, you can have a script file called `myScript.js` with the following content:

```javascript
const result = 2 + 2;
result;
```

6. Run your bot:

To run your bot, execute the following command:

```bash
node bot.js
```

Your bot will connect to the Discord server and listen for messages starting with `!runscript`. When it receives a command, it reads the script from a text file, executes it in a sandboxed environment, and sends the result back to the Discord channel.

Remember to replace 'YOUR_BOT_TOKEN' with your actual Discord bot token and adjust the code as needed for your specific use case. Also, consider security best practices when running user-provided code in a sandbox.
