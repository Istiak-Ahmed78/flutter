# Text Field Files - Quick Reference Guide

## 📋 Complete File Listing

### Material Design (9 files)
```
packages/flutter/lib/src/material/
  ✓ text_field.dart                    76 KB  Main Material TextField widget
  ✓ text_form_field.dart               18 KB  Form-integrated TextField
  ✓ text_button.dart                   23 KB  Text button
  ✓ text_button_theme.dart              4 KB  Text button theme
  ✓ text_theme.dart                    33 KB  Material typography system
  ✓ text_selection.dart                12 KB  Text selection (Material)
  ✓ text_selection_theme.dart           7 KB  Selection styling
  ✓ text_selection_toolbar.dart        29 KB  Copy/Paste toolbar
  ✓ text_selection_toolbar_text_button.dart  6 KB  Toolbar button
```

### Cupertino/iOS (6 files)
```
packages/flutter/lib/src/cupertino/
  ✓ text_field.dart                    75 KB  iOS-style CupertinoTextField
  ✓ text_form_field_row.dart           16 KB  iOS form field row
  ✓ text_theme.dart                    16 KB  iOS typography
  ✓ text_selection.dart                12 KB  Text selection (iOS)
  ✓ text_selection_toolbar.dart        45 KB  iOS selection toolbar
  ✓ text_selection_toolbar_button.dart  8 KB  iOS toolbar button
```

### Framework Core (1 file)
```
packages/flutter/lib/src/widgets/
  ✓ editable_text.dart                280 KB  ⭐ Core EditableText widget
```

### Rendering Engine (2 files)
```
packages/flutter/lib/src/rendering/
  ✓ editable.dart                     118 KB  RenderEditable rendering
  ✓ paragraph.dart                    149 KB  RenderParagraph layout
```

### Painting & Styling (4 files)
```
packages/flutter/lib/src/painting/
  ✓ text_painter.dart                  73 KB  Text measure & paint
  ✓ text_style.dart                    74 KB  TextStyle class
  ✓ text_span.dart                     20 KB  InlineSpan/TextSpan
  ✓ text_scaler.dart                    6 KB  Text scaling
```

### Platform Integration (6 files)
```
packages/flutter/lib/src/services/
  ✓ text_input.dart                   130 KB  Platform text input channels
  ✓ text_editing.dart                  10 KB  TextEditingValue & TextSelection
  ✓ text_editing_delta.dart            23 KB  Text change tracking
  ✓ text_formatter.dart                26 KB  Input validation/formatting
  ✓ text_boundary.dart                  8 KB  Word/line boundaries
  ✓ text_layout_metrics.dart            3 KB  Layout metrics
```

---

## 🎯 Where to Start

### I want to...

#### ...use TextField in my app
**Read:** `material/text_field.dart`  
**Learn from:** Flutter documentation + examples  

#### ...understand how TextField works
**Read in order:**
1. `material/text_field.dart` (UI layer)
2. `widgets/editable_text.dart` (core logic)
3. `rendering/editable.dart` (rendering)
4. `services/text_input.dart` (platform)

#### ...format/validate input
**Read:** `services/text_formatter.dart`  
**Also see:** `material/text_form_field.dart`

#### ...customize text appearance
**Read:** `painting/text_style.dart` + `painting/text_span.dart`  
**Also see:** `material/text_theme.dart`

#### ...render custom text
**Read:** `painting/text_painter.dart`  
**Also see:** `rendering/paragraph.dart`

#### ...handle text selection
**Read:** `rendering/editable.dart`  
**Also see:** `services/text_editing.dart`

#### ...build iOS text fields
**Read:** `cupertino/text_field.dart`  

#### ...integrate with native IME
**Read:** `services/text_input.dart`

#### ...add spell check
**See:** `services/spell_check.dart` (not in main list)

#### ...implement rich text
**Read:** `painting/text_span.dart`  
**Also see:** `rendering/paragraph.dart`

---

## 📊 File Size Ranking

```
1. editable_text.dart           280 KB  ⭐⭐⭐ Most important
2. text_input.dart              130 KB  ⭐⭐⭐ Platform integration
3. paragraph.dart               149 KB  ⭐⭐  Text layout
4. editable.dart                118 KB  ⭐⭐  Text rendering
5. material/text_field.dart      76 KB  ⭐⭐  Material UI
6. cupertino/text_field.dart     75 KB  ⭐⭐  iOS UI
7. text_painter.dart             73 KB  ⭐⭐  Text painting
8. text_style.dart               74 KB  ⭐⭐  Styling
9. text_selection_toolbar.dart   29 KB  ⭐   UI component
10. text_formatter.dart          26 KB  ⭐   Validation
```

---

## 🏗️ Typical Code Flow

```
User Types
    ↓
Platform (iOS/Android IME)
    ↓
text_input.dart (SystemChannels.textInput)
    ↓
editable_text.dart (_commitValue)
    ↓
text_formatter.dart (formatEditUpdate)
    ↓
text_editing.dart (TextEditingValue updated)
    ↓
editable.dart (RenderEditable.paint)
    ↓
text_painter.dart (TextPainter.paint)
    ↓
Screen Updated
```

---

## 🔑 Key Classes

| Class | File | Purpose |
|-------|------|---------|
| **TextField** | material/text_field.dart | Material text input |
| **CupertinoTextField** | cupertino/text_field.dart | iOS text input |
| **EditableText** | widgets/editable_text.dart | Core editing widget |
| **RenderEditable** | rendering/editable.dart | Editable rendering |
| **TextPainter** | painting/text_painter.dart | Text rendering |
| **TextStyle** | painting/text_style.dart | Text styling |
| **TextEditingValue** | services/text_editing.dart | Text state |
| **TextEditingController** | services/text_input.dart | Text controller |
| **TextInputFormatter** | services/text_formatter.dart | Input validation |
| **TextSelection** | services/text_editing.dart | Selection state |

