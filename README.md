# Absolute-Pathfinder
Take a keyword, select a drive, and the software will scan the drive and provide a list of absolute paths that include the submitted keyword

This script is not fancy, it isn't written well, and the only purpose it serves is to solve the niche problem I had of the standard Explorer being too slow and too inprecise for my liking.

The .exe will ask you for a keyword, and will ask you which drive you would like to search. After some time, it will populate a list of absolute paths that you can copy and paste into your file explorer. At this time, you can only search one drive at a time.

Totally unnecessary? Absolutely.
Will you be happy it exists? Maybe.

![image](https://github.com/user-attachments/assets/f38af06f-ba9a-4c66-ba62-2c88ae63ed43)

![image](https://github.com/user-attachments/assets/5652b467-216b-40a5-a0ea-62b24b073f92)

![image](https://github.com/user-attachments/assets/d0cf21bf-d097-4cb0-a52a-d0b92e2587b0)

```ruby
#fragment variable = keyword
#drive variable = desired drive
#search_root executes a search on the drive specified by the aforementioned variable

while True:
    fragment = input("Enter part of the filename to search for: ").strip()
    drive = input("Which drive do you want to search? (C, D, etc.): ").strip().lower()
    search_root = f"{drive}:\\"
```

```ruby
#Does the drive exist? Does the keyword exist within any files? This will give you whatever response is appropriate.

    if not os.path.exists(search_root):
        print(f"\nError: Drive '{drive}:' does not exist or is not accessible.\n")
        continue

    results = find_files_by_fragment(fragment, search_root)

    if results:
        print(f"\nFound {len(results)} file(s) matching '{fragment}' on drive {drive}:\n")
        for path in results:
            print(path)
    else:
        print(f"No file(s) found on drive {drive}")
```

```ruby
#This function outlines tqdm's rules for its progress; keeping track of the directory searched and the time spent. It also
#explains that if the fragment is found, it will append the 'matches' list to include the Absolute Path of whatever
#file is highlighted by that keyword. This is the meat and potatoes.

def find_files_by_fragment(fragments, search_path):
    matches = []

    for root, dirs, files in tqdm(os.walk(search_path), desc="Searching", unit="dir", file=sys.stdout, dynamic_ncols=True):
        for name in files:
            if fragments.lower() in name.lower():
                full_path = os.path.join(root, name)
                matches.append(full_path)

    return matches
```
