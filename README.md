# Instagram-Reel-To-YouTube-Compilation
An automated tool to fetch Instagram reels and compile them into a YouTube-ready video. This project simplifies content curation by seamlessly gathering, processing, and uploading reels as a compilation to YouTube. Includes support for downloading reels, video editing, and uploading via YouTube API.

## Installation Guide

### Prerequisites
1. Python 3.8 or higher
2. A Google Cloud Project with YouTube Data API v3 enabled
3. Instagram account credentials
4. FFmpeg installed for video processing

### Step-by-Step Installation

1. **Clone the Repository**
   ```bash
   git clone <your-repo-url>
   cd <repo-name>
   ```

2. **Create Virtual Environment**
   ```bash
   python -m venv venv
   # On Windows
   .\venv\Scripts\activate
   # On macOS/Linux
   source venv/bin/activate
   ```

3. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Directory Structure Setup**
   Create the following directories in your project root:
   ```bash
   mkdir -p downloads fixed logs output
   ```

### Required Files Setup

1. **Google API Setup**
   1. Go to [Google Cloud Console](https://console.cloud.google.com/)
   2. Create a new project or select an existing one
   3. Enable the YouTube Data API v3
   4. Go to Credentials → Create Credentials → OAuth 2.0 Client ID
   5. Set up OAuth consent screen if not already done
   6. Download the client secrets file
   7. Rename it to `client_secret_[YOUR_CLIENT_ID].apps.googleusercontent.com.json`
   8. Place it in the `fixed/` directory

2. **Description Template**
   1. Create `fixed/description.txt` with your default video description template
   2. Include a line where you want account credits to be inserted (default position is line 27)

3. **Video Assets**
   Place in `fixed/` directory:
   - `intro.mp4` - Your intro video clip
   - `outro.mp4` - Your outro video clip
   - `background.mp4` (optional) - Background video for reels compilation

### Authentication Setup

1. **Instagram Authentication**
   ```bash
   # Install instaloader globally for CLI usage
   pip install instaloader
   
   # Login to Instagram (replace USERNAME with your Instagram username)
   instaloader --login USERNAME
   ```
   This will create a session file in `~/.config/instaloader/`

2. **YouTube Authentication**
   - First run of `uploader.py` will:
     1. Look for the client secrets file in `fixed/`
     2. Open a browser window for Google OAuth
     3. Create `token.json` in the `fixed/` directory automatically

### Directory Structure Explained

```
├── downloads/           # Temporary storage for downloaded reels
├── fixed/              # Contains configuration files
│   ├── client_secret_[...].json  # (Required) Google OAuth credentials
│   ├── description.txt           # (Required) Video description template
│   ├── intro.mp4                 # (Required) Intro video
│   ├── outro.mp4                 # (Required) Outro video
│   ├── background.mp4            # (Optional) Background video
│   └── token.json                # (Auto-generated) YouTube OAuth token
├── logs/               # Log files and backups
├── output/             # Compiled videos and descriptions
└── src/               # Source code
    ├── cleaner.py
    ├── compiler.py
    ├── description.py
    ├── reels_downloader.py
    └── uploader.py
```

### Generated Files
The following files will be automatically generated:

1. `~/.config/instaloader/session-USERNAME`
   - Created after Instagram login via instaloader
   - Contains Instagram session data
   - **DO NOT SHARE THIS FILE**

2. `fixed/token.json`
   - Generated after first YouTube authentication
   - Contains YouTube OAuth tokens
   - **DO NOT SHARE THIS FILE**

3. `output/`
   - `final_compilation.mp4`: Final compiled video
   - `modified_description.txt`: Generated video description

4. `logs/`
   - `video_compiler.log`: Compilation process logs
   - `instagram_login.log`: Instagram download logs
   - `reels_downloader.log`: Reels download process logs
   - Backup directories with timestamps

### Usage

The entire process can be run with a single command:
```bash
python runner.py
```

This script will automatically:
1. Download reels from specified Instagram accounts
2. Compile them into a single video with intro/outro
3. Generate video description with credits
4. Upload the video to YouTube
5. Clean up temporary files

If you need to run individual components separately, you can use:

1. **Download Reels**
   ```bash
   python src/reels_downloader.py
   ```
   Downloads reels from specified Instagram accounts to `downloads/`

2. **Compile Video**
   ```bash
   python src/compiler.py
   ```
   Creates compilation with intro/outro in `output/final_compilation.mp4`

3. **Generate Description**
   ```bash
   python src/description.py
   ```
   Creates video description with credits in `output/modified_description.txt`

4. **Upload to YouTube**
   ```bash
   python src/uploader.py
   ```
   Uploads the compiled video to YouTube

5. **Clean Up**
   ```bash
   python src/cleaner.py
   ```
   Cleans up temporary files and creates backups

### Important Notes

1. **Security**
   - Never share authentication files (`token.json`, instaloader sessions)
   - Keep your Instagram credentials private
   - Protect your Google API credentials

2. **File Management**
   - The `downloads/` directory is cleared after each compilation
   - Old compilations are backed up to `logs/` before being cleared
   - Log files can be safely deleted if needed

3. **Troubleshooting**
   - If YouTube authentication fails, delete `token.json` and re-authenticate
   - If Instagram session expires, run `instaloader --login USERNAME` again
   - Check respective log files in `logs/` for error details

## Contributing

Feel free to open issues or submit pull requests for any improvements.

## License

[MIT License](LICENSE)
