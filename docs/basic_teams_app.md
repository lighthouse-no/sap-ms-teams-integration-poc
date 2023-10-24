# Create a BAsic MS Teams App

If you follow the instructions for [setting up a conversational bot](https://learn.microsoft.com/en-us/training/modules/msteams-conversation-bots/3-exercise-conversation-bots), everything works nicely until you get to the section called "Test the conversation bot"

## Unsupported Schema Version

The command `gulp ngrok-serve --debug` first invokes `gulp manifest` to the build `manifest.json` file.
This is an important step because the manifest is built from a template that contains variable names (such as `${{MICROSOFT_APP_ID}}`) that need to be substituted for the corresponding values found in the `.env` file.

However, when `gulp manifest` runs, it issues a warning saying `Unable to find "1.16" amongst supported schemas.`

This turns out to be more than a warning, because the manifest file is not built, which in turn means that the `./package` folder containing the app ZIP file is not created.

Even though Microsoft state that `1.16` is the latest schema version, you must change the schema version in the `$schema` and `manifestVerions` fields in the template `./src/manifest/manifest.json` to be `1.13`, otherwise the build process fails.

```json
{
  "$schema": "https://developer.microsoft.com/en-us/json-schemas/teams/v1.13/MicrosoftTeams.schema.json",
  "version": "1.0.0",
  "manifestVersion": "1.13",
```

After this, the `gulp ngrok-serve` command will correctly generate an app ZIP file

## Fix Involving the Preview Version of `yoteams-deploy`

The tutorial instructions describe a fix that they say is needed if, after building the app, you cannot find a `./package` folder.

The fix tells you to install the preview version of `yoteams-deploy`, but ***do not do this*** because, at least in my case, they simply produce a different (but more severe) error:

```shell
$ gulp ngrok-serve
[15:24:42] Found additional Yo Teams plugin: yoteams-deploy
[15:24:42] Found additional Yo Teams plugin: yoteams-deploy
Error [ERR_REQUIRE_ESM]: require() of ES Module /Users/chris/Developer/AkerBP/ConversationalBot/node_modules/yoteams-deploy/node_modules/chalk/source/index.js from /Users/chris/Developer/AkerBP/ConversationalBot/node_modules/yoteams-deploy/dist/execute.js not supported.
Instead change the require of index.js in /Users/chris/Developer/AkerBP/ConversationalBot/node_modules/yoteams-deploy/dist/execute.js to a dynamic import() which is available in all CommonJS modules.
    at Object.<anonymous> (/Users/chris/Developer/AkerBP/ConversationalBot/node_modules/yoteams-deploy/dist/execute.js:10:33)
    at Object.<anonymous> (/Users/chris/Developer/AkerBP/ConversationalBot/node_modules/yoteams-deploy/dist/deployTask.js:11:19)
    at Object.<anonymous> (/Users/chris/Developer/AkerBP/ConversationalBot/node_modules/yoteams-deploy/dist/index.js:30:22)
    at loadPlugins (/Users/chris/Developer/AkerBP/ConversationalBot/node_modules/yoteams-build-core/dist/loadPlugins.js:27:40)
    at Object.setup (/Users/chris/Developer/AkerBP/ConversationalBot/node_modules/yoteams-build-core/dist/index.js:119:35)
    at Object.<anonymous> (/Users/chris/Developer/AkerBP/ConversationalBot/gulpfile.js:21:6) {
  code: 'ERR_REQUIRE_ESM'
}
```

If you have applied this *"fix"*, you need to back it out.
To do this, edit the line in `package.json` that declares the dependency to `yoteams-deplpy` and change it back to

```json
    "yoteams-deploy": "1.4"
```

Then rerun `npm i`

Correcting the schema version number seems to be all that is needed to get the app ZIP file generated in the `./package` directory.

## Deployment Errors

When deploying the app to MS Teams, you may well see the vague error message "Deployment failed".

In order to discover the error details, you need to click on "Copy error to clipboard", then paste the text into an editor.\
Only then will you see the error details.

In my case, the error was due to the fact that both the long and short description field fields were empty.
These can be corrected by editing the template manifest in `./src/manifest/manifest.json`

## Deployed App to MS Teams

Once the deployment errors are fixed, the bot can be added to Teams as an App.

You then can send a message to the bot via the app which uses the temporary `ngrok` public URL to route the request to the app running on your local server.

You can see the request arrive when a message is printed to the `gulp ngrok-serve` console; however, the response stated in the tutorial is not displayed in Teams.

Nonetheless, this is the process of setting up a minimal chat bot in MS Teams
