---
description: iOS/SwiftUI Best Practices - Apple HIG Compliant Patterns
---

# iOS Best Practices Workflow

When implementing iOS features, ALWAYS use Apple's recommended patterns. Avoid custom workarounds.

## Keyboard Management

### Dismissal
- **Primary**: Use `.scrollDismissesKeyboard(.interactively)` on `ScrollView`
- **Explicit**: Use `ToolbarItemGroup(placement: .keyboard)` with a "Done" button
- **Focus Control**: Use `@FocusState` to programmatically manage focus

### Focus
- Use `@FocusState` with an enum for multi-field forms
- Set focus in `.onAppear` of the presenting view (not the parent)
- Use `.focused($focusedField, equals: .fieldName)` on each field

### Anti-Patterns (NEVER USE)
- ❌ `DispatchQueue.asyncAfter` for focus timing
- ❌ `UIApplication.shared.sendAction(#selector(UIResponder.resignFirstResponder)...)`
- ❌ Custom tap gestures to dismiss keyboard

## Navigation

- Use `NavigationStack` (NOT deprecated `NavigationView`)
- Use `navigationDestination(for:)` for programmatic navigation
- Use `.navigationTitle()` and `.navigationBarTitleDisplayMode()`

## Sheets & Modals

- Use `.sheet(isPresented:)` or `.sheet(item:)`
- Use `.presentationDetents([.medium, .large])` for half-sheets
- Isolate sheet content into separate `View` structs (component pattern)

## Forms

- Wrap form content in `ScrollView` for automatic keyboard avoidance
- Use `Form` for settings-style lists
- Use `.submitLabel(.next)` and `.submitLabel(.done)` for keyboard return key

## State Management

- Use `@State` for local view state
- Use `@Observable` (iOS 17+) for ViewModels
- Use `@Environment` for dependency injection (e.g., `AuthService`)

## Error Handling

- Use `.alert(isPresented:)` for error display
- Store error message in ViewModel (`var error: String?`)
- Use `.onChange(of: viewModel.error)` to trigger alert presentation

## Gestures

- Prefer built-in modifiers over custom `UIGestureRecognizer`
- Use `.onTapGesture`, `.onLongPressGesture`, etc.
- For swipe actions, use `.swipeActions()` on `List` rows

## Simulator Gotchas

- **Keyboard not showing**: Check I/O > Keyboard > Uncheck "Connect Hardware Keyboard" (⌘+Shift+K)
- **Pasteboard errors**: Normal in Simulator, ignore them
- **Haptic errors**: Normal in Simulator, ignore them
