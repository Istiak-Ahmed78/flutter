# Flutter Text Field Related Files - Complete Inventory

## Overview
This document lists all text field and text editing related files in the Flutter framework, organized by module and purpose.

**Total Files:** 27  
**Total Size:** ~953 KB  
**Modules:** 6 (Material, Cupertino, Widgets, Services, Painting, Rendering)

---

## 1. MATERIAL DESIGN TEXT FIELDS (9 files, ~210 KB)

### Core Text Input
| File | Size | Purpose |
|------|------|---------|
| **text_field.dart** | 76 KB | Main Material Design TextField widget with decoration, input styling, and Material-specific features |
| **text_form_field.dart** | 18 KB | TextField integrated with Form validation and auto-save capabilities |

### Theme & Styling
| File | Size | Purpose |
|------|------|---------|
| **text_button.dart** | 23 KB | Text button widget (not text field but text input related) |
| **text_button_theme.dart** | 4 KB | Theme configuration for text buttons |
| **text_theme.dart** | 33 KB | Material typography theme (font families, sizes, weights) |

### Text Selection & Toolbar
| File | Size | Purpose |
|------|------|---------|
| **text_selection.dart** | 12 KB | Text selection UI behavior (Material Design spec) |
| **text_selection_theme.dart** | 7 KB | Theme for text selection colors and styling |
| **text_selection_toolbar.dart** | 29 KB | Copy/paste/cut toolbar UI and positioning |
| **text_selection_toolbar_text_button.dart** | 6 KB | Individual toolbar button implementation |

---

## 2. CUPERTINO (iOS) TEXT FIELDS (6 files, ~172 KB)

### Core Text Input
| File | Size | Purpose |
|------|------|---------|
| **text_field.dart** | 75 KB | iOS-style CupertinoTextField widget with native iOS appearance |
| **text_form_field_row.dart** | 16 KB | iOS form field with label and row layout |

### Theme & Styling
| File | Size | Purpose |
|------|------|---------|
| **text_theme.dart** | 16 KB | iOS typography system fonts and sizes |

### Text Selection & Toolbar
| File | Size | Purpose |
|------|------|---------|
| **text_selection.dart** | 12 KB | iOS-style text selection behavior |
| **text_selection_toolbar.dart** | 45 KB | iOS-native style copy/paste toolbar |
| **text_selection_toolbar_button.dart** | 8 KB | iOS toolbar button styling |

---

## 3. PAINTING & TEXT STYLING (4 files, ~173 KB)

| File | Size | Purpose |
|------|------|---------|
| **text_painter.dart** | 73 KB | Low-level text rendering engine using dart:ui, handles measuring and drawing text |
| **text_style.dart** | 74 KB | TextStyle class - defines font family, size, weight, color, decoration, etc. |
| **text_span.dart** | 20 KB | InlineSpan/TextSpan for multi-style text (e.g., bold + regular in same line) |
| **text_scaler.dart** | 6 KB | TextScaler for responsive text scaling and accessibility |

---

## 4. RENDERING LAYER (2 files, ~267 KB)

| File | Size | Purpose |
|------|------|---------|
| **editable.dart** | 118 KB | RenderEditable - core rendering for editable text, handles cursor, selection, text layout |
| **paragraph.dart** | 149 KB | RenderParagraph - renders non-editable text paragraphs with layout metrics |

---

## 5. WIDGETS LAYER (1 file, ~280 KB)

| File | Size | Purpose |
|------|------|---------|
| **editable_text.dart** | 280 KB | EditableText widget - framework-level editable text controller (cursor, selection, input handling, focus) |

---

## 6. SERVICES LAYER (6 files, ~199 KB)

| File | Size | Purpose |
|------|------|---------|
| **text_input.dart** | 130 KB | Platform channel interface for text input (iOS/Android native communication) |
| **text_editing_delta.dart** | 23 KB | TextEditingDelta - represents changes to text (insert, delete, replace) |
| **text_formatter.dart** | 26 KB | TextInputFormatter - input validation and formatting logic |
| **text_editing.dart** | 10 KB | TextEditingValue & TextSelection - data models for text state |
| **text_boundary.dart** | 8 KB | TextBoundary - identifies word/line/document boundaries |
| **text_layout_metrics.dart** | 3 KB | TextLayoutMetrics - provides line/character metrics to platform |

