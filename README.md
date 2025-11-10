# Progressive Typing Trainer

A web-based typing trainer that teaches accurate typing through incremental mastery. Start with one letter and progressively add characters as you master each level.

## What It Is

This is a single-page HTML application that helps you learn to type accurately by:
- Starting with just the letter 'E'
- Adding one new character each time you complete a perfect run
- Focusing on accuracy first, speed second
- Providing real-time feedback on your performance

Unlike traditional typing tests that throw all keys at you at once, this trainer builds muscle memory gradually by introducing characters in order of English language frequency.

## Quick Start

1. Open `/Tools/Typing trainer/index.html` in any modern web browser
2. Press **Space** or **Enter** to start
3. Type the displayed text
4. Get 100% accuracy + meet WPM target → advance to next level
5. Your progress is automatically saved in the browser

## How It Works

### Character Progression

Characters are introduced in this order (based on English frequency):
1. **Letters (26 levels)**: `e t a o i n s h r d l c u m w f g y p b v k j x q z`
2. **Core punctuation (6 levels)**: `. , ' " ! ?`
3. **Numbers (10 levels)**: `1 2 3 4 5 6 7 8 9 0`
4. **Extended punctuation (18+ levels)**: `: ; - — ( ) [ ] / @ # $ % & * + =`

**Total: 60+ levels**

### Text Generation

- **Early levels (1-5 letters)**: Random character sequences with word-like spacing
- **Later levels (6+ letters)**: Real English words from built-in dictionary
- **New character emphasis**: Newest character appears more frequently (default: 4× more often)
- **Intelligent word wrapping**: Words never break mid-word across lines

### Advancement Rules

**To advance to the next level:**
- ✓ 100% accuracy (perfect typing, no errors)
- ✓ Meet WPM target (default: 10 WPM)

**Lives system:**
- Start each level with 3 lives (customizable 1-5)
- Lose a life when accuracy < 95% OR WPM < target
- If accuracy is between threshold and perfect, you can retry without losing a life
- Run out of lives → drop back one level, reset lives to max
- Successfully advance → reset lives to max

**Special case:** If you get 100% accuracy but miss the WPM target, you see "✓ Perfect! But need more speed..." and can retry without losing a life.

## User Interface

### Main Screen Layout

```
┌─────────────────────────────────────────┐
│  Progressive Typing Trainer    ⚙️  🌙   │
├─────────────────────────────────────────┤
│  Level 5 │ E T A O I              ❤❤❤   │
├─────────────────────────────────────────┤
│                                         │
│      [Text to type appears here]        │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │ 15 / 10 WPM  │  100%  │  0:12   │   │
│  │ 🔥 Nice pace!│Accuracy│  Time   │   │
│  ├─────────────────────────────────┤   │
│  │ Progress: ████████░░░░░░░░░░░   │   │
│  │ Errors: e a t                   │   │
│  └─────────────────────────────────┘   │
│                                         │
├─────────────────────────────────────────┤
│ [Space/Enter] Start  [Esc] Menu  [S]   │
└─────────────────────────────────────────┘
```

### Live Stats (During Typing)

- **WPM Counter**: Real-time words per minute with color coding
  - Green: Meeting or exceeding target
  - Orange: Within 5 WPM of target
  - Red: More than 5 WPM below target
  - Includes motivational messages ("🔥 Nice pace!", "🐌 Speed up!")

- **Accuracy Percentage**: Live accuracy with color coding
  - Green: 100% perfect
  - Orange: Above threshold (default 95%)
  - Red: Below threshold

- **Timer**: Elapsed time in MM:SS format

- **Progress Bar**: Visual indicator of test completion

- **Wrong Letters Tracker**: Shows actual letters you got wrong in order (e.g., "Errors: e t a e")

### Result Banner

After completing a test, a banner appears showing:
- 🎉 **Level Up!** - Perfect accuracy + met WPM target
- 💪 **Try Again** - Lost a life, try the level again
- 📉 **Level Down** - Ran out of lives, dropped to previous level
- ✓ **Perfect! But need more speed...** - 100% accuracy but didn't meet WPM

