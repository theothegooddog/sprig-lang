# Sprig

Sprig is a SwiftUI-style language for the web. Write views, modifiers and `@State`, or paste in ordinary SwiftUI structs, and get a page that runs in any browser.

```swift
struct ContentView: View {
    @State private var count = 0

    var body: some View {
        VStack(spacing: 16) {
            Text("Count: \(count)")
                .font(.largeTitle)
            Button("Add one") { count += 1 }
                .buttonStyle(.borderedProminent)
        }
        .padding()
    }
}
```

This repo holds:

- **`sprigc`**, a compiler that turns a `.spg` file into a web page
- **`playground/index.html`**, an in-browser editor with live preview, examples and a guide
- **`examples/`**, a counter, a habit tracker and a multi-screen fruit stand app

## Get sprigc

**Download a binary** from [Releases](https://github.com/theothegooddog/sprig/releases): macOS (Apple Silicon and Intel), Windows and Linux. Nothing else needs to be installed.

On macOS, the first run may be blocked because the binary isn’t signed by a registered developer. Clear that once with:

```
xattr -d com.apple.quarantine sprigc
```

**Or run it with Node 16+:**

```
git clone https://github.com/theothegooddog/sprig.git
cd sprig
npm install
node bin/sprigc.js examples/counter.spg
```

## Use

```
sprigc app.spg                     # web build: dist/index.html, app.js, sprig.runtime.js, sprig.css
sprigc app.spg --standalone        # one self-contained app.html
sprigc app.spg -s -o public/       # choose where it goes
sprigc app.spg --watch             # rebuild on every save
sprigc app.spg --check             # find errors without writing anything
```

| Option | What it does |
|---|---|
| `--web` | Separate HTML, CSS and JS (default). The runtime and CSS don’t change between builds, so browsers cache them. |
| `-s`, `--standalone` | One `.html` file you can email, upload or open from disk. |
| `-o`, `--out <path>` | Output folder, or a file for standalone builds. |
| `--title <text>` | Page title. Defaults to the file name. |
| `--check` | Checks without writing. Renders the program in a simulated browser, so it catches runtime errors as well as syntax errors. |
| `-w`, `--watch` | Rebuild whenever the file changes. |
| `--keep-source` | Ship the source text and parse it in the browser instead of the precompiled program. |
| `--no-icons` | Skip icon bundling. |

Errors point at the line:

```
app.spg:3: error: Expected “)” but found “Text”
  1 | VStack {
  2 |   Text("a"
> 3 |   Text("b")
```

## What the language covers

Sprig runs Swift-shaped code, not the Swift compiler. It supports structs, classes, enums, `@State`, `@Binding`, `@Observable`, `NavigationStack`, `TabView`, sheets, alerts, lists with swipe actions, grids, animations, transitions, gestures and timers, and draws SF Symbol names with Phosphor and Lucide icons. Types are read but not checked, and Apple frameworks beyond SwiftUI (Charts, MapKit, SwiftData) aren’t available. The full guide is in the playground’s Guide tab.

## Develop

```
npm install          # also fetches jsdom for the full --check
npm run build        # regenerates lib/assets.js
npm test             # checks every example
npm run binaries     # standalone binaries for every platform (needs Bun)
```

Pushing a tag like `v0.4.0` runs the Release workflow, which builds every binary and attaches it to a GitHub Release.

## License

MIT. Bundled icons are from [Lucide](https://lucide.dev) (ISC) and [Phosphor](https://phosphoricons.com) (MIT); see `lib/ICON-LICENSES.txt`.
