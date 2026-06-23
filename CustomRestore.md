# Creating a Custom Restore File

Currently, [HeliBoard](https://github.com/HeliBorg/HeliBoard) has no import or export capability for installing custom layouts.  As the keyboard is still in development, I don't expect a programmer's attention to be turned in that direction anytime soon.

In the meantime, try this technique as a workaround.  This method, as far as I know, has yet to be tested on a large scale.  I've tested it myself in a limited way, but there is a lot I may have missed.  Please report any major issues you may find.  I decided to create the instructions here instead of putting them on HeliBoard's Wiki because it needs testing first and the procedure may not work after HeliBoard updates.

There are two main benefits for creating custom restore files:
- Custom layouts are easier to share without compromising dictionaries, and other personal data (clipboard entries, gestures, etc.).
- It makes switching layouts much less painful.

<br>

## Suppositions

I have not examined HeliBoard's source code and I do not plan to do so, but through testing, I have reached the following suppositions:
- A HeliBoard backup/restore file follows this naming convention: ```HeliBoard_backup_yyyy-mm-dd.zip```, where ```yyyy``` is the year, ```mm``` is the numeric month, and ```dd``` is the numeric day of the month.
- A HeliBoard backup/restore file has the following structure:
  - HeliBoard_backup_yyy-mm-dd/
    - blacklists
    - unprotected/
      - layouts/
        - _custom_layouts/_
          - _layout_JSON_files_
    - UserHistoryDictionary._[language]_.dict/
      - _dictionary_files_
    - heliboard.db
    - preferences.json
    - protected_preferences.json
    
There may be more files and directories in the backup structure, but we are only interested in ```unprotected/layouts/...```, ```preferences.json```, and possibly ```protected_preferences.json```.

From my testing, ```UserHistoryDictionary.[language].dict/``` and ```heliboard.db``` are ignored if they are not in a restore file and, the current data is left intact.

I am not sure about the ```protected_preferences.json``` file because I have not seen it used in any of my testing.  Therefore, I would only include it in the custom restore file if it actually contains non-persistent data.

<br>

## Creating the Custom Restore File

Based on the suppositions above, to create a sharable and or switchable (for switching between your different custom layouts) custom restore file while leaving your dictionaries and other data intact, follow these steps:

1. Setup your custom HeliBoard the way you want it, including settings and layouts.
2. Backup HeliBoard.
3. Unzip a copy of your HeliBoard backup into an empty working directory (always keep an uncustomized backup).
4. Delete the ```UserHistoryDictionary.[language].dict/``` directory and the ```heliboard.db``` file.
   - If ```protected_preferences.json``` contains anything besides:
    ```
        boolean settings
        {}
        int settings
        {}
        long settings
        {}
        float settings
        {}
        string settings
        {}
        string set settings
        {}
    ```  

      then decide if you need it or not.  It appears that if an entry exist in the custom restore file, it will replace the current item when restored, so if you include the blank ```protected_preferences.json``` file, it could overwrite something important.  You should be safe deleting it.

      This will leave you with the following (root) directory structure:
	- ```unprotected/...```
	- ```preferences.json```
	- and _possibly_ ```protected_preferences.json```
5. In the various ```.../layouts/...``` folders, you will find your custom layout files (one for each custom layout of that type, e.g. ```main``` or ```number_row```).  If you plan on sharing your custom layout(s), GREP or look through these files and remove any compromising information (they are JSON files without the extension), like your email addresses, home address, etc., that you do not want to become public knowledge.  I recommend you put placeholders to make it easier for non-technical users to easily enter their own information.
6. Now, zip your directory and give it any name you want.  Remember, the starting ```HeliBoard_backup_yyyy-mm-dd``` directory is not part of the compressed data, but only the name of the HeliBoard backup/restore file.  Make sure your compressed root directory is the one containing:
	- ```unprotected/...```
	- ```preferences.json```
	- and _possibly_ ```protected_preferences.json```
7. Your custom restore file is now ready for distribution and use.