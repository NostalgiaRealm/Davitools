# Davitools can extract Davilex formatted `.IMG` files with their `.IND` counterpart with `rcs-extract` and pack such files with `rcs-pack`.

# Extracting Davilex `.IMG` / `.IND` Files with Davitools

This README explains how to extract Davilex game archive files using the PHP-based **Davitools** project.

Davitools can extract `.IMG` archives when used together with their matching `.IND` index files.

The extractor command uses this format:

```bash
php rcs-extract.php <index.ind> <archive.img> <output-folder>
```

Important: the `.IND` file comes first, and the `.IMG` file comes second.

Correct:

```bash
php rcs-extract.php file.ind file.img output_folder
```

Incorrect:

```bash
php rcs-extract.php file.img file.ind output_folder
```

## Requirements

You need:

- A matching `.IMG` and `.IND` file pair from a Davilex game disc
- PHP CLI
- Git
- A terminal:
  - Linux Terminal on Linux
  - PowerShell or Command Prompt on Windows

Example file pair:

```text
RCS.IND
RCS.IMG
```

The `.IND` file is the index file.  
The `.IMG` file contains the actual archived data.

---

# Windows Instructions

These instructions assume you are using **PowerShell** or **Command Prompt**.

PowerShell is recommended.

## 1. Install PHP and Git on Windows

You need both:

- PHP for Windows
- Git for Windows

### Option A: Install Using Winget

Open PowerShell and run:

```powershell
winget install Git.Git
winget install PHP.PHP
```

After installation, close PowerShell and open it again.

Check that PHP is available:

```powershell
php --version
```

Check that Git is available:

```powershell
git --version
```

### Option B: Install Manually

If Winget is not available:

1. Install Git for Windows.
2. Install PHP for Windows.
3. Make sure both `git.exe` and `php.exe` are added to your Windows `PATH`.
4. Close and reopen PowerShell.

Then check:

```powershell
php --version
git --version
```

If Windows says that `php` is not recognized, PHP is not in your `PATH`.

## 2. Download Davitools on Windows

Open PowerShell and go to your Downloads folder:

```powershell
cd "$env:USERPROFILE\Downloads"
```

Clone the Davitools GitHub repository:

```powershell
git clone https://github.com/Gymnasiast/Davitools.git
```

Enter the Davitools folder:

```powershell
cd Davitools
```

## 3. Find the `.IMG` and `.IND` Files on Windows

If your Davilex game disc is mounted as drive `D:`, you can search it from PowerShell like this:

```powershell
Get-ChildItem -Path D:\ -Recurse -Include *.img,*.ind
```

If your disc uses another drive letter, replace `D:\` with the correct drive.

Example:

```powershell
Get-ChildItem -Path E:\ -Recurse -Include *.img,*.ind
```

If you already copied the game files somewhere, search that folder instead.

Example:

```powershell
Get-ChildItem -Path "$env:USERPROFILE\Games" -Recurse -Include *.img,*.ind
```

You need a matching pair, for example:

```text
RCS.IND
RCS.IMG
```

## 4. Create a Working Folder on Windows

Create folders for the input files and extracted output:

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\davilex_extract\work"
New-Item -ItemType Directory -Force "$env:USERPROFILE\davilex_extract\output"
```

## 5. Copy the `.IMG` and `.IND` Files Locally on Windows

Copy the files from the disc or game folder into the working folder.

Example if the files are on drive `D:`:

```powershell
Copy-Item "D:\path\to\RCS.IND" "$env:USERPROFILE\davilex_extract\work\"
Copy-Item "D:\path\to\RCS.IMG" "$env:USERPROFILE\davilex_extract\work\"
```

