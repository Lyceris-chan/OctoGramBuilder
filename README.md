## OctoGramBuilder GitHub Actions Workflow Setup

To get your OctoGramBuilder up and running with GitHub Actions workflow, adhere to the following guidelines:

### **Step 1: Firebase Configuration**

1. Head to the [Firebase Console](https://console.firebase.google.com/).
2. Create a new project.
3. Click the Android icon at the center of the project overview page.
4. For the Android package name, use: `it.octogram.android`.
5. Download your google-services.json.

### **Step 2: Obtaining Telegram API_ID and API_HASH**

To create your own application using the Telegram API, follow these steps:

1. Log in to your Telegram core account by visiting: [https://my.telegram.org](https://my.telegram.org).
2. Access the "API development tools" section and complete the provided form.
3. Upon completion, you will receive basic addresses, along with the `api_id` and `api_hash` parameters required for user authorization.

### **Step 3: Maps API Key Setup**

1. Create a new file named `local.properties`, and include the following line as a placeholder, unless you intend to acquire an actual key. Instructions for obtaining a valid key can be found [here](https://developers.google.com/maps/documentation/javascript/get-api-key):

    ```
    MAPS_API_KEY=Abcdefgh:982735541'
    ```

### **Step 4: Keystore Configuration**

In your `signing.properties` file, include the following lines with your keystore information:

```
storePassword=<your-keystore-password>
keyAlias=<your-keystore-alias>
keyPath=<your-keystore-file-path>
keyPassword=<your-keystore-password>
```

To generate this information, follow these steps:

1. Open a terminal in your desired directory.
2. Run the following command to create the keystore file and set the keystore password:

```
keytool -genkey -v -keystore <your-keystore-file-name>.jks -keyalg RSA -keysize 2048 -validity 10000 -alias <your-keystore-alias> -storetype JKS -keypass <your-keystore-password> -storepass <your-keystore-password>
```

### **Step 5: Update Build Configuration**

1. Download the `BuildVars.java` file from [this link](https://github.com/OctoGramApp/OctoGram/blob/revamp/TMessagesProj/src/main/java/org/telegram/messenger/BuildVars.java).

2. Edit the `BuildVars.java` file and replace the `APP_ID` and `APP_HASH` values with the ones obtained in [Step 2](https://github.com/Lyceris-chan/OctoGramBuilder#step-2-obtaining-telegram-api_id-and-api_hash).

### **Step 6: Repository Setup**

1. Create a private GitHub repository.

2. Upload the following files to the repository:
   - `google-services.json`
   - `local.properties`
   - `signing.properties`
   - `BuildVars.java`
   - Your keystore file

### **Step 7: Setup the Workflow to Clone Your Private Repository**

1. Create a new [personal access token](https://github.com/settings/tokens/new) with the `repo` scope. Refer to the instructions provided [here](https://github.com/GuillaumeFalourd/clone-github-repo-action#for-a-private-repository) for detailed guidance.

2. Save the generated token.

3. In your forked repository of this project:
   - Go to the repository's settings.
   - Click on "Secrets and variables."
   - Add a new repository secret named `ACCESS_TOKEN` and provide the token you generated as the value.

4. Modify the workflow to clone your private repository:
   - Access the workflow configuration file at [this link](https://github.com/Lyceris-chan/OctoGramBuilder/blob/main/.github/workflows/build-release.yml#L38C1-L39C47).
   - Change the values of `owner` and `repository` to your GitHub account name (e.g., `Lyceris-chan` for me) and the name of your private repository (e.g., `OctoGramBuilderSecrets` for me).
  
### **Step 8: Setup the GITHUB_TOKEN permissions**
1. In your forked repository of this project:
   - Go to the repository's settings.
   - Select the Actions section.
   - Choose the General option.
   - Scroll to the bottom and adjust the 'Workflow permissions' to Read and Write permissions. This enables the workflow to autonomously generate and upload its release upon completion.

### **Step 9: Setup Telegram status notifications**

This project uses the appleboy/telegram-action action to send build and status notifications to the defined TG_CHAT_ID secret using the TG_TOKEN secret for authentication.

> If you don't use Telegram or don't want to receive notifications for build and status updates, then edit the build-release.yml to remove the steps including Telegram.

#### **Obtain your chat id:**
You can obtain the Telegram chat ID by sending a message in a chat and forwarding the message to [@ShowJsonBot](https://t.me/ShowJsonBot). The chat ID is shown under `forward_from_chat`.

#### **Obtain your bot token:**
In this context, a token is a string that authenticates your bot (not your account) on the bot API. Each bot has a unique token which can also be revoked at any time via [@BotFather](https://t.me/botfather).

Obtaining a token is as simple as contacting [@BotFather](https://t.me/botfather), issuing the `/newbot` command, and following the steps until you're given a new token. You can find a step-by-step guide [here](https://core.telegram.org/bots/features#creating-a-new-bot).

Your token will look something like this:
`4839574812:AAFD39kkdpWt3ywyRZergyOLMaJhac60qc`

**In your forked repository of this project:**
   - Go to the repository's settings.
   - Click on "Secrets and variables."
   - Add a new repository secret named `TG_CHAT_ID` and provide the chat ID you obtained from [@ShowJsonBot](https://t.me/ShowJsonBot) earlier.
   - Add a new repository secret named `TG_TOKEN` and provide the bot token you obtained from [@BotFather](https://t.me/botfather) earlier.

***Make sure to save your token in a secure place, treat it like a password, and don't share it with anyone.***

### **Step 10: Setup cache download url**
1. Open the `build-release.yml` file in your forked repository.
2. Locate the following line:
- `url="https://github.com/Lyceris-chan/OctoGramBuilder/releases/download/BuildCache/cache.tzst"`
3. Replace the URL with the URL of your own 💾 OctoGram build cache release that contains the `cache.tzst` file generated **AFTER** the first successful build.

### **Step 11: Run the GitHub Actions Workflow**

1. Navigate to the "Actions" tab in your repository.

2. Select "Build and Release OctoGram."

3. Click on "Run Workflow," then click "Run Workflow" again (the second button is typically green).

4. The initial workflow run may take around an hour or more. Upon completion, it will create two releases:
- 🐙 OctoGram build: Contains the built APKs.
- 💾 OctoGram build cache: Holds cached data from the build, speeding up future runs to under 30 minutes.