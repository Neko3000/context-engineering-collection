
## Architecture Principles

**This is always a feature branch:**
- Delete old code completely - no deprecation needed
- No versioned names (processV2, handleNew, ClientOld)
- No migration code unless explicitly requested
- No "removed code" comments - just delete it

**Prefer explicit over implicit:**
- Clear function names over clever abstractions
- Obvious data flow over hidden magic
- Direct dependencies over service locators

## Comments

**Styles**
- Always create comments in English-US, do not use Chinese.
- Comment syntax should follow the conventions of the host language. Select the appropriate prefix for each language, e.g.:
    - // MARK: comment
    - #pragma mark - comment
    - // ====== comment ======

**File Headers**
- Verify if a file header is required for the host language as a best practice, and if so, create one: e.g.:
    - Replace to use correct ___FILENAME___, ___DATE___, etc.
    - Descriptions
        - ___FILENAME___: real file names.
        - ___PACKAGENAME___: = Musically.
        - ___DATE___: format is DD/MM/YYYY.
```
//  ___FILENAME___
//  Musically
//
//  Created by Xueliang Chen on ___DATE___.
//  Copyright © ___YEAR___ Bytedance. All rights reserved.
```

**How to write single-line comments:**
- Use a emoji suitable with a semi-colon as the beginning of the line, e.g.

```
// 📌:  Single-line comment.
```

**How to write interface comments:**
- For public APIs that will be reused elsewhere, provide comprehensive documentation, e.g.
```
/// Show sheet view, with full settings.
/// @param view view will be shown on SheetContainer, inherited from UIView.
/// @param option option is designed for extensibility, defined in GBLSheetOption.
- (nullable id<GBLSheetController>)showView:(nullable UIView *)view withOption:(nullable GBLSheetOption *)option;
```

**How to write implementation comments:**
- Use the following template when adding comments to various sections of your implementation code, e.g.

```
class A {
    // MARK: - Constants
    //

    // MARK: - Props
    //

    // MARK: - Views
    //

    // MARK: - Life Cycle
    //
    @objc public init(){
        super.init(frame: CGRectZero)
        self.p_setup()
    }

    @objc public override init(frame: CGRect) {
        super.init(frame: frame)
        self.p_setup()
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

    // MARK: - Protocol B
    //

    // MARK: - Override
    //
}
```