# ccbar

A native macOS menu bar app for real-time [Claude Code](https://claude.ai/code) usage monitoring, powered by [ccusage](https://github.com/ryoppippi/ccusage).

<p align="center">
  <img src="assets/menu-bar-preview.png" alt="ccbar in action" width="400">
  <br>
  <em>Monitor your Claude Code usage without leaving your workflow</em>
</p>

## Features

### 🎯 Real-Time Monitoring

- **Live usage tracking** - See your token consumption as you code
- **5-hour block awareness** - Track Claude's billing windows
- **Burn rate analysis** - Monitor your usage velocity
- **Cost projections** - Estimate session costs in real-time

### 📊 Comprehensive Analytics

- **Daily reports** - Token usage and costs by date
- **Monthly summaries** - Long-term usage patterns
- **Session analysis** - Per-conversation breakdowns
- **Model tracking** - See Opus vs Sonnet usage

### 🎨 Native macOS Experience

- **Menu bar integration** - Always accessible, never intrusive
- **SwiftUI interface** - Smooth, responsive, and beautiful
- **Dark mode support** - Seamless system integration
- **Keyboard shortcuts** - Quick access to key features

### 🚀 Smart Setup

- **Auto-detection** - Finds ccusage installation automatically
- **Guided installation** - Step-by-step setup if needed
- **Multiple package managers** - Supports npm, bun, pnpm
- **Zero configuration** - Works out of the box

## Installation

### Prerequisites

- macOS 13.0 or later
- [Claude Code](https://claude.ai/code) installed and used
- Node.js or Bun (for ccusage)

### Option 1: Download Release

1. Download the latest `.dmg` from [Releases](https://github.com/yourusername/ccbar/releases)
2. Open the DMG and drag ccbar to Applications
3. Launch ccbar from Applications or Spotlight

### Option 2: Build from Source

```bash
# Clone the repository
git clone https://github.com/yourusername/ccbar.git
cd ccbar

# Open in Xcode
open ccbar.xcodeproj

# Build and run (⌘+R)
```

## First Launch

When you first open ccbar:

1. **ccbar checks for ccusage** - It searches common installation paths
2. **If ccusage is found** - You're ready to go! Usage data appears immediately
3. **If ccusage is missing** - ccbar shows installation options:

```bash
# Recommended: Install with npm
npm install -g ccusage

# Alternative: Using bun (faster)
bun install -g ccusage

# Alternative: Using pnpm
pnpm install -g ccusage
```

4. **Verify installation** - Click "Check Again" after installing
5. **Start monitoring** - Your usage data appears in the menu bar

## Usage

### Menu Bar Icon

The menu bar icon shows your current usage status:

- 🟢 **Green**: Normal usage (< 60% of typical limits)
- 🟡 **Yellow**: Moderate usage (60-80%)
- 🔴 **Red**: High usage (> 80%)
- ⚡ **Lightning**: Active session in progress

### Quick Actions

Click the menu bar icon for:

- Current session overview
- Today's total cost
- Quick access to detailed reports
- Settings and preferences

### Keyboard Shortcuts

- `⌘,` - Open preferences
- `⌘R` - Refresh usage data
- `⌘Q` - Quit ccbar

## Understanding the Data

### 5-Hour Blocks

Claude Code bills in 5-hour blocks. ccbar shows:

- Time remaining in current block
- Token usage within the block
- Burn rate and projections
- Cost for the current block

### Cost Calculations

ccbar displays costs based on:

- Actual costs from Claude (when available)
- Calculated costs from token usage
- Model-specific pricing (Opus vs Sonnet)

### Token Types

- **Input**: Tokens sent to Claude
- **Output**: Tokens received from Claude
- **Cache**: Tokens used for context caching
- **Total**: Combined token usage

## Configuration

### Settings

Access via menu bar icon → Preferences:

- **Refresh interval**: How often to update (1-60 seconds)
- **Token limits**: Set custom warning thresholds
- **Display options**: Choose what to show in menu bar
- **Data source**: Configure Claude data directory

### Advanced Options

For power users, ccbar respects ccusage environment variables:

```bash
# Custom Claude data directory
export CLAUDE_CONFIG_DIR="/path/to/claude/data"

# Use offline pricing data
export CCUSAGE_OFFLINE=1
```

## Troubleshooting

### ccusage Not Found

- Verify installation: `which ccusage`
- Check PATH: `echo $PATH`
- Try installing globally with sudo (not recommended)
- Use ccbar's installation guide

### No Usage Data

- Ensure Claude Code has been used
- Check data directory permissions
- Verify JSONL files exist in `~/.claude/projects/`
- Try running `ccusage daily` in terminal

### Performance Issues

- Large usage history may slow updates
- Increase refresh interval in settings
- Clear old session data if needed
- Report issues on GitHub

## How It Works

ccbar integrates with ccusage, a powerful CLI tool that analyzes Claude Code's local JSONL files. Here's the flow:

1. Claude Code generates usage data in JSONL files
2. ccusage reads and analyzes these files
3. ccbar executes ccusage commands and parses the output
4. You see beautiful, real-time usage analytics

## Contributing

We welcome contributions! See CONTRIBUTING.md for guidelines.

### Development Setup

1. Fork and clone the repository
2. Open in Xcode 16+ (tested with [Xcode 26 beta 3](https://developer.apple.com/documentation/xcode-release-notes/xcode-26-release-notes))
3. Build and run
4. Make your changes
5. Submit a pull request

## Credits

- [ccusage](https://github.com/ryoppippi/ccusage) by @ryoppippi - The powerful CLI tool that makes ccbar possible
- [The Composable Architecture](https://pointfreeco.github.io/swift-composable-architecture/) - Robust state management
- [swift-macos-template](https://github.com/simonweniger/swift-macos-template) by @simonweniger - Project foundation

## License

MIT License - see LICENSE file for details.