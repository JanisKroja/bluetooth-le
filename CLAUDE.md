# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the `@capacitor-community/bluetooth-le` Capacitor plugin that provides Bluetooth Low Energy functionality for Capacitor apps across web, Android, and iOS platforms. The plugin follows the Web Bluetooth API as a guideline and only supports the central role of BLE (not peripheral).

## Architecture

### Platform Structure
- **TypeScript Source** (`src/`): Core plugin interface and client wrapper
- **Android Implementation** (`android/`): Native Android BLE implementation in Kotlin  
- **iOS Implementation** (`ios/`): Native iOS BLE implementation in Swift
- **Web Implementation**: Uses Web Bluetooth API when available

### Key Files
- `src/bleClient.ts`: Main client wrapper class - this is the recommended API for users
- `src/definitions.ts`: TypeScript interfaces and enums defining the plugin API
- `src/plugin.ts`: Raw Capacitor plugin interface (not recommended for direct use)
- `src/conversion.ts`: Data conversion utilities (DataView, hex strings, etc.)
- `src/queue.ts`: Operation queue management for BLE operations
- `src/validators.ts`: UUID and data validation utilities

### Client Architecture
The plugin provides two interfaces:
1. `BleClient` - High-level wrapper class (recommended)
2. `BluetoothLe` - Low-level plugin interface (not recommended for direct use)

## Development Commands

**WSL Environment Note**: When running in WSL, execute npm/node commands using `powershell.exe` to run in the host Windows context for better compatibility with Node.js tooling and file system operations.

Example: `powershell.exe npm run build`

### Build & Test
- `npm run build` - Clean, generate docs, compile TypeScript, and bundle
- `npm run test` - Run Jest tests  
- `npm run test:coverage` - Run tests with coverage
- `npm run test:watch` - Run tests in watch mode

### Platform Verification  
- `npm run verify` - Verify all platforms (iOS, Android, web)
- `npm run verify:ios` - Build and test iOS implementation
- `npm run verify:android` - Build and test Android implementation  
- `npm run verify:web` - Run web tests and build

### Code Quality
- `npm run lint` - Run ESLint, Prettier, and Swift lint
- `npm run fmt` - Auto-fix linting issues
- `npm run eslint` - ESLint for TypeScript
- `npm run prettier` - Prettier for code formatting
- `npm run swiftlint` - SwiftLint for iOS code

### Documentation
- `npm run docgen` - Generate API documentation from JSDoc comments
- Output goes to README.md and dist/docs.json

## Important Development Notes

### Platform-Specific Considerations
- **Android**: Uses MAC addresses as device IDs, supports bonding, location permissions required for BLE scanning
- **iOS**: Uses system-generated identifiers as device IDs, bonding handled by OS, background modes may be needed
- **Web**: Limited by browser support, uses generated identifiers, some features behind flags

### Data Handling
- Use `DataView` for binary data across all platforms
- Helper functions in `conversion.ts` for DataView ↔ hex string conversion
- `numbersToDataView()` utility for creating DataView from number arrays

### Queue Management
BLE operations are queued to prevent race conditions. The queue system in `queue.ts` ensures operations execute sequentially per device.

### UUID Format
All UUIDs must be 128-bit format strings. Use `numberToUUID()` helper to convert 16-bit service IDs.

### Testing Strategy
- Unit tests for utilities and client wrapper
- Platform-specific verification builds
- No native unit tests - verification via example apps recommended