Press **Space** to continue to the next test.

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Space** or **Enter** | Start a test or continue after results |
| **Enter** (during test) | Force fail and restart immediately |
| **Enter** (in settings) | Apply settings and close panel |
| **Esc** | Return to welcome screen or close settings |
| **S** | Open/close settings |
| **D** | Toggle dark mode |

## Settings

Access settings by pressing **S** or clicking the ⚙️ icon.

### Customizable Options

| Setting | Range | Default | Description |
|---------|-------|---------|-------------|
| **Test Length** | 1-200 chars | 40 | Number of characters per test |
| **New Char Frequency** | 1-10× | 4× | How much more often new characters appear |
| **Lives per Level** | 1-5 | 3 | Number of attempts before dropping a level |
| **Target WPM** | 0-100 | 10 | Minimum WPM to advance (0 = no time limit) |
| **Accuracy Threshold** | 0-100% | 95% | Minimum accuracy to avoid losing a life |
| **Level Selector** | Any level | 1 | Jump to any level for practice |

**Applying Settings:**
- After changing settings, click **Apply Settings** (or press **Enter**) to apply changes
- The Apply button is always visible at the bottom of the settings panel
- If a test is active, applying settings will restart it with the new configuration
- Settings are automatically saved to browser localStorage and persist across sessions

## File Structure

```
/Tools/Typing trainer/
├── index.html              # Complete application (self-contained)
├── typing trainer.md       # Original specification
└── README.md              # This file
```

### Architecture Overview

The application is a **single HTML file** with inline CSS and JavaScript for maximum portability. No external dependencies or build process required.

**Code Organization (within index.html):**

1. **CSS Styles** (~500 lines)
   - Color theming (light/dark mode)
   - Responsive layout
   - Component styling (buttons, stats, progress bar, etc.)

2. **HTML Structure** (~200 lines)
   - Welcome screen
   - Training screen with live stats
   - Settings panel (slide-in from right)

3. **JavaScript** (~1000 lines)
   - Character progression constants
   - Built-in word dictionary (~80 common words)
   - State management
   - Text generation logic
   - Typing test logic
   - Live stats updates
   - Settings management
   - LocalStorage persistence
   - Event handlers

### Key JavaScript Functions

**Core Functions:**
- `startTest()` - Initialize a new typing test
- `handleKeyPress(char)` - Process typed characters
- `completeTest()` - Evaluate results and determine advancement
- `updateLiveStats()` - Update WPM, accuracy, progress bar, etc.

**Text Generation:**
- `generateText(level, length, newCharFreq)` - Main text generator
- `generateCharBasedText()` - For early levels (character sequences)
- `generateWordBasedText()` - For later levels (real words)
- `getAvailableChars(level)` - Get character set for a level

**UI Updates:**
- `renderTextDisplay()` - Render text with color coding
- `showInlineResults(result)` - Display result banner
- `updateInfoBar()` - Update level and lives display

**Settings:**
- `saveState()` - Persist to localStorage
- `loadState()` - Load from localStorage
- `updateSettingsUI()` - Sync UI with state

## Data Persistence

**What gets saved to localStorage:**
- Current level
- Maximum unlocked level
- All settings (test length, WPM, lives, etc.)

**What doesn't persist:**
- Current lives (reset to max on reload)
- In-progress test data
- Session statistics

**Storage key:** `typingTrainerState`

## Color Coding System

### WPM Indicator
- **Green**: On target or faster (≥ target WPM)
- **Orange**: Close to target (target - 5 WPM)
- **Red**: Too slow (< target - 5 WPM)

### Accuracy Indicator
- **Green**: 100% perfect
- **Orange**: Above threshold but not perfect
- **Red**: Below threshold

### Text Display
- **Green**: Correctly typed character
- **Red**: Incorrectly typed character (with light red background)
- **Gray**: Pending character (not yet typed)
- **Blue underline**: Current cursor position (animated blink)

## Design Philosophy

### Incremental Learning
Rather than overwhelming users with the full keyboard, the trainer introduces one character at a time. This allows muscle memory to develop naturally and prevents bad habits.

### Accuracy First, Speed Second
The requirement for 100% accuracy to advance ensures users develop correct finger placement before worrying about speed. The WPM requirement is set low (default 10 WPM) to avoid pressure.

