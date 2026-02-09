---
name: create-new-swift-oc-file
description: Enforce correct file creation conventions when creating new Swift (.swift) or Objective-C (.h/.m) files. Use this skill whenever Claude creates a new Swift or Objective-C source file, including classes, structs, enums, protocols, extensions, categories, or any other top-level type. Ensures correct file header (author, date, copyright), comment style, and class/struct section structure.
---

# Create New Swift / Objective-C File

When creating any new `.swift`, `.h`, or `.m` file, follow every rule below exactly.

## File Header

Every new file MUST begin with this header. Replace placeholders with real values:

```
//  {FILENAME}
//  Musically
//
//  Created by {USERNAME} on {DD/MM/YYYY}.
//  Copyright © {YYYY} Bytedance. All rights reserved.
```

- `{FILENAME}` — the actual file name including extension (e.g. `MyView.swift`, `MyManager.h`)
- `{USERNAME}` — resolve by running: first `git config user.name` (project-level), if empty then `git config --global user.name` (global). Use the first non-empty result.
- `{DD/MM/YYYY}` — today's date in day/month/year format
- `{YYYY}` — the current four-digit year

## Comment Rules

1. **Language** — All comments MUST be in English-US. Never use Chinese.
2. **Single-line comments** — Prefix with a contextually suitable emoji and semi-colon:
   ```swift
   // 📌: Single-line comment.
   ```
3. **Public API documentation** — Provide comprehensive docs for any public interface:

   Swift:
   ```swift
   /// Show sheet view, with full settings.
   /// - Parameters:
   ///   - view: View to show on SheetContainer, inherited from UIView.
   ///   - option: Designed for extensibility, defined in GBLSheetOption.
   ```

   Objective-C:
   ```objc
   /// Show sheet view, with full settings.
   /// @param view view will be shown on SheetContainer, inherited from UIView.
   /// @param option option is designed for extensibility, defined in GBLSheetOption.
   - (nullable id<GBLSheetController>)showView:(nullable UIView *)view withOption:(nullable GBLSheetOption *)option;
   ```

## Swift Class / Struct Structure

Use `// MARK: -` to organize sections in this exact order. Include an empty `//` line after each MARK header. Determine the superclass first, then apply the matching template.

### Normal Class (NSObject or no superclass)

No `_setupViews`. Initializer uses `override init()`.

```swift
class MyManager: NSObject {
    // MARK: - Constants
    //

    // MARK: - Props
    //

    // MARK: - Life Cycle
    //
    @objc public override init() {
        super.init()
        self._setup()
    }

    // MARK: - Public
    //

    // MARK: - Private
    //
    private func _setup() {
        //
    }

    // MARK: - Protocol {ProtocolName}
    //

    // MARK: - Override
    //
}
```

### UIView Subclass

Has `_setupViews`. Initializer uses `override init(frame: CGRect)`.

```swift
class MyView: UIView {
    // MARK: - Constants
    //

    // MARK: - Props
    //

    // MARK: - Views
    //

    // MARK: - Life Cycle
    //
    @objc public override init(frame: CGRect) {
        super.init(frame: frame)
        self._setup()
    }

    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    // MARK: - Public
    //

    // MARK: - Private
    //
    private func _setup() {
        self._setupViews()
    }

    private func _setupViews() {
        //
    }

    // MARK: - Protocol {ProtocolName}
    //

    // MARK: - Override
    //
}
```

### UIViewController Subclass

Has `_setupViews`. Setup called from `viewDidLoad`.

```swift
class MyViewController: UIViewController {
    // MARK: - Constants
    //

    // MARK: - Props
    //

    // MARK: - Views
    //

    // MARK: - Life Cycle
    //
    override func viewDidLoad() {
        super.viewDidLoad()
        self._setup()
    }

    // MARK: - Public
    //

    // MARK: - Private
    //
    private func _setup() {
        self._setupViews()
    }

    private func _setupViews() {
        //
    }

    // MARK: - Protocol {ProtocolName}
    //

    // MARK: - Override
    //
}
```

## Objective-C Class Structure

Use `#pragma mark -` to organize sections. Determine the superclass first, then apply the matching template.

### Normal Class (NSObject)

**Header (.h):**
```objc
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface MyManager : NSObject

#pragma mark - Public

@end

NS_ASSUME_NONNULL_END
```

**Implementation (.m):**
```objc
#import "MyManager.h"

@interface MyManager ()

#pragma mark - Props

@end

@implementation MyManager

#pragma mark - Life Cycle

- (instancetype)init {
    self = [super init];
    if (self) {
        [self _setup];
    }
    return self;
}

#pragma mark - Public

#pragma mark - Private

- (void)_setup {
    //
}

#pragma mark - Protocol {ProtocolName}

#pragma mark - Override

@end
```

### UIView Subclass

**Header (.h):**
```objc
#import <UIKit/UIKit.h>

NS_ASSUME_NONNULL_BEGIN

@interface MyView : UIView

#pragma mark - Public

@end

NS_ASSUME_NONNULL_END
```

**Implementation (.m):**
```objc
#import "MyView.h"

@interface MyView ()

#pragma mark - Props

@end

@implementation MyView

#pragma mark - Life Cycle

- (instancetype)initWithFrame:(CGRect)frame {
    self = [super initWithFrame:frame];
    if (self) {
        [self _setup];
    }
    return self;
}

#pragma mark - Public

#pragma mark - Private

- (void)_setup {
    [self _setupViews];
}

- (void)_setupViews {
    //
}

#pragma mark - Protocol {ProtocolName}

#pragma mark - Override

@end
```

### UIViewController Subclass

**Header (.h):**
```objc
#import <UIKit/UIKit.h>

NS_ASSUME_NONNULL_BEGIN

@interface MyViewController : UIViewController

#pragma mark - Public

@end

NS_ASSUME_NONNULL_END
```

**Implementation (.m):**
```objc
#import "MyViewController.h"

@interface MyViewController ()

#pragma mark - Props

@end

@implementation MyViewController

#pragma mark - Life Cycle

- (void)viewDidLoad {
    [super viewDidLoad];
    [self _setup];
}

#pragma mark - Public

#pragma mark - Private

- (void)_setup {
    [self _setupViews];
}

- (void)_setupViews {
    //
}

#pragma mark - Protocol {ProtocolName}

#pragma mark - Override

@end
```

## Quick Reference

### Class Type Decision

| Superclass | Initializer | Has `_setupViews`? | Has Views section? |
|------------|-------------|--------------------|--------------------|
| NSObject / none | `override init()` | No | No |
| UIView | `override init(frame:)` | Yes | Yes |
| UIViewController | `viewDidLoad()` | Yes | Yes |

### Section Order

| # | Section | Swift | Objective-C |
|---|---------|-------|-------------|
| 1 | Constants | `// MARK: - Constants` | `#pragma mark - Constants` |
| 2 | Props | `// MARK: - Props` | `#pragma mark - Props` |
| 3 | Views | `// MARK: - Views` | `#pragma mark - Views` |
| 4 | Life Cycle | `// MARK: - Life Cycle` | `#pragma mark - Life Cycle` |
| 5 | Public | `// MARK: - Public` | `#pragma mark - Public` |
| 6 | Private | `// MARK: - Private` | `#pragma mark - Private` |
| 7 | Protocol | `// MARK: - Protocol X` | `#pragma mark - Protocol X` |
| 8 | Override | `// MARK: - Override` | `#pragma mark - Override` |
