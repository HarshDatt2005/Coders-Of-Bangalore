# 📊 OpenAI Instagram Follower Analysis

> A pure-Python data parsing pipeline built under a 24-hour deadline.

---

## 🧵 The Questions

> *Collect raw Instagram data of all OpenAI followers and answer these questions — in 24 hours.*

The questions:
- 👤 Who has the **maximum posts**?
- 🌍 Who has the **maximum followers**?
- 👥 Who follows the **maximum people**?
- 🏷️ How many **page categories** exist (Digital Creator, Non-profit Foundation, etc.)?

---

## 🗂️ Project Structure

```
.
├── Initial_data.txt           # Demo dataset used during development
├── Final_data.txt             # Real collected Instagram follower data
├── Coders_of_Bangalore.ipynb  # Core parsing pipeline & Analysis: max posts, followers, following, categories
└── Data.json                  # Final Output in JSON File
```

---

## ⚙️ How It Works

### 1. Loads the Data

The raw data file contains multiple Instagram profiles, each separated by a blank line.

```python
with open ("Final_data.txt", encoding = "utf-8") as f:
    data = f.read()
```

### 2. Splits Into Profile Blocks

```python
chunks = data.split("\n\n")
chunks = [c for c in chunks if len(c)>3]
```

### 3. Parses Each Profile Into a Dictionary

```python
def parse_chunk(chunk):
        chunk = chunk.strip()
        sep_chunk = chunk.split("\n")
        username = sep_chunk[0]
        no_of_posts = int(sep_chunk[1].split(" post")[0].replace(",",""))
    
        no_of_followers = float(sep_chunk[2].split(" follower")[0].replace(",","").replace("K","").replace("M",""))
        if("K" in sep_chunk[2]):
            no_of_followers = int(no_of_followers * 1000)
        elif("M" in sep_chunk[2]):
            no_of_followers = int(no_of_followers * 1000000)
        else:
            no_of_followers = int(no_of_followers)
            
        no_of_following = float(sep_chunk[3].split(" following")[0].replace(",","").replace("K","").replace("M",""))
        if("K" in sep_chunk[3]):
            no_of_following = int(no_of_following * 1000)
        elif("M" in sep_chunk[3]):
            no_of_following = int(no_of_following * 1000000)
        else:
            no_of_following = int(no_of_following)
            
        name = sep_chunk[4]
        if(len(sep_chunk) > 5):
            type_of_page = sep_chunk[5]
            bio = "\n".join(sep_chunk[6:])
        else:
            type_of_page = "Unkown"
            bio = ""
        return {"username": username, "no_of_posts": no_of_posts, "no_of_followers": no_of_followers, "no_of_following": no_of_following, "name": name, "type_of_page": type_of_page, "bio": bio}
```

### 4. Analyses the Data

**Max posts:**
```python
# Code to find the max posts:
max = 0
for chunk in all_chunks:
    if(max < chunk["no_of_posts"]):
        max = chunk["no_of_posts"]
        chunk_with_max_posts = chunk
print(chunk_with_max_posts["username"])
```

**Max followers:**
```python
# code to find who has max followers:
max = 0
for chunk in all_chunks:
    if(max < chunk["no_of_followers"]):
        max = chunk["no_of_followers"]
        chunk_with_max_followers = chunk
print(chunk_with_max_followers["username"])
```

**Max following:**
```python
# Code to find the max people:
max = 0
for chunk in all_chunks:
    if(max < chunk["no_of_following"]):
        max = chunk["no_of_following"]
        chunk_with_max_following = chunk
print(chunk_with_max_following["username"])
```

**All unique page categories:**
```python
# Code to find the number categories:
categories = set()
for chunk in all_chunks:
    categories.add(chunk["type_of_page"])
print(f"There are {len(categories)} categories!!")
```

---

## 📋 Data Format

Each profile block in the `.txt` file follows this structure:

```
Name: <profile name>
Posts: <number>
Followers: <number>
Following: <number>
Bio: <bio text>
Type: <page category>
```

Profiles are separated by a blank line.

---

## 💡 Key Design Decisions

- **Pure Python only** — No external libraries or data-science frameworks. The parsing pipeline uses only built-in string operations.
- **Demo-first development** — The parsing logic was validated on `Initial_data.txt` before the real data arrived, so integration was seamless.
- **Consistent data format** — Maintained a strict structure during collection, which meant zero changes were needed when switching from demo to final data.

---
