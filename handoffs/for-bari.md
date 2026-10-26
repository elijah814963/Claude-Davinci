# Task: hand the cut timeline to Bari (run on the machine where the cut lives, with Resolve Studio open; the user says that's their **Windows PC**)

User's answers (already decided, don't re-ask):
- Bari edits in **DaVinci Resolve**.
- Proxies: **H.264, 1080p** (long edge 1920; keep the footage's orientation and frame rate).
- **Trimmed to the cut plus handles** (use 2 s handles), not full raw clips.
- Delivery is an **upload / cloud link**, so keep it small.

## Steps
1. **Pick the destination.** List the external drives.
   - **Windows (Git Bash):** run `powershell -NoProfile -Command "Get-CimInstance Win32_LogicalDisk | Select DeviceID,VolumeName,DriveType,FreeSpace,Size"` and use DriveType 2 (removable) or 3 (fixed) drives other than the system drive (`C:`). External USB SSDs often show as type 3, so use VolumeName and Size to tell them apart, and ask the user if it's unclear.
   - **macOS:** run `df -h /Volumes/*`, skipping the boot volume and Time Machine. Estimate size as used duration of the timeline plus 4 s per clip, at about 10 Mbps (around 4.5 GB per hour), then double it for safety. Use the external drive with the most free space that fits, at `<drive>:\For Bari\` on Windows or `/Volumes/<drive>/For Bari/` on macOS. If none fit (or none are mounted), use the Desktop: on Windows, get the real path from `powershell -NoProfile -Command "[Environment]::GetFolderPath('Desktop')"`, because OneDrive often moves it; on macOS use `~/Desktop`. Tell the user which one you picked and why.
2. **Never touch the original timeline.** Duplicate it in Resolve as `<name> - For Bari`.
3. **Make the trimmed proxies.** Use Resolve's Media Management (scripting it if possible; otherwise ask the user to click File > Media Management > Timelines):
   - Timelines tab, select only the `For Bari` duplicate. Mode: **Transcode**, used media only, handles **2 s** (`48`/`60` frames, whatever matches the frame rate).
   - Format: MP4, codec **H.264**, resolution 1920x1080 (or 1080x1920 for vertical footage), source frame rate, AAC audio.
   - Destination: `For Bari/Media/`.
   - Turn **on** "Relink to new files" so the duplicate timeline points at the proxies, and turn on keeping source file names and timecode. Bari's Resolve relinks by file name and timecode, so trimmed clips must keep their source timecode.
4. **Export the timeline.** Select the duplicate, then File > Export > Timeline as `For Bari/<name>.drt`. Also write an FCP 7 XML (`.xml`) next to it as a fallback.
5. **Verify.** ffprobe every proxy: H.264, 1080 on the short side, source frame rate, embedded timecode present. Count that the timeline's clips all point into `For Bari/Media/` (no offline clips). Compare the clip count and total length with the original timeline.
6. **Write `For Bari/README.txt` for Bari:**
   - In Resolve, open or create a project at the same frame rate as the cut.
   - File > Import > Timeline, pick the `.drt`, and leave "Automatically import source clips into media pool" ticked.
   - If clips show offline, select them in the Media Pool, choose Relink Selected Clips, and point at the `Media` folder.
   - These are trimmed proxies with 2 s handles, so he can't extend a cut further than that.
   - Note the frame rate and resolution.
7. **Report** the folder path, its total size, and anything that went offline. Suggest zipping `For Bari` for upload: on Windows, `powershell -NoProfile -Command "Compress-Archive -Path 'For Bari' -DestinationPath 'For Bari.zip'"`; on macOS, `ditto -c -k --keepParent "For Bari" "For Bari.zip"`.

On Windows, export `PYTHONUTF8=1` in any script, and run everything from Git Bash, never WSL.