---

## 💡 Pro Tips

### Tip 1: EditableText is the Core
Everything ultimately depends on `widgets/editable_text.dart`. If you want to understand text editing, understand this file first.

### Tip 2: Material vs Cupertino
- `material/text_field.dart` = High-level Material UI
- `cupertino/text_field.dart` = High-level iOS UI  
- Both wrap `EditableText` and add styling

### Tip 3: Rendering Pipeline
The text rendering happens in:
1. `text_style.dart` (what style to use)
2. `text_painter.dart` (measure and paint)
3. `editable.dart` (add cursor/selection)
4. Platform (display on screen)

### Tip 4: Input Validation
Use `services/text_formatter.dart` to validate/format input as user types. This runs before `TextEditingValue` is updated.

### Tip 5: Rich Text Styling
Use `text_span.dart` with `TextSpan` children to compose styled text. `RenderParagraph` handles the layout.

### Tip 6: Platform Communication
`services/text_input.dart` is the bridge between Flutter and native IME. It uses SystemChannels to communicate.

### Tip 7: Text Selection State
`TextSelection` tracks cursor and selection:
- `isCollapsed` = cursor only (no selection)
- `base` = start position
- `extent` = end position

### Tip 8: Controller Pattern
`TextEditingController` extends `ValueNotifier<TextEditingValue>`. Always call `.dispose()` when done.

---

## 📈 Dependency Graph (Simplified)

```
TextField (Material)
└─ EditableText ⭐
   ├─ RenderEditable
   │  ├─ TextPainter
   │  │  ├─ TextStyle
   │  │  ├─ TextSpan
   │  │  └─ dart:ui
   │  └─ RenderParagraph
   │     └─ TextPainter
   ├─ TextEditingController
   │  └─ TextEditingValue
   ├─ TextInputFormatter
   ├─ TextSelection
   └─ SystemChannels.textInput
      └─ Native Platform

CupertinoTextField (iOS)
└─ EditableText (same as above)
```

---

## 🔍 Search Tips

When reading the code:

- **"EditableText"** - Find main widget class definition
- **"RenderEditable"** - Find rendering logic, cursor, selection
- **"TextPainter"** - Find text measurement/painting
- **"_commitValue"** - Find input processing pipeline
- **"formatEditUpdate"** - Find where formatters are applied
- **"_handleSelectionChange"** - Find selection handling
- **"getOffsetForCaret"** - Find cursor positioning
- **"getPositionForOffset"** - Find offset-to-position conversion
- **"TextInputConnection"** - Find platform communication

---

## 📚 Reading Order (Recommended)

### Beginner (Want to use text fields)
1. Material TextField documentation
2. `material/text_field.dart` (overview only)
3. `services/text_formatter.dart` (for formatting)

### Intermediate (Want to understand how it works)
1. `material/text_field.dart` (full read)
2. `widgets/editable_text.dart` (main widget logic)
3. `services/text_input.dart` (platform integration)
4. `painting/text_style.dart` (styling)

### Advanced (Want to modify/extend)
1. `widgets/editable_text.dart` (complete)
2. `rendering/editable.dart` (rendering engine)
3. `painting/text_painter.dart` (painting)
4. `services/text_input.dart` (platform channels)
5. `rendering/paragraph.dart` (layout algorithm)

### Expert (Want to understand everything)
Read all 27 files in order:
1. Services (platform layer) → text_input.dart, text_editing.dart
2. Painting (styling) → text_style.dart, text_span.dart, text_painter.dart
3. Widgets (framework) → editable_text.dart
4. Rendering → paragraph.dart, editable.dart
5. Material → text_field.dart, text_form_field.dart, decorators
6. Cupertino → text_field.dart, variants

---

## ⚡ Quick Access

**Material TextField:** `packages/flutter/lib/src/material/text_field.dart`  
**Cupertino TextField:** `packages/flutter/lib/src/cupertino/text_field.dart`  
**Core Widget:** `packages/flutter/lib/src/widgets/editable_text.dart`  
**Platform Bridge:** `packages/flutter/lib/src/services/text_input.dart`  
**Text Rendering:** `packages/flutter/lib/src/painting/text_painter.dart`  

---

## 🎓 Learning Outcomes

After studying these files, you will understand:

- ✅ How TextField widget works internally
- ✅ How text is rendered on screen
- ✅ How user input is processed and validated
- ✅ How text selection works
- ✅ How styling is applied to text
- ✅ How Flutter communicates with native IME
- ✅ How to create custom text widgets
- ✅ How to format/validate input
- ✅ How rich text works
- ✅ Entire text editing pipeline

---

## 📞 Still Need Help?

If looking for:
- **Text metrics/layout** → `text_painter.dart`, `paragraph.dart`
- **Cursor/selection** → `rendering/editable.dart`, `text_editing.dart`
- **Keyboard/IME** → `services/text_input.dart`, `services/text_formatter.dart`
- **Platform-specific UI** → `material/text_field.dart` or `cupertino/text_field.dart`
- **Theme/styling** → `material/text_theme.dart`, `painting/text_style.dart`
- **Input validation** → `services/text_formatter.dart`, `material/text_form_field.dart`
- **Rich text** → `text_span.dart`, `paragraph.dart`

