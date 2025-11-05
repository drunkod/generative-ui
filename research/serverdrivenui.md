### Key Points
- Research suggests that server-driven UI (SDUI) frameworks are gaining traction in open source communities, allowing dynamic UI updates from the server without app redeployments, which can streamline development for mobile and web apps.
- It seems likely that popular projects focus on cross-platform support, such as Flutter, Android, iOS, and web, with JSON-based configurations for flexibility.
- Evidence leans toward Flutter having a strong ecosystem for SDUI, with tools like Mirai/Stac and Theta enabling real-time JSON-driven interfaces.
- While innovative, these frameworks may involve trade-offs like dependency on backend stability or learning curves for custom components, though community contributions help mitigate this.

### Overview of Server-Driven UI
Server-driven UI (SDUI), also known as backend-driven UI, shifts UI logic to the server, enabling apps to fetch and render interfaces dynamically. This approach supports A/B testing, quick fixes, and cross-platform consistency without updating the app binary. Open source projects in this space often use JSON schemas to define layouts, which clients parse into native components. For instance, frameworks like DivKit support iOS, Android, and web, while others target specific ecosystems like Flutter or Jetpack Compose.

### Notable Projects
Several open source repositories stand out for their maturity and features. DivKit from Yandex offers comprehensive cross-platform rendering with a playground for testing. CraftD supports multiple UI frameworks across Android, iOS, and Flutter. Theta focuses on converting designs to Flutter code with live updates. For Android, server-driven-compose integrates with Firebase for real-time UI. iOS developers can use ServerDrivenSwiftUI for SwiftUI-based dynamic views. RevenueCat's purchases-android includes SDUI for paywalls. Mirai (now Stac) excels in Flutter with JSON-driven cross-platform apps.

