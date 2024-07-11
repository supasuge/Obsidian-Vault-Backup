## Table of Contents

    - [`FileCrypto`](#`FileCrypto`)
    - [`KeyManager`](#`KeyManager`)



```Python
class FileCrypto:
    '''
	GUI - Houses Primary GUI functionality

	functions:
	
	'''
```


### `FileCrypto`

The `FileCrypto` class provides a Tkinter GUI interface for encrypting and decrypting files and managing RSA keys. The encryption and decryption of files are based on the AES (Advanced Encryption Standard) algorithm, and the secure storage of passwords is managed using Argon2 and Python's `keyring` library.

- **Attributes**:  
  - `root`: Tkinter root window  
  - `json_file`: File that stores encrypted passwords  
  - `key_manager`: Instance of the `KeyManager` class

- **Methods**:  
  - `__init__`: Initializes the Tkinter GUI and other class attributes  
  - `init_gui`: Sets up the Tkinter GUI components  
  - `check_for_directory`: Checks and creates a required directory  
  - `get_or_confirm_master_password`: Gets or confirms the master password from the user  
  - `delete_master_password`: Deletes the master password  
  - `update_status`: Updates the status label in the GUI  
  - `read_json`: Reads the JSON file containing encrypted passwords  
  - `write_json`: Writes encrypted passwords to the JSON file  
  - `encrypt_json`: Encrypts a JSON string  
  - `decrypt_json`: Decrypts an encrypted JSON string  
  - `encrypt_file`: Encrypts a selected file  
  - `decrypt_file`: Decrypts a selected file

**Examples**:  
- Instantiate the class: `file_crypto = FileCrypto()`  
- Initialize the GUI: `file_crypto.init_gui()`



### `KeyManager`

The `KeyManager` class is responsible for managing RSA keys. It provides a Tkinter-based UI within a frame to generate, save, and decrypt RSA key pairs.

- **Attributes**:  
  - `root`: The `FileCrypto` class instance  
  - `frame`: The Tkinter frame where the GUI widgets reside

- **Methods**:  
  - `__init__`: Initializes the class and its attributes  
  - `init_gui`: Sets up the initial GUI elements  
  - `clear_and_show_options`: Clears the frame and shows options for generating keys  
  - `validate_and_generate_keys`: Validates inputs and generates RSA keys  
  - `save_keys`: Saves the generated RSA keys to files  
  - `decrypt_private_key`: Decrypts the stored RSA private key
  
**Examples**:  
- Initialize the `KeyManager` within `FileCrypto`: `self.key_manager = KeyManager(self, key_manager_frame)`  
- Generate RSA keys: `key_manager.validate_and_generate_keys()`  
- Save RSA keys: `key_manager.save_keys(key_name, private_key, public_key, password)`  
- Decrypt RSA private key: `key_manager.decrypt_private_key(key_name)`