---

## Architecture Layers

```
┌─────────────────────────────────────┐
│   Material / Cupertino Widgets      │  (text_field.dart, text_form_field.dart)
│   (High-level UI Components)        │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│   EditableText Widget               │  (widgets/editable_text.dart)
│   (Framework Core)                  │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│   Painting Layer                    │  (text_painter.dart, text_style.dart,
│   (Text Styling & Low-level Paint)  │   text_span.dart, text_scaler.dart)
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│   Rendering Layer                   │  (rendering/editable.dart, 
│   (RenderEditable, RenderParagraph) │   rendering/paragraph.dart)
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│   Services Layer                    │  (text_input.dart, text_editing.dart,
│   (Platform Communication)          │   text_formatter.dart, etc.)
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│   Platform Layer                    │  (iOS/Android native code)
│   (Native Text Input)               │
└─────────────────────────────────────┘
```

---

## Key Dependencies

### EditableText Widget
- Depends on: RenderEditable, TextEditingValue, TextInputFormatter, TextSelection
- Used by: Material TextField, Cupertino TextField, custom text inputs

### Material TextField
- Depends on: EditableText, InputDecorator, TextStyle, TextSelection
- Configuration: InputDecoration for borders, labels, hints, etc.

### Cupertino TextField
- Depends on: EditableText, TextStyle, CupertinoTheme
- Configuration: Minimal, native iOS appearance

### Text Rendering Pipeline
1. **TextStyle** - Defines visual properties
2. **TextPainter** - Measures and paints text
3. **RenderParagraph** - Handles layout
4. **RenderEditable** - Adds selection and cursor

### Text Input Pipeline
1. **TextField** - User input
2. **TextEditingValue** - Text state
3. **TextInputFormatter** - Validation/formatting
4. **TextEditingDelta** - Change tracking
5. **Platform Channel (text_input.dart)** - Native communication

---

## File Classifications

### By Frequency of Direct Use
**High** (developers often interact):
- `material/text_field.dart` - Material TextField
- `cupertino/text_field.dart` - iOS TextField
- `painting/text_style.dart` - TextStyle
- `services/text_formatter.dart` - Input formatting
- `services/text_editing.dart` - Text state

**Medium** (used in specific scenarios):
- `material/text_form_field.dart` - Form integration
- `painting/text_span.dart` - Rich text
- `painting/text_painter.dart` - Custom text rendering
- `services/text_input.dart` - Custom keyboard behavior

**Low** (internal framework use):
- `rendering/editable.dart` - Internal rendering
- `rendering/paragraph.dart` - Internal rendering
- `widgets/editable_text.dart` - Base widget
- `services/text_boundary.dart` - Accessibility/platform
- `services/text_editing_delta.dart` - Change tracking

### By Code Complexity
**Most Complex** (highest line count):
1. `widgets/editable_text.dart` (280 KB) - Manages all text editing state
2. `rendering/paragraph.dart` (149 KB) - Text layout algorithm
3. `rendering/editable.dart` (118 KB) - Editable text rendering
4. `services/text_input.dart` (130 KB) - Platform communication

**Moderate Complexity**:
- `painting/text_painter.dart` (73 KB)
- `painting/text_style.dart` (74 KB)
- `material/text_field.dart` (76 KB)
- `cupertino/text_field.dart` (75 KB)

---

## Related but Not Listed Here

Some text-related files not specifically listed:
- `widgets/text.dart` - Simple Text widget (non-editable)
- `widgets/selectable_text.dart` - Selectable Text widget
- `widgets/text_selection.dart` - Text selection controller
- `widgets/default_text_editing_shortcuts.dart` - Keyboard shortcuts
- `services/autofill.dart` - Autofill support
- `services/spell_check.dart` - Spell checking
- Selection toolbars in Material and Cupertino

---

## Next Steps for Learning

1. **Start with:** `material/text_field.dart` & `cupertino/text_field.dart`
2. **Then read:** `widgets/editable_text.dart` (core logic)
3. **Understand rendering:** `rendering/editable.dart` & `painting/text_painter.dart`
4. **Platform integration:** `services/text_input.dart`
5. **Advanced:** `rendering/paragraph.dart` for layout algorithm

