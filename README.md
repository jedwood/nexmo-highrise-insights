# Nexmo &rarr; Highrise Insights
Have phone number insights from Nexmo appear in your Highrise contacts.

Using the new [Nexmo Number Insight service](https://www.nexmo.com/product/number-insight/), you can find out information about your contacts' phone numbers such as whether it's a mobile or landline number, which carrier it belongs to, and whether it's a valid number.

![Fake Highrise contact with real number and insight](https://cldup.com/4ejQ9bQkuq.png)

**The easiest way to "clone" this project is to duplicate the [Google Spreadsheet](https://docs.google.com/spreadsheets/d/1eu9H86cWmd5YF3_hyxpxmRktM160kiW4BQ6GOlhY0As/edit?usp=sharing) and follow its insructions.** I made this project with Highrise customers in mind— small business owners that are generally tech savvy, but that aren't necessarily web developers. So I chose an uncommon platform: Google Sheets + Google Apps Script (aka "GAS"). The initial setup, timed triggers, Nexmo callback endpoint, and decent permissions/security can all be had for free, with no server to spin up nor maintain.

I've put the source files here in this git repo for reference. They look like JavaScript (because GAS is essentially a superset of JS) but they would need some minor modifications in order to run in Node.js, for example.

## Setup Instructions
1. Sign in with your Google account, open the spreadsheet, click "File > Make a copy..."
2. Enter you own Nexmo and Highrise account information on the "config" sheet, in the cells that say `[YOUR ...]`
3. Click `Tools > Script editor...` and the script will open in a new tab/window.
4. In the script window, click `Publish > Deploy as web app...`. Enter any name for the version. "Execute the app as:" should be "me." "Who has access to the app:" should be "Anyone, even anonymous", click `Deploy`.
5. To the right of the bug icon, select "syncWithHighrise" from the function list. Click the "play" icon.
6. Authorize when prompted by confirming that you allow the script to blah blah blah. Within a few seconds, you should see the Nexmo data appear in the "insights" tab in the spreadsheet, as well as on individual contacts in Highrise.
7. You can now set up a timed trigger to run as often as you'd like, under "Resources > Current Project's Triggers"

## Features and Quirks
- Optionally, you can enter a tag (e.g. prospect) and only contacts with that tag will be considered.
- Each time the script runs, it will save a `last_sync` date. On future runs, only contact records that have been udpated since that last date will be affected. If you want to start over and have all records consider, just delete the value in that column of the `config` sheet.
- For now, each time Nexmo Insights is called on a record, it will _prepend_ new info to the note. This accounts for multiple phone numbers, and changes of status (e.g. roaming) to existing numbers. There are however some drawbacks including unwanted duplicate info, so this might change in the future.