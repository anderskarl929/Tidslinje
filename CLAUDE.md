# CLAUDE.md - AI Assistant Guide for Tidslinje

## Project Overview

**Tidslinje** (Swedish: "Timeline") is an interactive timeline visualization web application focused on the Medieval to Renaissance period (500-1500 CE). It's an educational tool for visualizing historical events with categorization and chronological display.

### Key Facts
- **Language**: Swedish (all UI text)
- **Tech Stack**: Pure vanilla JavaScript, HTML5, CSS3
- **Dependencies**: None (zero external libraries)
- **Architecture**: Single-file application (index.html)
- **Target Period**: 500-1500 CE (Medieval to Renaissance)

## Codebase Structure

### File Organization
```
/home/user/Tidslinje/
├── index.html                               # Main application (all code in one file)
├── Tidslinjer Mall.pdf                      # PDF design template reference
└── mindonmap-france-history-timeline.jpg    # Visual reference
```

### index.html Structure (528 lines)
| Section | Lines | Purpose |
|---------|-------|---------|
| **HTML Head** | 1-326 | Meta tags, embedded CSS |
| **CSS Styles** | 7-325 | All styling (embedded in `<style>`) |
| **HTML Body** | 327-369 | UI structure and layout |
| **JavaScript** | 371-527 | All application logic |

### Key JavaScript Functions
- **Lines 376-378**: Constants (`START_YEAR`, `END_YEAR`, `TOTAL_SPAN`)
- **Lines 383-395**: `createTimelineMarkers()` - Renders year markers
- **Lines 397-487**: `displayEvents()` - Main rendering function
- **Lines 489-507**: `addEvent()` - Event creation with validation
- **Lines 509-517**: `deleteEvent(id)` - Event deletion
- **Lines 519-527**: `clearAllEvents()` - Clear all with confirmation

## Development Workflows

### Running the Application

**Option 1: Direct browser open**
```bash
open /home/user/Tidslinje/index.html
# or
xdg-open /home/user/Tidslinje/index.html
```

**Option 2: Local server (recommended)**
```bash
# Python 3
python3 -m http.server 8000

# Node.js
npx serve .

# Then navigate to: http://localhost:8000
```

### Development Cycle
1. Edit `index.html`
2. Save changes
3. Refresh browser (Cmd/Ctrl+R)
4. No build step required

### Git Workflow
- **Development Branch**: `claude/claude-md-mi48x12nspk12zv2-01UQotZjo2PSA9FeXSc5UWM1`
- **Commit Pattern**: Descriptive messages in English
- **Push Command**: `git push -u origin <branch-name>`

## Key Conventions

### Code Style

#### JavaScript
- **Style**: ES6+ vanilla JavaScript
- **Quotes**: Single quotes for strings
- **Variable Naming**: camelCase
- **Constants**: UPPER_SNAKE_CASE
- **Functions**: Descriptive names (verb + noun)
- **Arrays**: Plural nouns (`events`, not `event`)

#### HTML
- **Indentation**: 2 spaces
- **Attributes**: Double quotes
- **IDs**: Kebab-case (`event-list`, `add-event-btn`)
- **Classes**: Kebab-case (`timeline-bar`, `event-box`)

#### CSS
- **Selector Order**: Element → Class → ID
- **Property Order**: Positioning → Box model → Typography → Visual → Other
- **Colors**: Hex values (uppercase preferred)
- **Units**: px for fixed, % for relative

### Naming Patterns
- **Functions**: `addEvent()`, `deleteEvent()`, `displayEvents()`
- **Event Handlers**: Inline or assigned to variables
- **DOM Elements**: Selected with `getElementById()`, `querySelector()`

