---
layout: post
title: "Bash One-Liners for Lichess Puzzle Sleuthing"
date: 2024-09-29
---

Chess tactics and shell scripts make an oddly satisfying pairing. Here are the bash one-liners I use to spot my own games inside the ever-growing Lichess puzzle set.

## Pull Down the Data

```bash
# Replace parsnips with your Lichess handle
declare -r USERNAME="parsnips"

curl -L "https://lichess.org/games/export/${USERNAME}" -o "lichess_${USERNAME}.pgn"
curl -L "https://database.lichess.org/lichess_db_puzzle.csv.bz2" -o lichess_db_puzzle.csv.bz2
```

## Decompress the Puzzle Archive

```bash
bzip2 -d lichess_db_puzzle.csv.bz2
```

## Capture Every Puzzle Game URL

```bash
cat lichess_db_puzzle.csv \
  | cut -d ',' -f9 \
  | cut -d '#' -f1 \
  | sed -e 's|/black||' \
  | sort -u \
  > puzzle-games
```

The pipeline slices the puzzle metadata, trims color suffixes, and deduplicates the URLs so each source game appears once.

## Extract Your Own Games

```bash
grep -e 'Site' "lichess_${USERNAME}.pgn" \
  | grep -o -e '\\".*\\"' \
  | sed -e 's|"||g' \
  | sort -u \
  > my-games
```

PGN headers keep the game URL in the `Site` field—we peel it out with grep and leave a neat list to compare.

## Reveal the Overlap

```bash
grep -F -f my-games puzzle-games
```

That single command highlights every Lichess puzzle sourced from your own play. I like to feed the results into a [study](https://lichess.org/study/EXvRdFYD) so I can relive the tactics and annotate the critical ideas.

## Why It Works

Each line sticks to portable Unix tools: `curl` handles downloads, `cut` and `sed` reshape CSV fragments, and `grep -F -f` performs the fast set intersection. Once you’ve scripted the workflow, updating your study is a nightly ritual—no GUI, just a few keystrokes and a fresh set of puzzles to celebrate (or cringe at).

Happy hacking, happy checkmating.
