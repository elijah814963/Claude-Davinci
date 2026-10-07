# Video editor project notes

- This session runs in a Claude Code **cloud container** (Linux x86_64, 4 CPU, 15 GB RAM, no GPU), not on the user's machine. The editing app (Resolve/Premiere/Final Cut) is not reachable from here, so live app control (step 3) must run from Claude Code on the user's own computer.
- Container tools: ffmpeg/ffprobe 6.1.1, uv 0.11, git 2.43, Node 22.22, Python 3.11 at /usr/bin/python3.11. WhisperX here would be CPU int8.
- Folders: raw/ (raw clip), transcript/ (words.json, cuts.json), scripts/, vendor/.