Replace `D:\path\to\` with the real location of your files.

Example:

```powershell
Copy-Item "D:\DATA\RCS.IND" "$env:USERPROFILE\davilex_extract\work\"
Copy-Item "D:\DATA\RCS.IMG" "$env:USERPROFILE\davilex_extract\work\"
```

## 6. Check That the Files Are Really There on Windows

Before extracting, verify that the files were copied correctly:

```powershell
Get-ChildItem "$env:USERPROFILE\davilex_extract\work"
```

You should see something like:

```text
RCS.IND
RCS.IMG
```

## 7. Extract the Files on Windows

Go back into the Davitools folder:

```powershell
cd "$env:USERPROFILE\Downloads\Davitools"
```

Run the extractor:

```powershell
php rcs-extract.php "$env:USERPROFILE\davilex_extract\work\RCS.IND" "$env:USERPROFILE\davilex_extract\work\RCS.IMG" "$env:USERPROFILE\davilex_extract\output"
```

Again, the order matters:

```text
.IND first
.IMG second
output folder third
```

## 8. View the Extracted Files on Windows

List the extracted files:

```powershell
Get-ChildItem "$env:USERPROFILE\davilex_extract\output"
```

To show all extracted files recursively:

```powershell
Get-ChildItem "$env:USERPROFILE\davilex_extract\output" -Recurse -File
```

## Full Windows PowerShell Example

```powershell
cd "$env:USERPROFILE\Downloads"
git clone https://github.com/Gymnasiast/Davitools.git

New-Item -ItemType Directory -Force "$env:USERPROFILE\davilex_extract\work"
New-Item -ItemType Directory -Force "$env:USERPROFILE\davilex_extract\output"

Copy-Item "D:\DATA\RCS.IND" "$env:USERPROFILE\davilex_extract\work\"
Copy-Item "D:\DATA\RCS.IMG" "$env:USERPROFILE\davilex_extract\work\"

cd "$env:USERPROFILE\Downloads\Davitools"

php rcs-extract.php "$env:USERPROFILE\davilex_extract\work\RCS.IND" "$env:USERPROFILE\davilex_extract\work\RCS.IMG" "$env:USERPROFILE\davilex_extract\output"

Get-ChildItem "$env:USERPROFILE\davilex_extract\output"
```

---

# Linux Instructions

## 1. Install PHP and Git

### Fedora

```bash
sudo dnf install php-cli git
```

### Debian / Ubuntu / Linux Mint

```bash
sudo apt update
sudo apt install php-cli git
```

Check that PHP is installed:

```bash
php --version
```

Check that Git is installed:

```bash
git --version
```

## 2. Download Davitools

Go to your Downloads folder:

```bash
cd ~/Downloads
```

Clone the Davitools GitHub repository:

```bash
git clone https://github.com/Gymnasiast/Davitools.git
```

Enter the Davitools folder:

```bash
cd Davitools
```

## 3. Find the `.IMG` and `.IND` Files

If your Davilex game disc is mounted, you can search it like this:

```bash
find /run/media/$USER -iname "*.img" -o -iname "*.ind"
```

If you copied the disc contents somewhere else, search that folder instead.

Example:

```bash
find ~/Games -iname "*.img" -o -iname "*.ind"
```

You need a matching pair, for example:

```text
RCS.IND
RCS.IMG
```

## 4. Create a Working Folder

Create folders for the input files and extracted output:

```bash
mkdir -p ~/davilex_extract/work
mkdir -p ~/davilex_extract/output
```

## 5. Copy the `.IMG` and `.IND` Files Locally

Copy the files from the disc or game folder into the working folder.

Example:

```bash
cp "/path/to/RCS.IND" ~/davilex_extract/work/
cp "/path/to/RCS.IMG" ~/davilex_extract/work/
```

Replace `/path/to/` with the real location of your files.

Example from a mounted disc:

```bash
cp "/run/media/$USER/GAME_DISC/RCS.IND" ~/davilex_extract/work/
cp "/run/media/$USER/GAME_DISC/RCS.IMG" ~/davilex_extract/work/
```

## 6. Check That the Files Are Really There

Before extracting, verify that the files were copied correctly:

```bash
ls -lah ~/davilex_extract/work
```

You should see something like:

```text
RCS.IND
RCS.IMG
```

Linux filenames are case-sensitive.

This means these are all different filenames:

```text
RCS.IND
RCS.ind
rcs.ind
```

Use the exact filename shown by `ls`.

## 7. Extract the Files on Linux

Go back into the Davitools folder:

```bash
cd ~/Downloads/Davitools
```

Run the extractor:

```bash
php rcs-extract.php ~/davilex_extract/work/RCS.IND ~/davilex_extract/work/RCS.IMG ~/davilex_extract/output
```

## 8. View the Extracted Files on Linux

List the extracted files:

```bash
ls -lah ~/davilex_extract/output
```

To show all extracted files recursively:

```bash
find ~/davilex_extract/output -type f
```

To inspect the file types:

```bash
find ~/davilex_extract/output -type f -exec file {} \; | less
```

## Full Linux Example

```bash
sudo dnf install php-cli git

