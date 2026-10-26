# Video editor project notes

- This session runs in a Claude Code **cloud container** (Linux x86_64, 4 CPU, 15 GB RAM, no GPU), not on the user's machine. The editing app (Resolve/Premiere/Final Cut) is not reachable from here, so live app control (step 3) must run from Claude Code on the user's own computer.
- Container tools: ffmpeg/ffprobe 6.1.1, uv 0.11, git 2.43, Node 22.22, Python 3.11 at /usr/bin/python3.11. WhisperX here would be CPU int8.
- Folders: raw/ (raw clip), transcript/ (words.json, cuts.json), scripts/, vendor/.

## User's answers (step 2)
- Finishing app: **DaVinci Resolve Studio 21.1.1** → drive it live via Resolve's built-in connector (File > Setup AI Assistants).
- User chose to run everything from Claude Code **on their own machine**, not the cloud container. Re-run the step 1 machine check locally, then continue from step 3.
- The user has both a MacBook Air and a **Windows PC**. Resolve, the footage and the external drives are on the **PC**, so Windows rules apply (Git Bash, PYTHONUTF8=1, HF_HUB_DISABLE_SYMLINKS=1, Controlled Folder Access test).

## Handoffs
- `handoffs/for-bari.md`: proxies plus a .drt for editor Bari (Resolve; H.264 1080p; trimmed with 2 s handles; to the external drive with room, else ~/Desktop/For Bari). Must run on the machine with the footage and Resolve, which is the user's **Windows PC** (not the MacBook).
