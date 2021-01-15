## How to Migrate your WhatsApp Chats to Telegram

### Jumping Ship
So you may have heard that WhatsApp is planning on sharing the data of its users with its parent company Facebook with the choice to either [opt-in or delete your account](https://www.forbes.com/sites/carlypage/2021/01/08/whatsapp-tells-users-share-your-data-with-facebook-or-well-deactivate-your-account/?sh=1bc6a2e2d46c). Well, Telegram is one of the best alternatives to consider if you decide to do the latter.


>"But what about my precious conversations stuck on WhatsApp?? 😱😱😱"  

There's a solution for that.

The point to note is that if you merely plan on uninstalling the WhatsApp app without deleting your account, provided you've already accepted the new terms, you don't have to worry about losing your chats. Your Google Drive backups will be kept. However, if you want to **delete** your account, *no looking back*, then you have to find a way to save those conversations.

### Enter TLImporter
 [TLImporter](https://github.com/TelegramTools/TLImporter) is a free and open-source tool, made by Fernando Fernández ([ferferga](https://github.com/ferferga)), that allows you to import chats from WhatsApp into Telegram with *relative* ease. It currently supports one-to-one conversations only  *(sorry group chats *😢*)*.

#### Prerequisites
- A WhatsApp account
- A Telegram account for you and your conversation partner
- A PC running Windows or Linux
- Stable internet connection

*Let's get started!*

#### Export your chat from WhatsApp
WhatsApp allows users to export individual conversations into a simple text file. TLImporter uses this text file to do its job.

**\*Side Note** - You can only export and restore messages with this method. Media (such as photos and voice notes) are not supported in text files.

Open the chat you wish to export and tap on the overflow menu button in the top right corner of the screen.

Tap "More" from the menu that pops up, then tap on "Export chat". You will be asked if you wish to export the conversation with or without its media. As mentioned above, TLImporter does not restore media so for this post we shall export the chat without media.

Once you select that option it will process for a while, depending on the volume of the conversation, and finally, bring up the share sheet where you can select who or what app you would like to share the exported chat to.

For this post, let's send it through Telegram to any conversation of your choosing *(just make sure the recipient doesn't mind *😅*)*.

![Steps to export WhatsApp chat](https://cdn.hashnode.com/res/hashnode/image/upload/v1610521972055/02-eGio72.jpeg)

The exported chat will be a text file with the name "WhatsApp Chat with [name of chat partner].txt"

Log in to Telegram web or the native Telegram app on your PC and download the exported file from the chat onto the PC. Place it in a location you can easily find like the desktop.

**\*Side Note** - You can use whichever method you prefer to transfer the exported chat from your phone to your PC.

Finally, rename the file so that there are no spaces between the words. You can replace the spaces with underscores as shown below.

![Rename exported chat file leaving no spaces](https://cdn.hashnode.com/res/hashnode/image/upload/v1610625884378/USIFDTLHK.jpeg)

Open the file and the contents should be simple text with date-and-time-stamped lines of conversation.

![WhatsApp chat backup text file contents](https://cdn.hashnode.com/res/hashnode/image/upload/v1610633109676/xPvTs1-bF.jpeg)

#### Run TLImporter and Log in to your Telegram account
Visit this [link](https://github.com/TelegramTools/TLImporter/releases) to download the latest release of TLImporter for Windows or Linux and run the app.

The app will display a console window with a welcome message and a couple of prompts. Hit "Enter" to proceed as requested

![TLImporter welcome message and prompts](https://cdn.hashnode.com/res/hashnode/image/upload/v1610266996277/f_93-HqZg.png)

The next step is to log in to Telegram. Enter the phone number associated with your Telegram account. Make sure to format it with your **Country code preceding your number**. For example, +233241234567

A verification code will be sent to your Telegram app, type it into the next prompt, and hit "Enter". Once entered correctly you will be successfully signed in to your account.

![Log in to Telegram with phone number and code](https://cdn.hashnode.com/res/hashnode/image/upload/v1610267520698/fIQNSmkbF.jpeg)

#### Select the import format of the conversation
Next, you have a choice to make. You can either choose to import a conversation using two users, in a 1:1 format, or choose to import the chat to your "Saved Messages" in Telegram.

With the 1:1 format, the conversation will be restored by sending the messages from both your device and that of your conversation partner. This preserves the normal structure of the chat and makes it more readable as a conversation.

Importing it into your "Saved Messages" does exactly that. Telegram allows users to send messages to themselves in a "Saved Messages" chat. TLImporter will send the entire conversation history to that chat.

For this post, we will use the 1:1 format so type in "y" and hit "Enter".

#### Log in to your conversation partner's Telegram account
Next, you'll be asked to log in to your conversation partner's Telegram account but before that there's yet another choice to make.

Telegram has a "Secret Mode" 🤫 which is a more secure way of having a conversation. The messages are end-to-end encrypted and none of them are stored on Telegram's servers. Everything is on you and your partner's devices.

TLImporter allows you to choose whether to import the chat into one of these secret chats or a normal chat. For this post, we'll go with the normal chats so type in "n" and hit "Enter".

After that, follow the same steps as earlier to provide your conversation partner's phone number and the verification code sent by Telegram to sign into their accounts in the tool.

![Import chat format and "Secret Mode" options](https://cdn.hashnode.com/res/hashnode/image/upload/v1610662347571/4_Lv_QUu7.png)

#### Set the backup chat file
It's now time to specify the location of the chat backup text file you exported from WhatsApp and put onto your PC. You can type the file's path directly into the tool but an easier and safer way to do it is to simply drag and drop the file into the TLImporter window.

Hit "Enter" once the correct file path has been entered.

#### Set conversation roles
The tool will then ask you to specify who you will be. This will help it know which messages in the exported chat are yours. It is important to **type your name exactly as it appears in the backup chat file**. It's safer to simply copy and paste your username from the chat, making sure to include any emojis or special characters.

Hit "Enter" after entering your name and repeat the same steps for your conversation partner.

![Set conversation roles](https://cdn.hashnode.com/res/hashnode/image/upload/v1610635306460/v3X1lvkRK.jpeg)

#### Review settings and initiate chat import
We're almost at the finish line! The final prompt in the tool shows your current settings for how the messages are going to be formatted when being imported into Telegram.

If you wish to view the description and change some of the presets you can enter the corresponding command. For instance to review the settings for timestamps you can enter "!1" (without the quotes) and hit "Enter". Type in "!C" when you're done to go back.

We aren't going to change any of the settings for this post so we can go ahead and enter "!C" immediately to begin the import.

![Import complete](https://cdn.hashnode.com/res/hashnode/image/upload/v1610706459727/T18VIg03i.jpeg)

**C'est fini!**

If everything goes well you should see your WhatsApp messages automagically flowing into your Telegram chat and it's a thing of beauty in my opinion. Now sit back and relax because depending on the volume of messages in the chat this could take a while, just make sure that your PC is plugged in and stays connected to the internet.

The screenshot below shows how a successful import looks like in Telegram. *(Yes, that's Telegram in WhatsApp clothing for those of us who like to make changes slowly 🐌. Find this theme at this [link](https://t.me/TelegramTips/226).)*

![Screenshot showing chat imported from WhatsApp](https://cdn.hashnode.com/res/hashnode/image/upload/v1610643658193/mj93U_IQm.jpeg)

#### Some points to note
- When exporting your chat without media, WhatsApp exports only 40,000 messages. [Read more...](https://faq.whatsapp.com/android/chats/how-to-save-your-chat-history/?lang=en)
- While importing the messages, TLImporter takes pauses of roughly 7 minutes after every 2000 messages sent to cater for Telegram's flood limits. [Read more...](https://core.telegram.org/bots/faq#my-bot-is-hitting-limits-how-do-i-avoid-this)

### Closing remarks
Having an almost replica of your conversation with someone in another app is a great way to ensure that you have a backup to fall on, you know... just in case. TLImporter's 1:1 format and access to Telegram's "Secret Mode" does this job and does it best. If you have a special conversation you could never dream of losing you should give this tool a shot.
  

**Enjoy your day**

\- ArthurDev