cd ~/Downloads
git clone https://github.com/Gymnasiast/Davitools.git

mkdir -p ~/davilex_extract/work
mkdir -p ~/davilex_extract/output

cp "/path/to/RCS.IND" ~/davilex_extract/work/
cp "/path/to/RCS.IMG" ~/davilex_extract/work/

cd ~/Downloads/Davitools
php rcs-extract.php ~/davilex_extract/work/RCS.IND ~/davilex_extract/work/RCS.IMG ~/davilex_extract/output

ls -lah ~/davilex_extract/output
```

---

# Troubleshooting

## Error: `Failed to open stream: No such file or directory`

Example:

```text
PHP Warning: fopen(/home/user/davilex_extract/work/RCS.IND): Failed to open stream: No such file or directory
```

This means the file does not exist at the path you gave to the extractor.

### Linux

Check the folder:

```bash
ls -lah ~/davilex_extract/work
```

Search for the actual files:

```bash
find ~/davilex_extract -iname "*.ind" -o -iname "*.img"
```

Then run the extractor again using the exact filenames.

### Windows PowerShell

Check the folder:

```powershell
Get-ChildItem "$env:USERPROFILE\davilex_extract\work"
```

Search for the actual files:

```powershell
Get-ChildItem "$env:USERPROFILE\davilex_extract" -Recurse -Include *.img,*.ind
```

Then run the extractor again using the exact filenames.

## Error: `Could not open input file: rcs-extract.php`

This means you are not inside the Davitools folder.

### Linux

```bash
cd ~/Downloads/Davitools
```

### Windows PowerShell

```powershell
cd "$env:USERPROFILE\Downloads\Davitools"
```

Then try again.

## Error: `php` Is Not Recognized on Windows

If Windows shows something like:

```text
php is not recognized as an internal or external command
```

then PHP is not installed correctly or is not in your `PATH`.

Fix:

1. Install PHP for Windows.
2. Add the PHP folder to the Windows `PATH`.
3. Close and reopen PowerShell.
4. Run:

```powershell
php --version
```

## Do Not Use `sudo` on Linux

Normally, you should not run the extractor with `sudo`.

Use:

```bash
php rcs-extract.php ~/davilex_extract/work/RCS.IND ~/davilex_extract/work/RCS.IMG ~/davilex_extract/output
```

Not:

```bash
sudo php rcs-extract.php ~/davilex_extract/work/RCS.IND ~/davilex_extract/work/RCS.IMG ~/davilex_extract/output
```

Using `sudo` can create extracted files owned by root, which can cause permission problems later.

## Use Tab Completion

If you are unsure about the exact filename, type part of the command and press `Tab`.

### Linux

```bash
php rcs-extract.php ~/davilex_extract/work/
```

### Windows PowerShell

```powershell
php rcs-extract.php "$env:USERPROFILE\davilex_extract\work\
```

Then press `Tab` to let the terminal complete the filename.

## Notes

- Always use the matching `.IND` and `.IMG` files from the same game archive.
- Do not mix an `.IND` file from one archive with an `.IMG` file from another archive.
- Some Davilex games may use multiple `.IMG` / `.IND` pairs.
- If extraction succeeds but the output files do not have useful extensions, inspect them with tools such as `file` on Linux or a hex editor on Windows.