For more details, explore GitHub topics like [backend-driven-ui](https://github.com/topics/backend-driven-ui) or search for "server-driven-ui" repositories.

---

Server-driven UI (SDUI) represents an evolving paradigm in application development where the server dictates the structure, content, and behavior of user interfaces, allowing for dynamic updates without requiring client-side redeployments. This technique, also referred to as backend-driven UI, leverages JSON or similar formats to describe UI components, which are then parsed and rendered by client libraries. It addresses challenges in traditional UI development, such as lengthy release cycles for mobile apps, by enabling real-time modifications, A/B testing, and cross-platform consistency. Open source projects in this domain emphasize accessibility, extensibility, and integration with popular frameworks like Flutter, Jetpack Compose, SwiftUI, and web technologies. While promising, SDUI introduces considerations around network dependency, security (e.g., validating server responses), and performance optimization for complex layouts. Community-driven efforts often include playgrounds, documentation, and modular designs to facilitate adoption.

The workflow typically involves defining UI schemas on the server, fetching them via APIs, and rendering them client-side. For example, a JSON object might specify a button's text, color, and action, which the client library translates into a native widget. This decouples UI from app code, allowing backend teams to iterate independently. Projects vary in scope: some are full frameworks with built-in components, while others provide templates or examples for custom implementations. Integration with tools like Firebase for real-time databases or Vapor for server-side logic enhances their utility. As of October 2025, the ecosystem shows growth in Flutter-centric tools, reflecting the framework's popularity for cross-platform development.

One key example is DivKit, an open-source SDUI framework that supports server-sourced updates across iOS, Android, and web. It uses a JSON schema for layouts, enabling a single source of truth for all platforms. Features include a sandbox for prototyping, live demos, and client libraries for rendering. Tutorials on platforms like Medium guide integration, emphasizing its use in production environments. CraftD offers a multi-platform approach, supporting Android View System, Jetpack Compose, Flutter, SwiftUI, and Kotlin Multiplatform. It includes pre-built components like buttons and checkboxes, with extensibility for custom ones via data classes and builders. Theta provides an open-source method for designing SDUI in Flutter, converting designs to code via a CLI tool, with options for live runtime fetching.

For Android-specific implementations, server-driven-compose uses Firebase Realtime Database to showcase SDUI in Jetpack Compose, focusing on rendering protocols, action handlers, and versioning. It employs MVVM architecture and unidirectional data flow for scalability. ServerDrivenSwiftUI targets iOS, allowing server-side SwiftUI code to be fetched and rendered, with examples using Vapor for the backend. RevenueCat's purchases-android integrates SDUI for paywalls, simplifying in-app purchases with remote configurations and webhooks.

Flutter has a vibrant SDUI community, with Mirai (now rebranded as Stac) standing out for its JSON-driven rendering of native widgets. It supports forms, theming, custom actions, and a Dart-to-JSON workflow for easier development. Other Flutter examples include server_driven_ui_with_mirai and flutter-server-driven-ui, which use Mirai for dynamic rendering.

Discussions on platforms like GitHub and Reddit highlight practical implementations, such as strategies for mobile apps or tools for Dart-to-JSON conversion. X (formerly Twitter) posts reveal ongoing interest, with mentions of SDUI in contexts like React Native, Vaadin for web, and contributions to projects like Stac.

To compare these projects, consider the following tables summarizing key attributes and use cases.

#### Project Overview Table

| Project Name | GitHub Link | Primary Focus | Supported Platforms | Key Technologies |
|--------------|-------------|---------------|---------------------|------------------|
| DivKit | [divkit/divkit](https://github.com/divkit/divkit) | Cross-platform SDUI with JSON schemas | iOS, Android, Web | JSON, Client libraries (Swift, Kotlin, JS) |
| CraftD | [CodandoTV/CraftD](https://github.com/CodandoTV/CraftD) | Multi-framework SDUI | Android (View/Compose), iOS (SwiftUI), Flutter, KMP | JSON, Composables, Builders |
| Theta | [buildwiththeta/buildwiththeta](https://github.com/buildwiththeta/buildwiththeta) | Design-to-code for Flutter | Flutter | Dart CLI, JSON fetching |
| server-driven-compose | [skydoves/server-driven-compose](https://github.com/skydoves/server-driven-compose) | SDUI in Jetpack Compose | Android | Firebase, Jetpack Compose, MVVM |
| ServerDrivenSwiftUI | [andylindebros/ServerDrivenSwiftUI](https://github.com/andylindebros/ServerDrivenSwiftUI) | Server-side SwiftUI rendering | iOS | SwiftUI, Vapor |
| purchases-android | [RevenueCat/purchases-android](https://github.com/RevenueCat/purchases-android) | SDUI for paywalls and subscriptions | Android | Google Play Billing, Remote configs |
| Stac (formerly Mirai) | [StacDev/stac](https://github.com/StacDev/stac) | JSON-driven Flutter UI | Flutter (cross-platform) | JSON, Native widgets, Forms/Theming |

#### Use Case Comparison Table

| Use Case | Recommended Projects | Why Suitable | Potential Drawbacks |
|----------|----------------------|--------------|---------------------|
| Cross-Platform Mobile/Web | DivKit, CraftD | Unified JSON for multiple platforms; Playground for testing | May require custom extensions for advanced features |
| Flutter-Specific Development | Theta, Stac/Mirai | Live updates and Dart-to-JSON; Built-in forms and theming | Dependency on Flutter's ecosystem limits to Dart apps |
| Android Subscriptions/Paywalls | purchases-android, server-driven-compose | Integrated with billing and Firebase; Real-time versioning | Focused on monetization, less general-purpose |
| iOS Dynamic UI | ServerDrivenSwiftUI, CraftD | SwiftUI integration; Server-side code fetching | Limited to Apple ecosystem; Vapor backend setup needed |
| Prototyping/Experimentation | DivKit, Theta | Sandboxes and CLI tools for quick iterations | Network latency in live modes |

In practice, getting started often involves cloning the repository, setting up dependencies (e.g., via pub for Flutter or Gradle for Android), and configuring API keys for backends like Firebase. For DivKit, use the playground to test JSON layouts before integrating. CraftD requires defining component builders and JSON models. Theta's CLI simplifies code generation with commands like `theta gen`. server-driven-compose observes Firebase data flows for rendering. Contributions are encouraged, such as adding new components or improving cross-platform support, to advance these frameworks.

### Key Citations
- [DivKit is an open source Server-Driven UI (SDUI ... - GitHub](https://github.com/divkit/divkit)
- [A framework example for Server Driven UI (SDUI) that ... - GitHub](https://github.com/csmets/Server-Driven-UI)
- [backend-driven-ui · GitHub Topics](https://github.com/topics/backend-driven-ui)
- [Meet Mirai: The Open-Source Server-Driven UI Framework ... - Reddit](https://www.reddit.com/r/opensource/comments/1id4n8p/meet_mirai_the_opensource_serverdriven_ui/)
- [CodandoTV/CraftD: A Server Driven UI library - GitHub](https://github.com/CodandoTV/CraftD)
- [Server-driven UI (or Backend driven UI) strategies #47 - GitHub](https://github.com/MobileNativeFoundation/discussions/discussions/47)
- [The open source way of designing server-driven UI, with ... - GitHub](https://github.com/buildwiththeta/buildwiththeta)
- [skydoves/server-driven-compose - GitHub](https://github.com/skydoves/server-driven-compose)
- [andylindebros/ServerDrivenSwiftUI: Server-driven SwiftUI - GitHub](https://github.com/andylindebros/ServerDrivenSwiftUI)
- [MohamedAbd0/server_driven_ui_with_mirai - GitHub](https://github.com/MohamedAbd0/server_driven_ui_with_mirai)
- [KasunHasanga/flutter-server-driven-ui - GitHub](https://github.com/KasunHasanga/flutter-server-driven-ui)
- [Stac - GitHub](https://github.com/stacdev)
- [StacDev/stac - GitHub](https://github.com/StacDev/stac)
- [Mirai — A Server Driven UI framework for Flutter](https://forum.itsallwidgets.com/t/mirai-a-server-driven-ui-framework-for-flutter/2374)
- [GitHub | Divyanshu Bhargava - LinkedIn](https://www.linkedin.com/posts/divyanshub024_github-securrency-ossmirai-mirai-is-a-activity-7168258234217099264-Judv)
- [Server Driven UI in Flutter Instantly Update Your App with Stac.](https://medium.com/@arorasarthak54/server-driven-ui-in-flutter-instantly-update-your-app-with-stac-3d952373b8da)
- [GitHub - divkit/divkit: DivKit is an open source Server-Driven UI (SDUI) framework. SDUI is a an emerging technique that leverage the server to build the user interfaces of their mobile app](https://github.com/divkit/divkit)
- [GitHub - buildwiththeta/buildwiththeta: The open source way of designing server-driven UI, with instant updates. Follow to stay updated about our Beta.](https://github.com/buildwiththeta/buildwiththeta)
- [GitHub - CodandoTV/CraftD: A Server Driven UI library](https://github.com/CodandoTV/CraftD)
- [GitHub - andylindebros/ServerDrivenSwiftUI: Server-driven SwiftUI - Maintain iOS apps without making app releases.](https://github.com/andylindebros/ServerDrivenSwiftUI)
- [GitHub - skydoves/server-driven-compose: 🧙 Server Driven Compose showcases server-driven UI approaches in Jetpack Compose with Firebase.](https://github.com/skydoves/server-driven-compose)
- [GitHub - RevenueCat/purchases-android: RevenueCat SDK for Android & Jetpack Compose, easy in-app purchases and subscriptions.](https://github.com/RevenueCat/purchases-android)
- [GitHub - StacDev/stac: Stac is a Server-Driven UI (SDUI) framework for Flutter, enabling you to create stunning, cross-platform applications dynamically with JSON. Build and update your app's UI in real-time with ease and flexibility!](https://github.com/StacDev/stac)
- [MohamedAbd0/server_driven_ui_with_mirai - GitHub](https://github.com/MohamedAbd0/server_driven_ui_with_mirai)
- [Meet Mirai: The Open-Source Server-Driven UI Framework ... - Reddit](https://www.reddit.com/r/opensource/comments/1id4n8p/meet_mirai_the_opensource_serverdriven_ui/)
- [KasunHasanga/flutter-server-driven-ui - GitHub](https://github.com/KasunHasanga/flutter-server-driven-ui)
- [Mirai — A Server Driven UI framework for Flutter](https://forum.itsallwidgets.com/t/mirai-a-server-driven-ui-framework-for-flutter/2374)
- [StacDev/stac - GitHub](https://github.com/StacDev/stac)
- [Mirai – Server-Driven UI Framework for Flutter - Pub.dev](https://pub.dev/documentation/mirai/latest/)
- [GitHub | Divyanshu Bhargava - LinkedIn](https://www.linkedin.com/posts/divyanshub024_github-buildmiraimirai-mirai-is-a-server-driven-activity-7283921309766475777-JfRI)
- [Introducing Mirai: A Server-Driven UI for Flutter : r/programming](https://www.reddit.com/r/programming/comments/1i18fn2/introducing_mirai_a_serverdriven_ui_for_flutter/)
- [Introducing Stac — A Server Driven UI framework for Flutter - Medium](https://medium.com/stac/introducing-mirai-a-server-driven-ui-framework-for-flutter-d020fd0c387d)


