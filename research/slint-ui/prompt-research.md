Research and provide a detailed guide on Slint UI toolkit based on its official documentation, GitHub repos (slint-ui/slint, tools/lsp, tools/viewer), and recent releases (up to version 1.13 as of November 2025). Cover the following topics:
Understanding Slint Live Preview:
Explain how live preview works in Slint, including the LSP server for features like auto-complete and real-time UI rendering in editors (e.g., VS Code extension).
Describe the slint-viewer tool, including usage with the --auto-reload flag for previewing .slint files while editing.
Cover runtime live preview for running applications (introduced in later versions like 1.13), such as enabling SLINT_LIVE_PREVIEW=1 in Rust/C++ builds to reload UI changes without restarting the app.
Include setup steps: Installing Slint, configuring LSP in VS Code or other editors, and basic examples of previewing a simple .slint file.
Running Live Preview on Android:
Detail how to build and deploy Slint apps to Android devices or emulators (using features like backend-android-activity, cargo-apk, Android SDK/NDK setup).
Explain if and how live preview can be adapted for Android development, such as using runtime reload in a running Android app, previewing on desktop/emulator while targeting Android, or any workarounds for hot-reloading .slint files over ADB.
Provide step-by-step instructions for a basic Android Slint app, including Cargo.toml modifications, entry points (e.g., android_main), and running on a physical device.
Note any limitations, like file system access for reloads, and suggest alternatives like using SlintPad for initial preview before Android deployment.
Replicating Generative UI Functionality (Similar to Crayon):
Crayon is a React-based SDK that turns LLM streaming responses into dynamic, interactive UI components (e.g., charts, forms) in chat interfaces.
Propose how to achieve similar "generative UI" with Slint: Use AI/LLMs to generate .slint code dynamically, save it to a file, and leverage live preview or auto-reload to render it in real-time.
Include examples: Integrating Rust code with an LLM API (e.g., OpenAI) to output .slint syntax, then using slint-viewer --auto-reload or runtime preview to display the generated UI.
Suggest extensions for Android: Build a Slint Android app that fetches/generated .slint code at runtime (if possible via dynamic loading) or uses pre-generated components.
Provide code snippets in Rust for a proof-of-concept, and discuss challenges like Slint's compile-time nature vs. Crayon's runtime streaming.
Structure your response with clear sections, code examples, potential pitfalls, and references to official docs (e.g., https://slint.dev/docs, GitHub). Assume a beginner-level user with Rust knowledge, and focus on practical, testable steps.

gnerative ui https://deepwiki.com/thesysdev/crayon

https://github.com/slint-ui/slint
https://deepwiki.com/slint-ui/slint
A LSP Server that adds features like auto-complete and live preview of the .slint files to many editors.
It is bundled in a Visual Studio Code Extension available from the market place.
A slint-viewer tool which displays the .slint files. The --auto-reload argument makes it easy to preview your UI while you are working on it (when using the LSP preview is not possible).
https://github.com/slint-ui/slint/tree/master/tools/lsp
https://github.com/slint-ui/slint/tree/master/tools/viewer 