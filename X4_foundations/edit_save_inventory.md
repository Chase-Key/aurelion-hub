# X4 Foundations — Save File Inventory Editing

## Context

X4 Foundations saves are stored as gzip-compressed XML files (`.xml.gz`). This documents the exact process used to manually inject an item into the player's personal inventory when it is unavailable in-game (e.g. removed in a patch or bugged out of the loot pool).

**Confirmed working on:** X4 Foundations v9.00 build 611726, save `autosave_02.xml.gz`  
**Player name:** Skyrad  
**Item added:** Programmable Field Array (`inv_programmablefieldarray`) — 3 units

---

## File Locations

| File | Path |
|---|---|
| Game install | `C:\Program Files (x86)\Steam\steamapps\common\X4 Foundations` |
| Save directory | `C:\Users\chase\OneDrive\Documents\Egosoft\X4\71022833\save\` |
| Target save | `autosave_02.xml.gz` |

> **Note:** Most game data (ware definitions, etc.) is packed inside `.cat`/`.dat` archives in the game install. Loose XML files under `t\` are translation/text files only.

---

## Step 1 — Back Up the Save

Always back up before editing.

```python
import shutil

src = r'C:\Users\chase\OneDrive\Documents\Egosoft\X4\71022833\save\autosave_02.xml.gz'
bak = r'C:\Users\chase\OneDrive\Documents\Egosoft\X4\71022833\save\autosave_02.xml.gz.bak'
shutil.copy2(src, bak)
print('Backup created:', bak)
```

---

## Step 2 — Decompress the Save

```python
import gzip, os

src = r'C:\Users\chase\OneDrive\Documents\Egosoft\X4\71022833\save\autosave_02.xml.gz'
dst = r'C:\Users\chase\AppData\Local\Temp\autosave_02_x4.xml'

with gzip.open(src, 'rb') as f_in, open(dst, 'wb') as f_out:
    f_out.write(f_in.read())

print('Decompressed size:', os.path.getsize(dst), 'bytes')
```

> The decompressed file was ~700 MB for this save. Use a temp folder with enough free space.

---

## Step 3 — Find the Item ID

The item ID for **Programmable Field Array** is:

```
inv_programmablefieldarray
```

To verify or find other item IDs, search the decompressed XML:

```python
xmlfile = r'C:\Users\chase\AppData\Local\Temp\autosave_02_x4.xml'

with open(xmlfile, 'r', encoding='utf-8', errors='replace') as f:
    for i, line in enumerate(f, 1):
        if 'programmable' in line.lower():
            print(f'Line {i}: {line.rstrip()[:200]}')
```

Item IDs in inventory entries follow the pattern `inv_<name>`.

---

## Step 4 — Locate the Player Inventory Section

The player entity is near the top of the save at line 6:

```xml
<player name="Skyrad" location="{20004,140011}" money="50766820"/>
```

The actual player component (with inventory) is deeper in the file:

```xml
<component class="player" macro="character_player_terran_gamestart1_macro" ... name="Skyrad" ...>
    ...
    <inventory>
        <ware ware="weapon_gen_spacesuit_repairlaser_01_mk1"/>
        <ware ware="inv_bandages" amount="48"/>
        ...
    </inventory>
```

To find it programmatically:

```python
xmlfile = r'C:\Users\chase\AppData\Local\Temp\autosave_02_x4.xml'

with open(xmlfile, 'r', encoding='utf-8', errors='replace') as f:
    for i, line in enumerate(f, 1):
        if 'skyrad' in line.lower():
            print(f'Line {i}: {line.rstrip()[:200]}')
```

In this save the `<inventory>` block was at **lines 5175878–5175961**, with `</inventory>` at line **5175961**.

---

## Step 5 — Edit the XML and Recompress

Insert the new ware entry just before `</inventory>` and write the modified file back as `.gz` in one pass:

```python
import gzip, os

xmlfile = r'C:\Users\chase\AppData\Local\Temp\autosave_02_x4.xml'
outxml  = r'C:\Users\chase\AppData\Local\Temp\autosave_02_x4_modified.xml'
savegz  = r'C:\Users\chase\OneDrive\Documents\Egosoft\X4\71022833\save\autosave_02.xml.gz'

INSERT_BEFORE_LINE = 5175961  # The line number of </inventory>
ware_id = 'inv_programmablefieldarray'
NEW_LINE = '            <ware ware="{}" amount="3"/>\n'.format(ware_id)

# Write modified XML
with open(xmlfile, 'r', encoding='utf-8', errors='replace') as fin, \
     open(outxml, 'w', encoding='utf-8') as fout:
    for i, line in enumerate(fin, 1):
        if i == INSERT_BEFORE_LINE:
            fout.write(NEW_LINE)
        fout.write(line)

print('Modified XML written:', os.path.getsize(outxml), 'bytes')

# Recompress back to .gz (overwrites the save)
with open(outxml, 'rb') as fin, gzip.open(savegz, 'wb') as fout:
    chunk_size = 1024 * 1024
    while True:
        chunk = fin.read(chunk_size)
        if not chunk:
            break
        fout.write(chunk)

print('Save written:', savegz, '-', os.path.getsize(savegz), 'bytes')
```

---

## Inventory XML Format

```xml
<!-- Single unit (amount defaults to 1 when omitted) -->
<ware ware="inv_programmablefieldarray"/>

<!-- Multiple units -->
<ware ware="inv_programmablefieldarray" amount="3"/>
```

---

## Notes

- **Line numbers will shift** between saves. Always re-search for `</inventory>` within the player component section rather than hardcoding line 5175961.
- The `.bak` file is the safe restore point. If the game rejects the save, copy it back and retry.
- X4 does not validate inventory contents on load — any `inv_*` ware ID can be injected this way.
- The save also had a mission objective `"Programmable Field Array (0 / 3)"` — the injected items satisfied that delivery quest on load.
