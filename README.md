# Verified GitHub Commits (GPG‑Verified)
Have you ever wanted your GitHub commits to carry the "Verified" badge? Verified commits prove that your work really comes from you, prevent easy identity spoofing, and add a professional touch to your profile and projects.

This repository explains what verified commits are, why they matter, and how to set them up using GPG.

<br>

## What Are Verified Commits?
Verified commits are Git commits that are cryptographically signed with a GPG (GNU Privacy Guard) key that you control. GitHub checks the signature and, if it matches a trusted key on your account, displays a "Verified" badge next to the commit.

This helps ensure that:

- The commit really came from you.
- The commit content has not been tampered with after it was signed.

<br>

## Why Use Verified Commits?
- **Authenticity**: Confirms to collaborators and users that the commits are genuinely yours.
- **Security**: Makes it much harder for someone to impersonate you in a repository.
- **Professionalism**: Shows that you care about the integrity and security of your open‑source work.

<br>

## The Problem With Unverified Commits
Without verification, Git only relies on the `user.name` and `user.email` in your local Git configuration. Anyone can set these to your details and produce commits that appear to be from you:

```bash
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

On GitHub, these commits will look like they came from your identity, even though you never made them. If you normally sign your commits, these spoofed commits will stand out as "Unverified", while your genuine commits will carry the "Verified" badge.

Using verified commits ensures that only commits signed with your GPG key appear as verified, which makes spoofing much easier to spot.

<br>

## How to Enable Verified Commits
The steps below walk you through installing GPG, generating a key, connecting it to GitHub, and configuring Git to sign your commits.

### 1. Install GPG

Install GPG on your system:

- **Windows**: Install [Gpg4win](https://www.gpg4win.org/) from the official website.
- **macOS** (with Homebrew):

```bash
brew install gnupg
```

- **Linux** (Debian/Ubuntu example):

```bash
sudo apt-get install gnupg
```

### 2. Check Existing GPG Keys

Before creating a new key, check if you already have one:

```bash
gpg --list-secret-keys --keyid-format LONG
```

If you already have a suitable key associated with your GitHub email, you can reuse it and skip directly to adding the key to GitHub.

### 3. Generate a New GPG Key

If you do not have a key yet, generate one:

```bash
gpg --full-generate-key
```

Recommended options:

- Key type: RSA and RSA (default)
- Key size: 4096 bits
- Validity: choose a period you are comfortable with, or make it non‑expiring
- Name and email: use the same email address you use on GitHub

When prompted, choose a strong passphrase. This protects your private key.

### 4. Find Your GPG Key ID

List your keys again to get the key ID:

```bash
gpg --list-secret-keys --keyid-format LONG
```

Example output:

```text
sec   4096R/ABC123456789DEF0 2024-01-01 [expires: 2025-01-01]
uid                  Your Name <your.email@example.com>
ssb   4096R/0987654321ABCDEF 2024-01-01
```

In this example, the key ID is `ABC123456789DEF0` (the part after `sec`).

### 5. Add Your GPG Key to GitHub

Export your public key in ASCII‑armored format:

```bash
gpg --armor --export ABC123456789DEF0
```

Copy the entire output.

Then, in GitHub:

1. Go to **Settings**.
2. Navigate to **SSH and GPG keys**.
3. Click **New GPG key**.
4. Paste the exported key, then save.

GitHub will now trust commits that are signed with this key.

### 6. Configure Git to Use Your GPG Key

Tell Git which GPG key to use for signing:

```bash
git config --global user.signingkey ABC123456789DEF0
```

To sign all commits by default:

```bash
git config --global commit.gpgSign true
```

You can still sign individual commits manually if you prefer:

```bash
git commit -S -m "Your commit message"
```

### 7. Verify Your Signed Commits

After everything is configured, make a test commit and push it to GitHub. In the commit history:

- Signed commits associated with your GitHub GPG key will show a "Verified" badge.
- Commits that only match your email but are not signed will be marked as unverified.

<br>

## Troubleshooting
- **Commit not verified on GitHub**  
  - Ensure the email on your GPG key matches an email on your GitHub account.  
  - Confirm that the correct public key is added under **SSH and GPG keys**.  
  - Check that `user.signingkey` and `commit.gpgSign` are correctly set in your Git config.

- **GPG repeatedly asks for your passphrase**  
  You can cache your passphrase with the GPG agent to avoid entering it for every commit:

  ```bash
  echo "use-agent" >> ~/.gnupg/gpg.conf
  echo "default-cache-ttl 28800" >> ~/.gnupg/gpg-agent.conf
  echo "max-cache-ttl 28800" >> ~/.gnupg/gpg-agent.conf

  gpgconf --kill gpg-agent
  gpgconf --launch gpg-agent
  ```

  This configuration keeps your passphrase cached for several hours.

<br>

## Conclusion
Setting up GPG‑verified commits is a straightforward way to improve the authenticity, security, and perceived quality of your work on GitHub. Once configured, your commits will clearly show that they were created and signed by you, giving collaborators and users additional confidence in your contributions.
