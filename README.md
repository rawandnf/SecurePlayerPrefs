SecurePlayerPrefs
=================
_Copyright (c) 2014 Rawand Fatih_, 
_Licensed under MIT License (MIT)_

Securely save player preferences in [Unity3d](http://unity3d.com/) projects.

You can use this class to save player preferences in your [Unity3d](http://unity3d.com/) project. When saving a preference,
it will be encrypted using DES algorithm. The key that's used for the encryption is different
in each device since we generate a random number and add it to the key in initialization for each device.

This also uses [Unity3d](http://unity3d.com/)'s native [PlayerPrefs](http://docs.unity3d.com/ScriptReference/PlayerPrefs.html) classes
for saving the values, but encrypts them before hand.

If you are already familiar with [Unity3d](http://unity3d.com/)'s [PlayerPrefs](http://docs.unity3d.com/ScriptReference/PlayerPrefs.html),
with just a little bit of change you can go ahead and use this one!

See [PlayerPrefs](http://docs.unity3d.com/ScriptReference/PlayerPrefs.html) Docs of Unity3d if you are not familiar with it.


## Contents
- [How To Use](#how-to-use)
  - [Usage Example](#usage-example)
  - [Usage](#usage)
  - [Setup](#setup)
  - [Moving from PlayerPrefs to SecurePlayerPrefs](#moving-from-playerprefs-to-secureplayerprefs)
- [Methods](#methods)
  - [Init()](#init)
  - [isInitialized()](#isinitialized)
  - [setLogErrorsEnabled(bool state)](#setlogerrorsenabledbool-state)
  - [SetString(string key, string val)](#setstringstring-key-string-val)
  - [SetFloat(string key, float val)](#setfloatstring-key-float-val)
  - [SetInt(string key, int val)](#setintstring-key-int-val)
  - [SetBool(string key, bool val)](#setboolstring-key-bool-val)
  - [GetString(String key, String defaultValue = "")](#getstringstring-key-string-defaultvalue--)
  - [GetInt(String key, int defaultValue = 0)](#getintstring-key-int-defaultvalue--0)
  - [GetFloat(String key, float defaultValue)](#getfloatstring-key-float-defaultvalue)
  - [GetBool(string key, bool defaultValue = false)](#getboolstring-key-bool-defaultvalue--false)
  - [DeleteKey(string key)](#deletekeystring-key)
  - [DeleteAll()](#deleteall)
  - [HasKey(string key)](#haskeystring-key)
  - [Save()](#save)
- [Contributing](#contributing)
- [Contributors](#contributors)
- [License](#license)

----------

## How To Use

### Usage Example
Before diving into details, i'll show you an example and hopefully you'll get what you need. But its also important to know how exactly everything works. Its simple as reading the document bellow ;).
```csharp
  // Initializing the SecurePlayerPrefs
  SecurePlayerPrefs.Init();
  
  // Setting a high score but encrypted ;)
  SecurePlayerPrefs.SetInt("HIGH_SCORE", 23);
  
  // Getting the highscore back.
  int highScore = SecurePlayerPrefs.GetInt("HIGH_SCORE");
  
  // Just setting a string.
  SecurePlayerPrefs.SetString("A key name", "This is so simple");
```


### Usage
Somewhere before you ever use SecurePlayerPrefs, initialize the keys.
For this you only need to run `SecurePlayerPrefs.Init()` in an `Awake` or `Start` function in
your project or scene.
```csharp
void Start () {
	SecurePlayerPrefs.Init();
}
```
After this, SecurePlayerPrefs is ready to use.

### Setup
Download the [SecurePlayerPrefs](https://github.com/rawandnf/SecurePlayerPrefs/blob/master/SecurePlayerPrefs.cs) 
source code and copy it into your project.

Open the code in a editor, and let's modify the your encryption key as following:
- Your DES key is 8 character key of letters, digits, or symbols.
- Your key is devided into three parts:
  - Prefix --> `PRIVATE_KEY_PREFIX`
  - Random (Three Characters) --> `PRIVATE_KEY_RAND`
  - Suffix --> `PRIVATE_KEY_SUFFIX`
- Contactinating all three should end up making a 8 letter key. Keep in mind the random part is 3 letters.
- Change those three to what ever you like, see the example:

```csharp
private static readonly string PRIVATE_KEY_PREFIX = "abcd";
private static readonly string PRIVATE_KEY_RAND = "efg";
private static readonly string PRIVATE_KEY_SUFFIX = "h";

// Or another example.

private static readonly string PRIVATE_KEY_PREFIX = "ab";
private static readonly string PRIVATE_KEY_RAND = "cde";
private static readonly string PRIVATE_KEY_SUFFIX = "fgh";
```

- We said the random part changes between devices, so each device generates its own random key and saves it in the PlayerPrefs securly encrypted with the key you created up there.
- So now, change the key name that the random key of the device is saved in the PlayerPrefs. Example:
```csharp
private static readonly string RAND_KEY = "abcdefgh";
```

GOOD! Everything is ready now!

### Moving from PlayerPrefs to SecurePlayerPrefs
If you already have a game with player preferences saved and you don't want to lose them, it's fine.
You can copy your existing player preferences for the first initialization in a device.

Simply you create an array and call a function we've already provided and the rest is done for you!
You add this array and method call under [Line 62](https://github.com/rawandnf/SecurePlayerPrefs/blob/master/SecurePlayerPrefs.cs#L62) of the code where the comment says add your copying code here.
Learn from the example:
```csharp
// Each index has the keyname, a new key name, and the player preference type.
string[,] keyandnewkeys = new string[5, 3] {
  {"A_KEY_I_USED", "A_NEW_NAME_FOR_THE_KEY", "int"},
  {"ANOTHER_KEY", "ANOTHER_KEY", "int"},
  {"HIGHT_SCORE", "fssdes23f", "int"},
  {"A_FLOAT_KEY", "A_NEW_FLOAT_KEY", "float"},
  {"FINAL_KEY", "STRRRRR", "string"}
};

/*
 * The first param is the array of keys, new key names, and the pref type.
 * the second is to indicate if the old keys should be removed from the prefs.
 */
securePlayerPrefs(keyandnewkeys, true);
```

------------

## Methods
This section we define each function.

### Init()
_public static void Init()_


This initializes the keys to be ready to use. If the project is runnin on a device for the first time,
it will generate a random 3 digits to add to the key.

You should call this function before using SecurePlayerPrefs anywhere, otherwise it fails.

### isInitialized()
_public static bool isInitialized()_


Returns the state of the SecurePlayerPrefs, which tells if its initialized.


### setLogErrorsEnabled(bool state)
_public static void setLogErrorsEnabled(bool state)_


Setting logErrorsEnabled to ture will print exception stack traces and other logs from the SecurePlayerPrefs.


### SetString(string key, string val)
_public static void SetString(string key, string val)_


Saves a string in player preferences but securly encrypted by a key identifier.

Parameters:
- `key` The preference key id.
- `val` The value to set.


### SetFloat(string key, float val)
_public static void SetFloat(string key, float val)_


Saves a float in player preferences but securly encrypted by a key identifier.

Parameters:
- `key` The preference key id.
- `val` The float value to set.



### SetInt(string key, int val)
_public static void SetInt(string key, int val)_


Saves an int in player preferences but securly encrypted by a key identifier.

Parameters:
- `key` The preference key id.
- `val` The int value to set.


### SetBool(string key, bool val)
_public static void SetBool(string key, bool val)_


Saves a boolean in player preferences but securly encrypted by a key identifier.

Parameters:
- `key` The preference key id.
- `val` The boolean value to set.


### GetString(String key, String defaultValue = "")
_public static string GetString(String key, String defaultValue = "")_


Returns the decrypted value corresponding to key in the preference file if it exists.
Returns default value `defaultValue` if:
- If the key doesn't exist.
- In case of any exceptions.

Parameters:
- `key` The id of the player preferences.
- `val` The default to return.

You can also leave the `defaultValue` parameter, and in this case an _empty string_ will be set as default.


### GetInt(String key, int defaultValue = 0)
_public static int GetInt(String key, int defaultValue = 0)_


Returns the decrypted value corresponding to key in the preference file if it exists.
Returns default value `defaultValue` if:
- If the key doesn't exist.
- In case of any exceptions.

Parameters:
- `key` The id of the player preferences.
- `val` The default to return.

You can also leave the `defaultValue` parameter, and in this case _0_ will be set as default.


### GetFloat(String key, float defaultValue)
_public static float GetFloat(String key, float defaultValue = 0.0f)_


Returns the decrypted value corresponding to key in the preference file if it exists.
Returns default value `defaultValue` if:
- If the key doesn't exist.
- In case of any exceptions.

Parameters:
- `key` The id of the player preferences.
- `val` The default to return.

You can also leave the `defaultValue` parameter, and in this case _0.0f_ will be set as default.



### GetBool(string key, bool defaultValue = false)
_public static bool GetBool(string key, bool defaultValue = false)_


Returns the decrypted value corresponding to key in the preference file if it exists.
Returns default value `defaultValue` if:
- If the key doesn't exist.
- In case of any exceptions.

Parameters:
- `key` The id of the player preferences.
- `val` The default to return.

You can also leave the `defaultValue` parameter, and in this case _false_ will be set as default.


### DeleteKey(string key)
_public static void DeleteKey(string key)_

Removes key and its corresponding value from the preferences.
(This is exactly same as [PlayerPrefs.DeleteKey](http://docs.unity3d.com/ScriptReference/PlayerPrefs.DeleteKey.html))

Parameters:
- `key` The id of the player preferences.


### DeleteAll()
_public static void DeleteAll()_

Removes all keys and values from the preferences.
(This is exactly same as [PlayerPrefs.DeleteAll](http://docs.unity3d.com/ScriptReference/PlayerPrefs.DeleteAll.html))

**_Use with caution._**


### HasKey(string key)
_public static bool HasKey(string key)_

Returns true if key exists in the preferences.
(This is exactly same as [PlayerPrefs.HasKey](http://docs.unity3d.com/ScriptReference/PlayerPrefs.HasKey.html))

Parameters:
- `key` The id of the player preferences.


### Save()
_public static void Save()_

Writes all modified preferences to disk.
(This is exactly same as [PlayerPrefs.Save](http://docs.unity3d.com/ScriptReference/PlayerPrefs.Save.html))

By default Unity writes preferences to disk on Application Quit.
In case when the game crashes or otherwise prematuraly exits,
you might want to write the PlayerPrefs at sensible 'checkpoints'
in your game. This function will write to disk potentially causing
a small hiccup, therefore it is not recommended to call during actual gameplay.

------------

## Contributing

Do the usual GitHub fork and pull request dance. Add yourself to the
contributors section of this file too if you want to.

## Contributors
- Rawand Fatih (rawandnf@gmail.com)

## License

The MIT License (MIT)

Copyright (c) 2014 Rawand

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

-------

[Back To Top](#secureplayerprefs)
