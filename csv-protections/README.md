# CSV files protections

## Parts
- [CSV compression](#csv-compression)
- [Server verification](#server-verification)
  - [Basic knowledge](#basic-knowledge)
  - [Bypassing security](#bypassing-security)

## CSV compression

In certain Supercell games (excluding Brawl Stars), CSV files are **compressed** to prevent unauthorized modifications to the game files. 
You can **decompress** these files using [this tool](https://github.com/xcoder-tool/XCoder).

## Server verification

### Basic knowledge
For games with **compressed** CSV files, modifying them is more challenging due to a **security feature** that verifies the content of each CSV file **when connecting to the server**.  

If your CSV files are **not in sync** with the server’s files, you will see an **"Updating content" screen**, then the game will **reboot**, and your modified CSV file will be **replaced by the original**, compressed version.  

It’s also worth noting that the server treats an **original decompressed CSV** file the same as an **original compressed one**. As long as the file **hasn’t been altered**, the server won’t flag it. It only checks **if the file’s content has been changed**, regardless of whether the file is **compressed or decompressed**.

### Bypassing security
To bypass this security, we can exploit a relatively simple oversight : **the server doesn’t check text files**. 

Below is the **structure of the game folders** (using Clash of Clans on iOS as an example) :
``` text
updated/
│
├── csv/*.csv          // CSV files with parameters, data, etc.
│
├── logic/*.csv        // CSV files with parameters, data, etc.
│
└── localization/*.csv // CSV files for text
  ...
```

We know that if we modify any data in `csv/*.csv` or `logic/*.csv`, **the server will detect it**.    

However, the server **doesn’t verify** files in the `localization/*.csv` folder because these files contain only text, which the server likely considers unimportant for security purposes probably. The key insight here is that the **entire localization folder is excluded from verification**.

**Strategy** :  
Place the modified CSV file in the `localization` folder under a new name (e.g., `a.csv`) :
``` text
updated/
│
└── localization
    │
    └── a.csv //Our modified CSV file
```

The final step is to **link** this new CSV file **in the executable**. For example, if `localization/a.csv` was previously `csv/billing_packages.csv`, we would **replace occurrences** in the executable so that `csv/billing_packages.csv` **now points to** `localization/a.csv`.  

**Final result** : We have successfully modified `csv/billing_packages.csv` by using `localization/a.csv`. Since `a.csv` is in the `localization` folder, the server will **not** verify its content.