### Color Palette (Design System)
```css
Primary Green:    #A8D08D  /* Buttons, highlights */
Secondary Green:  #8BBF6F  /* Hover states */
Light Green:      #C6E0B4  /* Timeline bar */
Background Green: #E8F5E0  /* Instruction boxes */
Accent Red:       #dc3545  /* Delete buttons */
Text Dark:        #333     /* Primary text */
Text Medium:      #555     /* Secondary text */
Text Light:       #666     /* Tertiary text */
Background:       #f5f5f5  /* Page background */
```

## Data Models

### Event Object Structure
```javascript
{
  id: Number,          // Unique identifier (Date.now() timestamp)
  year: Number,        // Year of event (500-1500 range)
  description: String, // Event description
  category: String     // One of: 'politisk', 'kulturell', 'religiös', 'teknisk', 'militär'
}
```

### Global State
```javascript
let events = [];               // Array of all events
let selectedCategory = 'politisk'; // Current category selection
```

### Timeline Constants
```javascript
const START_YEAR = 500;   // Timeline start year
const END_YEAR = 1500;    // Timeline end year
const TOTAL_SPAN = 1000;  // Year range (END_YEAR - START_YEAR)
```

## Common Tasks

### Adding New Features

#### 1. Adding a New Category
```javascript
// 1. Update HTML category selector (around line 343)
<button class="category-btn" data-category="ny-kategori">Ny Kategori</button>

// 2. Add CSS styling for the category (around line 194)
.event-box.ny-kategori .category-badge { background-color: #YOUR_COLOR; }

// 3. Update validation if needed (no changes required to JavaScript logic)
```

#### 2. Changing Timeline Range
```javascript
// Update constants (lines 376-378)
const START_YEAR = 1000;  // Change start year
const END_YEAR = 2000;    // Change end year
const TOTAL_SPAN = END_YEAR - START_YEAR;

// Update timeline markers (lines 383-395)
// Adjust marker positions based on new range
```

#### 3. Adding Data Persistence
```javascript
// Add to addEvent() after events.push()
localStorage.setItem('events', JSON.stringify(events));

// Add to page load (around line 524)
window.addEventListener('DOMContentLoaded', () => {
  const saved = localStorage.getItem('events');
  if (saved) {
    events = JSON.parse(saved);
    displayEvents();
  }
  createTimelineMarkers();
});

// Add to deleteEvent() and clearAllEvents()
localStorage.setItem('events', JSON.stringify(events));
```

### Modifying UI

#### Changing Colors
- **Timeline bar**: Line 148 (`.timeline-bar`)
- **Event boxes**: Lines 179-188 (`.event-box`)
- **Buttons**: Lines 231-253 (`.add-btn`, `.delete-btn`)
- **Category badges**: Lines 194-210 (`.category-badge`)

#### Adjusting Layout
- **Container width**: Line 33 (`.container`)
- **Timeline height**: Line 143 (`.timeline-section`)
- **Event positioning**: Lines 397-487 (`displayEvents()` function)
- **Responsive breakpoint**: Line 255 (`@media (max-width: 768px)`)

### Bug Fixes

#### Event Positioning Issues
- Check calculation in lines 461-462 (position calculation)
- Verify START_YEAR and END_YEAR constants
- Ensure TOTAL_SPAN is correctly calculated

#### Rendering Issues
- Verify `displayEvents()` clears properly (line 398)
- Check DOM element IDs match between HTML and JavaScript
- Ensure event sorting is correct (line 400)

## Important Notes & Gotchas

### Critical Limitations
1. **No Data Persistence**: Events are lost on page refresh
2. **Single File**: All code in one HTML file (consider modularizing for major features)
3. **No Build Process**: Cannot use TypeScript, SCSS, or modern JS features requiring transpilation
4. **No Testing**: No unit tests or integration tests

### Best Practices

#### When Editing
1. **Always test in browser** after changes
2. **Preserve Swedish language** for all UI text
3. **Maintain green color scheme** (matches PDF template)
4. **Keep functions pure** where possible
5. **Validate user inputs** (year range, required fields)

