## Table of Contents

- [GnuPG (gpg) Cheat Sheet](#gnupg\(gpg)\cheat\sheet)
  - [Key Management](#Key\Management)
    - [Generate a New Key Pair](#Generate\a\New\Key\Pair)
    - [List Keys](#List\Keys)
    - [Export a Public Key](#Export\a\Public\Key)
    - [Import a Public Key](#Import\a\Public\Key)
    - [Export a Private Key](#Export\a\Private\Key)
    - [Delete a Public Key](#Delete\a\Public\Key)
    - [Delete a Private Key](#Delete\a\Private\Key)
  - [Encryption and Decryption](#Encryption\and\Decryption)
    - [Encrypt a File](#Encrypt\a\File)
    - [Decrypt a File](#Decrypt\a\File)
    - [Symmetric Encryption](#Symmetric\Encryption)
    - [Decrypt Symmetrically Encrypted File](#Decrypt\Symmetrically\Encrypted\File)
  - [Signing and Verification](#Signing\and\Verification)
    - [Sign a File](#Sign\a\File)
    - [Verify a Signed File](#Verify\a\Signed\File)
    - [Create a Detached Signature](#Create\a\Detached\Signature)
    - [Verify a Detached Signature](#Verify\a\Detached\Signature)
  - [Advanced Usage](#Advanced\Usage)
    - [Edit a Key](#Edit\a\Key)
    - [List Secret Keys](#List\Secret\Keys)
    - [Add a Subkey](#Add\a\Subkey)
    - [Create a Revocation Certificate](#Create\a\Revocation\Certificate)
    - [Change a Passphrase](#Change\a\Passphrase)
    - [Backup and Restore Keys](#Backup\and\Restore\Keys)
    - [Using GPG with an Agent](#Using\GPG\with\an\Agent)
  - [Advanced Key Management](#Advanced\Key\Management)
    - [Refresh Public Keys from a Keyserver](#Refresh\Public\Keys\from\a\Keyserver)
    - [Send a Key to a Keyserver](#Send\a\Key\to\a\Keyserver)
    - [Receive a Key from a Keyserver](#Receive\a\Key\from\a\Keyserver)
    - [Set Trust Level for a Key](#Set\Trust\Level\for\a\Key)
    - [Add a Photo ID to Your Key](#Add\a\Photo\ID\to\Your\Key)
    - [Export Owner Trust Values](#Export\Owner\Trust\Values)
    - [Import Owner Trust Values](#Import\Owner\Trust\Values)
  - [Encryption and Decryption in Depth](#Encryption\and\Decryption\in\Depth)
    - [Encrypt for Multiple Recipients](#Encrypt\for\Multiple\Recipients)
    - [Encrypt with ASCII Armor](#Encrypt\with\ASCII\Armor)
    - [Decrypt to Standard Output](#Decrypt\to\Standard\Output)
  - [Batch Processing and Scripting](#Batch\Processing\and\Scripting)
    - [Batch Key Generation](#Batch\Key\Generation)
    - [Unattended Encryption/Decryption](#Unattended\Encryption/Decryption)
  - [Handling Passphrases](#Handling\Passphrases)
    - [Passphrase File for Batch Processing](#Passphrase\File\for\Batch\Processing)
    - [Cache Passphrase with gpg-agent](#Cache\Passphrase\with\gpg-agent)
  - [Advanced Signing](#Advanced\Signing)
    - [Clearsign a Document](#Clearsign\a\Document)
    - [Detached Signature with Timestamp](#Detached\Signature\with\Timestamp)
    - [Sign and Encrypt in One Step](#Sign\and\Encrypt\in\One\Step)
  - [Miscellaneous Advanced Commands](#Miscellaneous\Advanced\Commands)
    - [Print GPG Configuration](#Print\GPG\Configuration)
    - [Verify without Importing Key](#Verify\without\Importing\Key)
    - [Use an Alternate Configuration File](#Use\an\Alternate\Configuration\File)
    - [List GPG Components and Versions](#List\GPG\Components\and\Versions)


# GnuPG (gpg) Cheat Sheet

## Key Management

### Generate a New Key Pair
- Creates a new public/private key pair.
  ```bash
  gpg --gen-key
  ```

### List Keys
- Lists all keys in your public keyring.
  ```bash
  gpg --list-keys
  ```

### Export a Public Key
- Exports a public key to a file.
  ```bash
  gpg --export -a "User Name" > public.key
  ```

### Import a Public Key
- Imports a public key from a file.
  ```bash
  gpg --import public.key
  ```

### Export a Private Key
- Exports your private key.
  ```bash
  gpg --export-secret-keys -a "User Name" > private.key
  ```

### Delete a Public Key
- Removes a public key from your keyring.
  ```bash
  gpg --delete-key "User Name"
  ```

### Delete a Private Key
- Removes a private key from your keyring.
  ```bash
  gpg --delete-secret-key "User Name"
  ```

## Encryption and Decryption

### Encrypt a File
- Encrypts a file for a specific recipient.
  ```bash
  gpg --encrypt --recipient "User Name" file.txt
  ```

### Decrypt a File
- Decrypts a file.
  ```bash
  gpg --decrypt file.txt.gpg
  ```

### Symmetric Encryption
- Encrypts a file using a passphrase.
  ```bash
  gpg --symmetric file.txt
  ```

### Decrypt Symmetrically Encrypted File
- Decrypts a file encrypted with symmetric encryption.
  ```bash
  gpg --decrypt file.txt.gpg
  ```

## Signing and Verification

### Sign a File
- Digitally signs a file.
  ```bash
  gpg --sign file.txt
  ```

### Verify a Signed File
- Verifies a signed file.
  ```bash
  gpg --verify file.txt.gpg
  ```

### Create a Detached Signature
- Creates a detached signature for a file.
  ```bash
  gpg --detach-sign file.txt
  ```

### Verify a Detached Signature
- Verifies a detached signature.
  ```bash
  gpg --verify file.txt.sig file.txt
  ```

## Advanced Usage

### Edit a Key
- Accesses the key editing menu to manage key trust, add subkeys, etc.
  ```bash
  gpg --edit-key "User Name"
  ```

### List Secret Keys
- Lists all your secret keys.
  ```bash
  gpg --list-secret-keys
  ```

### Add a Subkey
- Adds a subkey to your keyring.
  ```bash
  gpg --edit-key "User Name" addkey
  ```

### Create a Revocation Certificate
- Generates a revocation certificate for a key.
  ```bash
  gpg --gen-revoke "User Name"
  ```

### Change a Passphrase
- Changes the passphrase for your private key.
  ```bash
  gpg --edit-key "User Name" passwd
  ```

### Backup and Restore Keys
- Backup:
  ```bash
  gpg --export-secret-keys "User Name" > my-private-backup.gpg
  ```
- Restore:
  ```bash
  gpg --import my-private-backup.gpg
  ```

### Using GPG with an Agent
- Configures GPG to use `gpg-agent` for key management and passphrase caching.
  ```bash
  gpg-agent --daemon
  gpg --use-agent
  ```

## Advanced Key Management

### Refresh Public Keys from a Keyserver
- Updates your public keys with the latest versions from a keyserver.
  ```bash
  gpg --refresh-keys
  ```

### Send a Key to a Keyserver
- Publishes your public key to a keyserver.
  ```bash
  gpg --send-keys --keyserver [keyserver address] [keyID]
  ```

### Receive a Key from a Keyserver
- Fetches a public key from a keyserver using the key ID.
  ```bash
  gpg --recv-keys --keyserver [keyserver address] [keyID]
  ```

### Set Trust Level for a Key
- Manually sets the trust level of a public key.
  ```bash
  gpg --edit-key [keyID] trust
  ```

### Add a Photo ID to Your Key
- Attaches a photo ID to your GnuPG key.
  ```bash
  gpg --edit-key [keyID] addphoto
  ```

### Export Owner Trust Values
- Exports the owner trust values of your keys.
  ```bash
  gpg --export-ownertrust > ownertrust.txt
  ```

### Import Owner Trust Values
- Imports owner trust values from a file.
  ```bash
  gpg --import-ownertrust ownertrust.txt
  ```

## Encryption and Decryption in Depth

### Encrypt for Multiple Recipients
- Encrypts a file for multiple recipients.
  ```bash
  gpg --encrypt --recipient [User Name 1] --recipient [User Name 2] file.txt
  ```

### Encrypt with ASCII Armor
- Encrypts data in ASCII format, useful for text-based communication.
  ```bash
  gpg --armor --encrypt --recipient "User Name" file.txt
  ```

### Decrypt to Standard Output
- Decrypts a file and outputs the content to standard output.
  ```bash
  gpg --decrypt --output - file.txt.gpg
  ```

## Batch Processing and Scripting

### Batch Key Generation
- Generates a key pair without interactive prompts (useful for scripting).
  ```bash
  gpg --batch --gen-key key-script.txt
  ```

### Unattended Encryption/Decryption
- Encrypts/decrypts in batch mode, allowing for scripting without interactive prompts.
  - **Encryption:**
    ```bash
    gpg --batch --trust-model always --encrypt --recipient "User Name" file.txt
    ```
  - **Decryption:**
    ```bash
    gpg --batch --decrypt file.txt.gpg
    ```

## Handling Passphrases

### Passphrase File for Batch Processing
- Uses a passphrase from a file for batch operations.
  ```bash
  gpg --batch --passphrase-file mypassphrase.txt --decrypt file.txt.gpg
  ```

### Cache Passphrase with gpg-agent
- Caches the passphrase using `gpg-agent` to avoid repeated prompts.
  ```bash
  gpg-agent --daemon
  gpg --use-agent --decrypt file.txt.gpg
  ```

## Advanced Signing

### Clearsign a Document
- Creates a clearsigned document, useful for signing text files like emails.
  ```bash
  gpg --clearsign document.txt
  ```

### Detached Signature with Timestamp
- Creates a detached signature with a timestamp.
  ```bash
  gpg --detach-sign --timestamp document.txt
  ```

### Sign and Encrypt in One Step
- Digitally signs and then encrypts a document.
  ```bash
  gpg --sign --encrypt --recipient "User Name" document.txt
  ```

## Miscellaneous Advanced Commands

### Print GPG Configuration
- Prints the GnuPG configuration file.
  ```bash
  gpg --version
  ```

### Verify without Importing Key
- Verifies a signature without importing the signer's key to your keyring.
  ```bash
  gpg --verify --no-default-keyring --keyring /dev/null document.txt.sig
  ```

### Use an Alternate Configuration File
- Specifies an alternate `gpg.conf` file.
  ```bash
  gpg --options /path/to/alternate/gpg.conf
  ```

### List GPG Components and Versions
- Lists GnuPG components and their versions.
  ```bash
  gpg --version
  ```
