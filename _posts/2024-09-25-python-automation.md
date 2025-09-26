---
layout: post
title: "Automating Daily Tasks with Python"
date: 2024-09-25
---

Python is an excellent tool for automating repetitive tasks. Here's a simple script to organize your downloads folder:

```python
import os
import shutil
from pathlib import Path

def organize_downloads():
    downloads_path = Path.home() / "Downloads"

    # Create folders for different file types
    folders = {
        'images': ['.jpg', '.jpeg', '.png', '.gif', '.bmp'],
        'documents': ['.pdf', '.doc', '.docx', '.txt', '.rtf'],
        'videos': ['.mp4', '.avi', '.mkv', '.mov', '.wmv'],
        'archives': ['.zip', '.rar', '.7z', '.tar', '.gz']
    }

    for folder_name, extensions in folders.items():
        folder_path = downloads_path / folder_name
        folder_path.mkdir(exist_ok=True)

        for file in downloads_path.iterdir():
            if file.suffix.lower() in extensions:
                shutil.move(str(file), str(folder_path / file.name))
                print(f"Moved {file.name} to {folder_name}/")

if __name__ == "__main__":
    organize_downloads()
```

This script automatically sorts your downloads into organized folders based on file type. Run it daily to keep your downloads folder clean!

## Watch it in action

Here's a quick demo of the script running:

<iframe width="560" height="315" src="https://www.youtube.com/embed/dQw4w9WgXcQ" title="Python File Organization Demo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Pretty neat, right? This kind of automation can save you hours each week.