#### When Adding Features
1. **Keep it simple**: No external dependencies
2. **Mobile-first**: Test responsive design
3. **Accessibility**: Consider adding ARIA labels
4. **Performance**: Keep DOM manipulations minimal
5. **User feedback**: Add visual confirmations for actions

#### When Debugging
1. **Use browser DevTools**: Console for errors, Elements for CSS
2. **Check year calculations**: Most bugs relate to positioning
3. **Verify event array**: Log `events` array to inspect state
4. **Test edge cases**: Year boundaries (500, 1500), empty inputs

### Common Pitfalls

❌ **Don't:**
- Add npm packages (no package.json)
- Use JSX or TypeScript
- Assume data persists across refreshes
- Break the single-file structure without good reason
- Change UI language from Swedish to English

✅ **Do:**
- Keep code readable and well-commented
- Maintain consistent indentation
- Test responsive design (mobile + desktop)
- Validate all user inputs
- Use semantic HTML where possible

## Testing Checklist

### Manual Testing
- [ ] Add event with valid year (500-1500)
- [ ] Add event with invalid year (<500 or >1500)
- [ ] Add event with empty description
- [ ] Delete single event
- [ ] Clear all events (confirm dialog works)
- [ ] Switch between categories
- [ ] Test on mobile viewport (<768px)
- [ ] Verify events sort chronologically
- [ ] Check event positioning accuracy
- [ ] Test Enter key to add event

### Visual Testing
- [ ] Year markers display correctly (500-1500)
- [ ] Events alternate above/below timeline
- [ ] Triangle connectors point to correct year
- [ ] Category badges show correct colors
- [ ] Hover effects work on buttons and events
- [ ] Delete buttons appear on hover
- [ ] Responsive layout adapts on mobile

## Architecture Patterns

### MVC-Like Structure
- **Model**: `events` array (in-memory state)
- **View**: `displayEvents()`, `createTimelineMarkers()` (DOM rendering)
- **Controller**: `addEvent()`, `deleteEvent()`, event listeners (user interactions)

### Rendering Strategy
- **Full re-render on state change**: `displayEvents()` clears and rebuilds entire event list
- **Event-driven updates**: Actions trigger state change → re-render
- **No virtual DOM**: Direct DOM manipulation

### Position Calculation
```javascript
// Linear interpolation from year to percentage
const position = ((event.year - START_YEAR) / TOTAL_SPAN) * 100;
// Applied as: left: ${position}%
```

### Event Alternation
```javascript
// Even index = above timeline, odd index = below timeline
const isAbove = index % 2 === 0;
```

## Quick Reference

### Frequently Edited Sections

| Task | Lines | File |
|------|-------|------|
| Add/modify CSS styles | 7-325 | index.html |
| Change timeline constants | 376-378 | index.html |
| Modify event rendering | 397-487 | index.html |
| Update validation logic | 489-507 | index.html |
| Edit HTML structure | 327-369 | index.html |
| Change color palette | 7-325 | index.html (CSS) |

### Key Selectors (CSS/JavaScript)
```css
#year-input          /* Year input field */
#description-input   /* Description textarea */
#add-event-btn      /* Add event button */
.category-btn       /* Category selector buttons */
.timeline-bar       /* Main timeline bar */
.event-box          /* Individual event containers */
.category-badge     /* Category labels on events */
.delete-btn         /* Delete event buttons */
#event-list         /* Container for all events */
```

## Contact & Resources

### Reference Files
- **PDF Template**: `Tidslinjer Mall.pdf` - Visual design reference
- **Timeline Example**: `mindonmap-france-history-timeline.jpg` - Inspiration

### Git Information
- **Repository**: Tidslinje
- **Current Branch**: `claude/claude-md-mi48x12nspk12zv2-01UQotZjo2PSA9FeXSc5UWM1`
- **Main Branch**: Not set

---

**Last Updated**: 2025-11-18
**Version**: 1.0
**Status**: Active Development
