# Flutter Text Field Architecture & Dependencies

## Class Hierarchy Overview

```
Widget (framework.dart)
  └── EditableText (widgets/editable_text.dart) ⭐ CORE
      ├── Extends: StatefulWidget
      ├── Creates: _EditableTextState
      ├── Renders: RenderEditable
      └── Uses:
          ├── TextEditingController
          ├── TextInputType
          ├── TextInputFormatter[]
          ├── TextSelection
          ├── TextEditingValue
          └── TextAlign, TextDirection

  └── TextField (material/text_field.dart) ⭐ HIGH LEVEL
      ├── Wraps: EditableText
      ├── Provides: InputDecoration (borders, labels, hints, icons)
      ├── Uses: TextEditingController
      └── Related: TextFormField (with Form integration)

  └── CupertinoTextField (cupertino/text_field.dart) ⭐ HIGH LEVEL
      ├── Wraps: EditableText
      ├── Style: iOS native appearance
      └── Related: CupertinoTextFormFieldRow


RenderObject (object.dart)
  └── RenderBox (box.dart)
      └── RenderEditable (rendering/editable.dart) ⭐ CORE RENDERING
          ├── Extends: RenderBox with ContainerRenderObjectMixin
          ├── Renders: Text with cursor and selection
          ├── Uses: TextPainter
          ├── Manages: Cursor position, selection, scrolling
          └── Paints: Text content, selection highlight, cursor

      └── RenderParagraph (rendering/paragraph.dart) ⭐ TEXT LAYOUT
          ├── Extends: RenderBox
          ├── Renders: Formatted text
          ├── Uses: TextPainter
          └── Layout: Multi-line text with styles


TextStyle (painting/text_style.dart) ⭐ STYLING
  ├── Properties:
  │   ├── color, backgroundColor
  │   ├── fontSize, fontWeight, fontStyle
  │   ├── fontFamily, fontFamilyFallback
  │   ├── decoration (underline, line-through)
  │   ├── decorationColor, decorationThickness
  │   ├── letterSpacing, wordSpacing
  │   └── ... (30+ properties)
  └── Usage: Applied to text spans, entire text, etc.

TextSpan (painting/text_span.dart) ⭐ RICH TEXT
  ├── text: String
  ├── style: TextStyle
  ├── children: List<InlineSpan>
  └── Usage: Compose styled text from multiple spans

TextPainter (painting/text_painter.dart) ⭐ PAINTING
  ├── text: InlineSpan
  ├── Methods:
  │   ├── layout(maxWidth, minWidth, textScaler)
  │   ├── paint(canvas, offset)
  │   ├── getOffsetForCaret(TextPosition)
  │   ├── getPositionForOffset(Offset)
  │   └── ... (measuring, text metrics)
  └── Usage: Measure and render text


TextEditingValue (services/text_editing.dart) ⭐ STATE MODEL
  ├── text: String
  ├── selection: TextSelection
  ├── composing: TextRange
  └── Methods:
      ├── copyWith()
      ├── replaced()
      └── isComposingRangeValid

TextEditingController (services/text_input.dart)
  ├── Extends: ValueNotifier<TextEditingValue>
  ├── Properties:
  │   ├── text: String
  │   ├── value: TextEditingValue
  │   ├── selection: TextSelection
  │   └── runtimeType
  └── Methods:
      ├── clear()
      ├── buildTextEditingValue()
      ├── buildSemantics()
      └── addListener(), removeListener()


TextInputFormatter (services/text_formatter.dart) ⭐ VALIDATION
  ├── Abstract method: formatEditUpdate()
  ├── Implementations:
  │   ├── FilteringTextInputFormatter (allowed/denied chars)
  │   ├── LengthLimitingTextInputFormatter (max length)
  │   ├── UpperCaseTextInputFormatter
  │   └── Custom formatters
  └── Pipeline: All formatters applied in order


TextSelection (services/text_editing.dart) ⭐ SELECTION STATE
  ├── base: int (start)
  ├── extent: int (end)
  ├── isValid: bool
  ├── isCollapsed: bool (cursor position, not a range)
  └── Methods:
      ├── getInfo()
      ├── copyWith()
      ├── expandTo()
      └── ... (selection manipulation)


TextEditingDelta (services/text_editing_delta.dart)
  ├── Represents a change to text
  ├── Subtypes:
  │   ├── TextEditingDeltaInsertion
  │   ├── TextEditingDeltaDeletion
  │   ├── TextEditingDeltaReplacement
  │   └── TextEditingDeltaNonTextUpdate
  └── Usage: Track every keystroke, IME change


InputDecoration (material/input_decorator.dart) ⭐ MATERIAL DECORATION
  ├── label, hint, helper, error text
  ├── prefixIcon, suffixIcon
  ├── border, enabledBorder, focusedBorder
  ├── contentPadding
  ├── fillColor, filled
  └── 30+ styling properties


TextInputType (services/text_input.dart)
  ├── text, multiline, number, phone, datetime, emailAddress, url, etc.
  └── Usage: Hints to platform (which keyboard to show)

TextInputAction (services/text_input.dart)
  ├── none, done, go, search, send, next, previous, continueAction, join, route, emergencyCall, newline
  └── Usage: Configures keyboard return button

TextSelection (services/text_input.dart)
  ├── Manages: current cursor position and selected range
  └── Methods: expansion, collapse, extents, etc.


## Data Flow for Text Editing

### Initialization
```
TextField created
    ↓
