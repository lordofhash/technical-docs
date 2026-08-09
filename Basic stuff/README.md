### Setting Up and Troubleshooting SSH Keys on macOS for GitHub

A complete guide to generating an Ed25519 SSH key, linking it to GitHub, adding it to the macOS Keychain, and making the configuration permanent. 

### Step 1: Generate a New SSH Key

Open Terminal and run the following command. Replace the example email with your GitHub email address: 

bash

ssh-keygen -t ed25519 -C "your_email@example.com"

Use code with caution.

* **Save Location:** Press **Enter** to accept the default hidden location (~/.ssh/id_ed25519).
* **Passphrase:** Type a secure password and press **Enter**, then type it again to confirm. *(Highly recommended to protect your key)*.

### Step 2: Copy the Public Key to Clipboard

Run this command to copy your new public key directly to your Mac's clipboard: 

bash

pbcopy < ~/.ssh/id_ed25519.pub

Use code with caution.

### Step 3: Add the Key to GitHub

1. Log into **GitHub.com**.
2. Click your profile picture in the top-right corner and select **Settings**.
3. In the left sidebar, click **SSH and GPG keys**.
4. Click the green **New SSH key** button.
5. Give it a descriptive **Title** (e.g., "MacBook Air").
6. Leave the **Key type** as "Authentication Key".
7. Paste (Cmd + V) your key into the **Key** field.
8. Click **Add SSH key**.

### Step 4: Register the Key with macOS Keychain

To prevent "Permission denied (publickey)" errors and stop macOS from asking for your passphrase every time, register the key with your system: 

1. Start the SSH agent in the background: 

bash

eval "$(ssh-agent -s)"

Use code with caution.
2. Add your private key to the Apple Keychain: 

bash

ssh-add --apple-use-keychain ~/.ssh/id_ed25519

Use code with caution.

*(Enter your passphrase if prompted)*.

### Step 5: Make the Configuration Permanent

Configure your SSH settings so macOS automatically reloads this key every time your computer restarts. 

1. Open the SSH config file using the nano text editor: 

bash

nano ~/.ssh/config

Use code with caution.
2. Paste the following configuration block into the file: 

text

Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519

Use code with caution.
3. Save and exit the editor: 

  * Press Ctrl + O then **Enter** to save changes.
  * Press Ctrl + X to exit.

### Step 6: Test the Connection

Run this final command to verify everything is working: 

bash

ssh -T git@github.com

Use code with caution.

### Expected Success Output:

Hi lordofhash! You've successfully authenticated, but GitHub does not provide shell access.
