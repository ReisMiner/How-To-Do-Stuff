# [Root] Get Steam Guard TOTP Secret

You finally bought Bitwarden Premium and think: Wow great, now I have all my Passwords and TOTPs in the same App.
But then you notice that you don't have the Secret for the Steam TOTP. Luckily this Guide is here to help you!

## Prerequisites

- **Rooted** Android Phone
- Steam App installed with a Steam Guard already set up.
- Optional: PC with a working ADB Driver and Root USB Debugging enabled.

## Steps

> ℹ️ If you do it without a PC download the Termux App and skip to step 5.

- Connect the Phone to your PC
- Open a Terminal of your choice
- Enter `adb root` to start ADB with Root access.
  - will give an error if you don't have `Rooted Debugging` enabled on the Phone.
- After the ADB Daemon restarted with Root Privileges enter `adb shell`. This opens a shell from your Phone, much like SSH.
- Then enter `cd /data/data/com.valvesoftware.android.steam.community/files/`. This changes the directory to the location of the Steam Guard Files.
- Now there should be a file called like `Steamguard-xxxxxxxxxxxxxxxxx` whereas the x stand for your account's steamID64.
  - You can also enter command `ls` to list all files in the Directory, just to be sure if you have multiple accounts!
- Since we know the file, we can just print its content to the Terminal by using this command `cat Steamguard-xxxxxxxxxxxxxxxxx`. The output looks like JSON.
  - replace the x with your actual file of course.
- Search the `uri` attribute and take a look at the `secret` URI Parameter. This is your TOTP Secret. Be careful with it!
  - URI Parameter looks like this: `  "uri": "otpauth://totp/Steam:STEAM_ACCOUNT?secret=TOTP_SECRET_HERE&issuer=Steam"`
- To import the Secret into Bitwarden, you need to prefix the Secret with `steam://` so Bitwarden know that it's a steam TOTP, because those are a bit different.
- PC ONLY: After you've completed all this and imported the secret into Bitwarden, make sure to disable `Rooted Debugging` again because it might put your device at risk.

> ℹ️ Of course this Key does also work with all other Apps Like Authy and Yubikey Authenticator. For the Yubikey Authenticator GUI make sure that the issuer is labeled as Steam!