TextEditingController created
    ↓
TextEditingValue initialized { text: "", selection: TextSelection.collapsed(0) }
    ↓
EditableText created (wraps controller)
    ↓
RenderEditable created
    ↓
TextPainter created
```

### User Types Character
```
1. Platform sends character to Flutter
   ↓
2. TextInputConnection receives it
   ↓
3. EditableText._commitValue() called
   ↓
4. TextInputFormatter applied (each one in order)
   ↓
5. TextEditingValue updated with new text
   ↓
6. TextEditingController notifies listeners
   ↓
7. EditableText rebuilds
   ↓
8. RenderEditable repaints
   ↓
9. TextPainter renders updated text with cursor
```

### Text Selection
```
User selects text (tap + drag)
    ↓
Gesture recognizer detects
    ↓
RenderEditable._handleSelectionChange() called
    ↓
TextSelection updated (base, extent)
    ↓
TextEditingValue updated
    ↓
RenderEditable repaints
    ↓
Selection highlight + toolbar shown
```

### Platform Communication
```
EditableText
    ↓
TextInputConnection (via SystemChannels.textInput)
    ↓
text_input.dart (method channel)
    ↓
Native code (iOS/Android IME)
    ↓
Platform sends updates back
    ↓
EditableText receives and processes
```


## Key Classes & Their Locations

| Class | Location | Purpose |
|-------|----------|---------|
| **EditableText** | widgets/editable_text.dart | Core editable widget |
| **RenderEditable** | rendering/editable.dart | Rendering logic |
| **RenderParagraph** | rendering/paragraph.dart | Text paragraph layout |
| **TextPainter** | painting/text_painter.dart | Measure & paint text |
| **TextStyle** | painting/text_style.dart | Text styling |
| **TextSpan** | painting/text_span.dart | Styled text span |
| **TextEditingValue** | services/text_editing.dart | Text state |
| **TextEditingController** | services/text_input.dart | Text state manager |
| **TextInputFormatter** | services/text_formatter.dart | Input validation |
| **TextSelection** | services/text_editing.dart | Selection state |
| **TextField** | material/text_field.dart | Material input widget |
| **CupertinoTextField** | cupertino/text_field.dart | iOS input widget |
| **InputDecoration** | material/input_decorator.dart | Material styling |
| **TextInputType** | services/text_input.dart | Keyboard type |


## File Dependencies Map

```
material/text_field.dart
  ├─ imports: widgets/editable_text.dart
  ├─ imports: material/input_decorator.dart
  ├─ imports: painting/text_style.dart
  ├─ imports: services/text_formatting.dart
  └─ imports: services/text_input.dart

cupertino/text_field.dart
  ├─ imports: widgets/editable_text.dart
  ├─ imports: painting/text_style.dart
  ├─ imports: services/text_input.dart
  └─ imports: cupertino/theme.dart