### Frequency-Based Progression
Characters are introduced based on their frequency in English text. You learn 'E' before 'Z' because you'll use it far more often in real typing.

### Immediate Feedback
Real-time stats (WPM, accuracy, progress) help users self-correct during the test rather than finding out afterward.

### No Backspace by Design
Errors remain visible to discourage the "type fast and correct" habit. This encourages thinking before pressing keys.

## Common Use Cases

### 1. Complete Beginner
- Start at Level 1 (just 'E')
- Use default settings
- Focus on accuracy, ignore WPM initially (set to 0)
- Gradually build up through levels

### 2. Practicing Specific Characters
- Jump to any level using Settings → Level Selector
- Practice punctuation/numbers without doing all 26 letters first
- Adjust test length for focused drills (even 1 character tests)

### 3. Speed Training
- Set higher WPM targets (30-60)
- Keep accuracy threshold high (95-100%)
- Use longer test lengths (100-200 chars)

### 4. Pure Accuracy Training
- Set WPM target to 0 (no time pressure)
- Set accuracy threshold to 100%
- Take as long as needed per test

## Browser Compatibility

Works in all modern browsers:
- Chrome/Edge (v90+)
- Firefox (v88+)
- Safari (v14+)

**Requirements:**
- JavaScript enabled
- localStorage support (for saving progress)

## Customization & Extension Ideas

### Easy Modifications

**Change character order:**
Edit `CHAR_PROGRESSION` constant (line ~808):
```javascript
const CHAR_PROGRESSION = 'etaoinshrdlcumwfgypbvkjxqz...'.split('');
```

**Add more words to dictionary:**
Edit `WORD_DICTIONARY` array (line ~811):
```javascript
const WORD_DICTIONARY = ['the', 'and', ...];
```

**Adjust default settings:**
Edit `state.settings` object (line ~832):
```javascript
settings: {
    testLength: 40,
    newCharFreq: 4,
    // etc.
}
```

**Change color themes:**
Edit CSS variables in `:root` and `[data-theme="dark"]` (lines ~18-40)

### Advanced Extensions

**Statistics tracking:**
- Add a stats object to track total tests, average WPM, etc.
- Create a stats screen to visualize progress over time

**Custom word lists:**
- Allow users to upload/paste custom word lists
- Support different languages

**Lessons/exercises:**
- Pre-defined lesson plans
- Specific problem character drills

**Multiplayer:**
- Add WebSocket support for competitive typing
- Leaderboards

**Export/Import:**
- Export progress as JSON
- Share settings between devices

## Troubleshooting

### Progress not saving
- Check if browser allows localStorage
- Try a different browser
- Check browser's privacy settings (localStorage may be disabled)

### Layout issues
- Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)
- Clear browser cache
- Try a different browser

### Keys not registering
- Make sure settings panel is closed (press Esc)
- Check if another application is intercepting keys
- Ensure page has focus (click on it)

### WPM seems wrong
- Very short tests can show inflated WPM (try longer tests)
- WPM is calculated as: (characters typed / 5) / (time in minutes)
- Standard typing measurement: 5 characters = 1 word

## Credits & License

Created with Claude (Anthropic) in November 2024.

This is a personal project with no external dependencies. Feel free to modify, extend, or redistribute as needed.

## Version History

**v1.0** (November 2024)
- Initial release
- 60+ levels (letters, punctuation, numbers, extended)
- Real-time WPM/accuracy tracking
- Lives system with configurable difficulty
- Dark mode support
- localStorage persistence
- Fully keyboard-driven interface
- Single-file, zero-dependency architecture

---

**For AI Assistants:**

When making modifications to this project:
1. The entire application is in `index.html` (single file)
2. Use the Read tool to examine specific sections before editing
3. Test changes by opening index.html in a browser
4. Key sections to know:
   - State management: ~line 828
   - Text generation: ~line 870
   - Typing logic: ~line 1000
   - Live stats updates: ~line 1040
   - Settings UI: ~line 1270
5. Always preserve the single-file architecture
6. Follow existing code style and commenting patterns
7. Test in both light and dark modes
8. Verify keyboard shortcuts still work after changes
