# CSV files protections

## Parts
- [CSV compression](#csv-compression)
- [Server verification](#server-verification)
  - [Basic knowledges](#basic-knowledges)
  - [Bypassing security](#bypassing-security)

## CSV compression

So, CSV files are **compressed** for some Supercell's games (not brawl stars) to prevent modifications of the game.  
You can **decompress** them using [this tool](https://github.com/xcoder-tool/XCoder).

## Server verification

### Basic knowledges
For games who have compressed CSV files, it's a bit harder to modify them because of **a security that check the content of every CSV files using a signature or something like that when connecting to server**.  

If your csv files are not "sync" with files of the server, you just get the **"updating content" screen**, then **your game reboot** and **your modified CSV file is replaced by the original compressed file**.  

Last thing, for the server, an **original decompressed CSV file** and an **original compressed CSV file** are treated the same way. As long as the file hasn’t been modified, everything is fine. **They don’t pay attention to whether the file is compressed or decompressed**; they only check **if the file has been altered**.

### Bypassing security
Also, the thing that we exploit to bypass that is pretty easy here : **they don't care about texts files**.  

Here the **structure of game folders** (for clash of clans iOS example) :
``` text
updated/
│
├── csv/*.csv //csv files, parameters, data, etc
│
├── logic/*.csv //csv files, parameters, data, etc
│
└── localization/*.csv //csv files, texts files
  ...
```

We now that : **if we modify any data in `csv/*.csv` or in `logic/*.csv`, the server detect it**.  

**BUT the server don't care about `localization/*.csv`, simply because it's just text, it's maybe not important for them to verify that**. The logic start from here

I already now that : **it's not just files in `localization` that are skipped by the verification, but the ENTIRE FOLDER**  

**Strategy** :  
Inject our modded csv file in `localization` with a basic new name (`a.csv` for example) :
``` text
updated/
│
└── localization
    │
    └── a.csv //our modified csv file
```

And then the last step is to **link our new CSV file in the executable**, for example, if `localization/a.csv` was `csv/billing_packages.csv` we will **replace occurences in the executable** to make that `csv/billing_packages.csv` is now `localization/a.csv`.  

**Final result** : we modded the file `csv/billing_packages.csv` using `localization/a.csv`, and the **server will not verify the modified content** of `localization/a.csv` because the file is in `localization`.