widgets/editable_text.dart ⭐ CENTRAL HUB
  ├─ imports: rendering/editable.dart
  ├─ imports: services/text_editing.dart
  ├─ imports: services/text_input.dart
  ├─ imports: services/text_formatter.dart
  ├─ imports: painting/text_painter.dart
  ├─ imports: painting/text_style.dart
  ├─ imports: gestures/*
  └─ imports: foundation/*

rendering/editable.dart
  ├─ imports: painting/text_painter.dart
  ├─ imports: services/text_editing.dart
  ├─ imports: rendering/paragraph.dart
  └─ imports: painting/*

rendering/paragraph.dart
  ├─ imports: painting/text_painter.dart
  ├─ imports: painting/text_style.dart
  └─ imports: painting/text_span.dart

painting/text_painter.dart
  ├─ imports: painting/text_style.dart
  ├─ imports: painting/text_span.dart
  ├─ imports: dart:ui (engine)
  └─ imports: services/text_editing.dart

services/text_input.dart
  ├─ implements: Platform channel interface
  ├─ uses: SystemChannels
  └─ communicates: iOS/Android native code
```


## Layered Architecture

```
LAYER 1: HIGH-LEVEL WIDGETS (Material/Cupertino)
├── TextField (material)
├── TextFormField (material)
├── CupertinoTextField (cupertino)
└── CupertinoTextFormFieldRow (cupertino)
└── Input styling, decoration, validation

LAYER 2: FRAMEWORK WIDGETS
├── EditableText ⭐ Core logic
├── Text (read-only)
├── SelectableText (selectable read-only)
└── EditableText state management

LAYER 3: PAINTING (Styling & Metrics)
├── TextStyle (appearance)
├── TextSpan (rich text)
├── TextPainter (measure & paint)
├── TextScaler (scaling)
└── TextLayout helpers

LAYER 4: RENDERING (Render tree)
├── RenderEditable (editable text rendering)
├── RenderParagraph (paragraph rendering)
├── RenderBox (basic box layout)
└── Cursor, selection, scrolling

LAYER 5: SERVICES (Platform integration)
├── TextEditingController (state)
├── TextEditingValue (data model)
├── TextInputFormatter (validation)
├── TextSelection (selection state)
├── TextEditingDelta (change tracking)
└── TextInputType, TextInputAction

LAYER 6: PLATFORM CHANNELS (Native communication)
├── text_input.dart (method channels)
├── SystemChannels.textInput
└── iOS/Android IME integration
```


## Common Use Cases & Which Files to Look At

### 1. Create a TextField
Files: `material/text_field.dart`, `services/text_input.dart`
```dart
TextField(
  controller: TextEditingController(),
  decoration: InputDecoration(labelText: 'Name'),
)
```

### 2. Validate Input
Files: `services/text_formatter.dart`, `material/text_form_field.dart`
```dart
TextFormField(
  validator: (value) => value?.isEmpty ?? true ? 'Required' : null,
)
```

### 3. Format Input (e.g., phone number)
Files: `services/text_formatter.dart`
```dart
inputFormatters: [
  FilteringTextInputFormatter.allow(RegExp(r'[0-9]')),
  LengthLimitingTextInputFormatter(10),
]
```

### 4. Rich Text (multiple styles)
Files: `painting/text_span.dart`, `painting/text_painter.dart`, `rendering/paragraph.dart`
```dart
Text.rich(
  TextSpan(children: [
    TextSpan(text: 'Bold', style: TextStyle(fontWeight: FontWeight.bold)),
    TextSpan(text: ' Normal'),
  ])
)
```

### 5. Custom Text Rendering
Files: `painting/text_painter.dart`, `rendering/paragraph.dart`
```dart
CustomPaint(
  painter: MyTextPainter(),
)
```

### 6. Handle Text Changes
Files: `widgets/editable_text.dart`, `services/text_editing.dart`
```dart
TextEditingController()..addListener(() {
  print(controller.text);
})
```

### 7. Text Selection Handling
Files: `rendering/editable.dart`, `services/text_editing.dart`
```dart
TextField(
  onChanged: (value) {
    // User text changed
  },
)
```

### 8. Custom Formatter (e.g., currency)
Files: `services/text_formatter.dart`
```dart
class CurrencyFormatter extends TextInputFormatter {
  @override
  TextEditingValue formatEditUpdate(
    TextEditingValue oldValue,
    TextEditingValue newValue,
  ) {
    // Custom formatting logic
  }
}
```

