# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ccbar is a SwiftUI-based macOS menu bar application that provides real-time monitoring of Claude Code token usage and costs. It acts as a native GUI wrapper around the [ccusage CLI tool](https://github.com/ryoppippi/ccusage), bringing its powerful analytics directly to your Mac's menu bar.

Key features:
- **Real-time usage monitoring** in the menu bar
- **Live session tracking** with 5-hour billing block awareness
- **Cost analysis** with model-specific breakdowns
- **Automatic ccusage detection** and installation guidance
- **Native macOS interface** following Apple Human Interface Guidelines
- **TCA architecture** for robust state management

## General Development Guidelines

### Swift & Language Features

- **Target Swift 6** with modern language features and Swift concurrency (async/await, actors)
- **Use The Composable Architecture (TCA)** for all state management - [Documentation](https://pointfreeco.github.io/swift-composable-architecture/main/documentation/composablearchitecture/)
- **Use Dependencies library** for dependency injection - [Documentation](https://pointfreeco.github.io/swift-dependencies/main/documentation/dependencies/)
- **Prefer SwiftUI over AppKit** unless functionality requires AppKit
- **Target latest macOS** (13.0+) to use newest APIs without compatibility constraints

### Code Style

- Use 2 spaces for indentation
- Run `swift format -i <path>` to format code
- Avoid excessive comments within function bodies
- Follow Swift naming conventions and style guidelines
- Write clean, readable, and well-documented code

## ccusage Integration

### Understanding ccusage

Based on the source code analysis, ccusage provides these key commands:
- `ccusage daily` - Daily token usage and costs
- `ccusage monthly` - Monthly aggregated reports  
- `ccusage session` - Per-conversation session analysis
- `ccusage blocks` - 5-hour billing window tracking
- `ccusage blocks --live` - Real-time monitoring with progress bars

All commands support:
- `--json` flag for structured output
- `--since` and `--until` for date filtering
- `--mode` for cost calculation modes (auto/calculate/display)
- `--offline` for cached pricing data

### Implementation Requirements

#### CCUsageService

Create a robust service using TCA's Dependencies system:

```swift
struct CCUsageService: DependencyKey {
  // Core functionality
  func checkInstallation() async throws -> InstallationStatus
  func getDailyUsage(since: Date?, until: Date?) async throws -> DailyUsageData
  func getMonthlyUsage(since: Date?, until: Date?) async throws -> MonthlyUsageData  
  func getSessionUsage(since: Date?, until: Date?) async throws -> [SessionUsage]
  func getBlocksUsage(recent: Bool) async throws -> [SessionBlock]
  func startLiveMonitoring() -> AsyncStream<LiveBlockData>
  
  // Installation helpers
  func getInstallationPaths() -> [String]
  func verifyInstallation(at path: String) async -> Bool
}
```

#### Data Models

Parse ccusage JSON output into Swift models:

```swift
struct DailyUsageData: Codable {
  let type: String // "daily"
  let data: [DailyUsage]
  let summary: UsageSummary
}

struct SessionBlock: Codable {
  let blockStart: Date
  let blockEnd: Date
  let isActive: Bool
  let models: [String]
  let inputTokens: Int
  let outputTokens: Int
  let totalTokens: Int
  let costUSD: Double
  let burnRate: Int?
  let projectedTotal: Int?
  let projectedCost: Double?
}
```

#### Error Handling

- Handle ccusage not found gracefully
- Parse stderr for error messages
- Implement retry logic for transient failures
- Cache last successful results

### User Experience Flow

#### First Launch

- Check for ccusage in common locations
- Show setup wizard if not found
- Guide through installation with package manager detection

#### Normal Operation

- Poll ccusage periodically (configurable interval)
- Show current 5-hour block status in menu bar
- Display burn rate and projections
- Alert when approaching token limits

#### Live Monitoring

- Use ccusage blocks --live --json for real-time data
- Parse streaming JSON output
- Update UI with minimal latency
- Show progress bars and warnings

## Menu Bar Integration

### Status Display

- Show current token usage as percentage of typical limits
- Use color coding: green (< 60%), yellow (60-80%), red (> 80%)
- Display abbreviated cost (e.g., "$12.45")
- Indicate active session with animated icon

### Popup View

Quick glance information:
- Current 5-hour block usage
- Today's total cost
- Active models
- Time remaining in block
- Burn rate indicator

### Main Window

Detailed analytics matching ccusage reports:
- Daily/Monthly/Session views
- Model breakdown charts
- Cost projections
- Export functionality

## Architecture Patterns

### TCA Feature Structure

```
Features/
├── CCUsageDetection/     # Installation detection and setup
├── LiveMonitoring/       # Real-time usage tracking
├── UsageReports/         # Daily/Monthly/Session reports
├── Settings/             # Preferences and configuration
└── MenuBar/              # Menu bar UI and interactions
```

### Dependency Injection

- Use Dependencies library for all external interactions
- Mock ccusage responses for testing
- Implement live and test implementations

### State Management

- Single source of truth in TCA stores
- Computed properties for derived data
- Effects for async operations
- Proper cancellation handling

## UI/UX Guidelines

### Menu Bar Icon

- Use SF Symbols for consistency
- Show usage level visually (fill amount or color)
- Animate during active sessions
- Badge with cost if over threshold

### Color Scheme

- Follow system appearance (light/dark mode)
- Use semantic colors for adaptability
- Status colors: systemGreen, systemYellow, systemRed
- Accent color for branding

### Data Visualization

- Use SwiftUI Charts for usage graphs
- Show trends with sparklines
- Model breakdown as pie/donut charts
- Cost projections with confidence intervals

## Testing Strategy

### Unit Tests

- Test ccusage output parsing
- Verify cost calculations
- Test date filtering logic
- Mock Process execution

### Integration Tests

- Test actual ccusage execution
- Verify error handling
- Test installation detection
- Performance benchmarks

### UI Tests

- Test menu bar interactions
- Verify popup behavior
- Test settings persistence
- Accessibility compliance

## Performance Considerations

- Cache ccusage results to minimize calls
- Batch multiple queries when possible
- Use background queues for processing
- Implement progressive data loading
- Monitor memory usage with large datasets

## Common Issues and Solutions

### ccusage Not Found

- Check standard paths: /usr/local/bin, /opt/homebrew/bin, ~/.npm/bin
- Use which ccusage to find installation
- Verify PATH environment variable
- Handle permission issues gracefully

### Parsing Failures

- ccusage output format may change between versions
- Implement flexible JSON parsing
- Log raw output for debugging
- Fallback to basic functionality

### Performance Issues

- Large JSONL files can slow ccusage
- Implement pagination for long date ranges
- Use --since and --until to limit data
- Consider caching strategies

## Development Workflow

### Build and Test

```bash
# Kill existing instance
killall ccbar || true

# Build and run
xcodebuild -scheme ccbar build
open build/Release/ccbar.app
```

### Debug ccusage Integration

- Enable verbose logging
- Capture ccusage stderr
- Test with sample JSONL data
- Use ccusage debug mode

### Release Process

- Test with various ccusage versions
- Verify auto-update functionality
- Create signed and notarized build
- Update sparkle feed

## Future Enhancements

- Direct JSONL parsing (bypass ccusage)
- Cloud sync for multiple machines
- Cost alerts and budgets
- Team usage aggregation
- API integration for real-time data
- Widget extension for quick glance