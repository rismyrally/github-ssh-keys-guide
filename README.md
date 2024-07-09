# Managing Multiple SSH Keys for GitHub on macOS

This guide will help you configure your macOS to use multiple SSH keys for different GitHub accounts, such as personal and work accounts. Follow the steps below to ensure that each GitHub account uses its own SSH key and loads automatically.

## Step 1: Generate SSH Keys

### 1.1 Generate SSH Key for Personal GitHub Account

Open Terminal and run the following command:

```sh
ssh-keygen -t ed25519 -C "your_personal_email@example.com" -f ~/.ssh/id_ed25519_personal
```

### 1.2 Generate SSH Key for Work GitHub Account

Run the following command:

```sh
ssh-keygen -t ed25519 -C "your_work_email@example.com" -f ~/.ssh/id_ed25519_work
```

## Step 2: Start SSH Agent

Start the ssh-agent in the background:

```sh
eval "$(ssh-agent -s)"
```

## Step 3: Configure SSH Config File

1. Open or create the `~/.ssh/config` file:
   ```sh
   vim ~/.ssh/config
   ```
2. Add the following configurations to the file:

   ```sh
   # Personal GitHub account
   Host github.com-personal
     HostName github.com
     User git
     AddKeysToAgent yes
     IdentityFile ~/.ssh/id_ed25519_personal

   # Work GitHub account
   Host github.com-work
     HostName github.com
     User git
     AddKeysToAgent yes
     IdentityFile ~/.ssh/id_ed25519_work
   ```

3. Save the file and exit (`Esc`, then `:wq`, and `Enter`).

## Step 4: Add SSH Keys to the SSH Agent

### 4.1 Add Personal SSH Key to SSH Agent

```sh
ssh-add ~/.ssh/id_ed25519_personal
```

### 4.2 Add Work SSH Key to SSH Agent

```sh
ssh-add ~/.ssh/id_ed25519_work
```

**Note**: If you continue to be prompted for your SSH passphrase, you may need to add the following commands to your `~/.zshrc` to ensure your SSH keys are loaded automatically:

```sh
# Load personal SSH key
ssh-add -K ~/.ssh/id_ed25519_personal

# Load work SSH key
ssh-add -K ~/.ssh/id_ed25519_work
```

## Step 5: Add SSH Keys to GitHub

### 5.1 Add Personal SSH Key to GitHub

Copy the personal SSH public key to your clipboard:

```sh
cat ~/.ssh/id_ed25519_personal.pub | pbcopy
```

1. Log in to your personal GitHub account.
2. Go to **Settings > SSH and GPG keys**.
3. Click **New SSH key**.
4. Paste the key and give it a title, then click **Add SSH key**.

### 5.2 Add Work SSH Key to GitHub

Copy the work SSH public key to your clipboard:

```sh
cat ~/.ssh/id_ed25519_work.pub | pbcopy
```

1. Log in to your work GitHub account.
2. Go to **Settings > SSH and GPG keys**.
3. Click **New SSH key**.
4. Paste the key and give it a title, then click **Add SSH key**.

## Step 6: Verify SSH Connections

### 6.1 Test Personal GitHub SSH Connection

Run the following command to test the connection:

```sh
ssh -T git@github.com-personal
```

You should see a success message from GitHub.

### 6.2 Test Work GitHub SSH Connection

Run the following command to test the connection:

```sh
ssh -T git@github.com-work
```

You should see a success message from GitHub.

## Step 7: Modify Git Remote URLs

### 7.1 Update Remote URL for Personal Repositories

Navigate to your personal repository and update the remote URL:

```sh
cd path/to/your/personal/repo
git remote set-url origin git@github.com-personal:username/repo.git
```

### 7.2 Update Remote URL for Work Repositories

Navigate to your work repository and update the remote URL:

```sh
cd path/to/your/work/repo
git remote set-url origin git@github.com-work:username/repo.git
```

### 7.3 Set Remote URL for New Repositories

If setting up a new repository, use the following command:

```sh
git remote add origin git@github.com-personal:username/repo.git
```

## Optional Step: Configure Git User Details

### 8.1 Set User Email and Name for Personal Repositories

Navigate to your personal repository and set the user email and name:

```sh
cd path/to/your/personal/repo
git config --local user.email "personal@example.com"
git config --local user.name "Personal Name"
```

### 8.2 Set User Email and Name for Work Repositories

Navigate to your work repository and set the user email and name:

```sh
cd path/to/your/work/repo
git config --local user.email "work@example.com"
git config --local user.name "Work Name"
```

By following these steps, you will have successfully configured your macOS to use multiple SSH keys for different GitHub accounts and ensured they load automatically.
