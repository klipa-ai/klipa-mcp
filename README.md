# Klipa MCP connector

Edit videos from your AI assistant. Klipa is a web video toolbox (AI clips, animated subtitles, vertical reframing, transcription, translation, compression). This connector lets Claude, ChatGPT and other MCP clients use it with your own Klipa account.

- **Server URL:** `https://mcp.klipa.ai/mcp`
- **Transport:** Streamable HTTP
- **Auth:** OAuth 2.1 (per the MCP spec). No API key: you sign in to Klipa and click "Allow". Clients that only accept a fixed key can use a personal access key created on https://klipa.ai/en/mcp.
- **Official MCP Registry:** `ai.klipa/klipa`
- **Docs and setup page:** https://klipa.ai/en/mcp

This repository only documents the hosted connector. The server itself is run by Klipa; there is nothing to install.

## Setup

### Claude (claude.ai, desktop, mobile)
Customize → Connectors → "+" → Add custom connector. Name: `Klipa`, URL: `https://mcp.klipa.ai/mcp`. Click Add, then Connect, sign in to Klipa and click "Allow".

### ChatGPT
1. Settings → Security and login → Developer mode (availability depends on your ChatGPT plan).
2. Go to chatgpt.com/plugins → "+", name it `Klipa` and paste `https://mcp.klipa.ai/mcp` as the connection address.
3. Sign in to Klipa and click "Allow".

### Claude Code
```bash
claude mcp add --transport http klipa https://mcp.klipa.ai/mcp
```

### Cursor (`~/.cursor/mcp.json`)
```json
{
  "mcpServers": {
    "klipa": { "url": "https://mcp.klipa.ai/mcp" }
  }
}
```

### VS Code (`.vscode/mcp.json`)
```json
{
  "servers": {
    "klipa": { "type": "http", "url": "https://mcp.klipa.ai/mcp" }
  }
}
```

Other MCP clients with remote server and OAuth support (Mistral Le Chat, etc.) work with the same URL.

## Tools

| Tool | What it does |
|---|---|
| `get_account` | Your plan, credits and limits |
| `estimate_credits` | Cost of a job before running it |
| `create_upload_link` | Private link to upload a file from your device (up to 2 GB) |
| `create_ai_clips` | Finds the best moments of a long video and returns scored vertical clips |
| `export_clips` | Exports selected clips |
| `make_highlight_video` | Best-of montage of a whole video |
| `add_subtitles` | Word-by-word animated subtitles, optional translated line |
| `show_subtitle_styles` | Lists the subtitle styles so the user can pick one |
| `transcribe_video` | Transcript as SRT, VTT or text |
| `reframe_video` | 9:16, 1:1 or 4:5, following the speaker |
| `remove_silences` | Cuts silences and filler words |
| `clean_audio` | Removes background noise |
| `extract_audio` | MP3 or WAV |
| `compress_video`, `convert_video`, `resize_video` | Smaller files, MP4/WebM/MOV/AVI/MKV |
| `cut_video`, `change_speed`, `rotate_video`, `make_gif` | Quick edits |
| `get_job`, `list_jobs`, `cancel_job` | Follow or cancel long jobs |

## How it behaves

- A video comes from a public link or from a file uploaded through `create_upload_link`.
- A finished job's `job_id` can be passed as `source_id` to chain another tool on its result (for example: remove silences, then add subtitles).
- Processing tools wait about 40 seconds. Longer jobs return a `job_id` to check with `get_job`.
- Download links are valid for 72 hours. Results also appear in your Klipa projects on the website.
- The connector uses the same plan, credits and limits as the website. A free account is enough to try it (with the Klipa watermark).

## Example prompts

- "What can I do with my Klipa plan?"
- "Find the 3 best moments of this video for TikTok: [link]"
- "Remove the silences from my interview, then add subtitles."
- "Turn this landscape video into a vertical 9:16 video: [link]"
- "I want to compress a video from my computer: give me a link to upload it."

## Support

hello@klipa.ai · https://klipa.ai
