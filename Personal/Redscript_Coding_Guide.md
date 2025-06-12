# Redscript Coding Guide

## Table of Contents

- [Section 1: Language Fundamentals](#section-1-language-fundamentals)
  - [1.1 Syntax Basics](#11-syntax-basics)
  - [1.2 Data Types](#12-data-types)
  - [1.3 Variables and Constants](#13-variables-and-constants)
  - [1.4 Functions](#14-functions)
  - [1.5 Control Flow](#15-control-flow)
  - [1.6 Classes](#16-classes)
  - [1.7 Access Modifiers](#17-access-modifiers)
  - [1.8 Arrays](#18-arrays)
  - [1.9 Logging and Debugging](#19-logging-and-debugging)
  - [1.10 Common Built-ins](#110-common-built-ins)
- [Section 2: Advanced Concepts](#section-2-advanced-concepts)
  - [2.1 Modules and Namespacing](#21-modules-and-namespacing)
  - [2.2 Script Annotations](#22-script-annotations)
  - [2.3 Native and External Methods](#23-native-and-external-methods)
  - [2.4 Persistent Data](#24-persistent-data)
  - [2.5 Conditional Compilation](#25-conditional-compilation)
    - [Practical Use Case:](#practical-use-case)
  - [2.6 Event and Callback Handling](#26-event-and-callback-handling)
  - [2.7 Performance Considerations](#27-performance-considerations)
    - [Inefficient Example:](#inefficient-example)
    - [Efficient Example:](#efficient-example)
    - [Object Instantiation Tip:](#object-instantiation-tip)
- [Section 3: Use Case Patterns](#section-3-use-case-patterns)
  - [3.6 Custom Event Triggers](#36-custom-event-triggers)
  - [3.1 Modify AI Behavior](#31-modify-ai-behavior)
  - [3.2 Inject Custom UI](#32-inject-custom-ui)
  - [3.3 Add Gameplay Mechanics](#33-add-gameplay-mechanics)
  - [3.4 Extend Player Abilities](#34-extend-player-abilities)
  - [3.5 Enhance Vehicle Systems](#35-enhance-vehicle-systems)
- [Section 4: Best Practices](#section-4-best-practices)
  - [4.1 Namespace Discipline](#41-namespace-discipline)
  - [4.2 Wrapping over Replacing](#42-wrapping-over-replacing)
  - [4.3 Defensive Programming](#43-defensive-programming)
  - [4.4 Compatibility Shims](#44-compatibility-shims)
  - [4.5 Testing and Isolation](#45-testing-and-isolation)
- [Section 5: Troubleshooting and Debugging](#section-5-troubleshooting-and-debugging)
  - [5.1 Compile Errors](#51-compile-errors)
  - [5.2 Script Conflicts](#52-script-conflicts)
  - [5.3 Cache Corruption](#53-cache-corruption)
  - [5.4 REDmod Compatibility](#54-redmod-compatibility)
- [Section 6: Resources and References](#section-6-resources-and-references)
  - [6.1 Tool Links](#61-tool-links)
  - [6.2 CLI and Script Utilities](#62-cli-and-script-utilities)
- [Section 7: Expert Techniques](#section-7-expert-techniques)
  - [7.1 Virtual Functions and Polymorphism](#71-virtual-functions-and-polymorphism)
    - [Key Concepts:](#key-concepts)
    - [Example:](#example)
  - [7.2 Lifecycle Hooks Deep Dive](#72-lifecycle-hooks-deep-dive)
    - [GameObject Lifecycle:](#gameobject-lifecycle)
    - [Best Practices:](#best-practices)
  - [7.3 Native Intrinsics and RTTI Access](#73-native-intrinsics-and-rtti-access)
    - [Useful Intrinsics:](#useful-intrinsics)
    - [Advanced RTTI:](#advanced-rtti)
  - [7.4 Memory Management and GC](#74-memory-management-and-gc)
  - [7.5 Multithreading and Frame Control](#75-multithreading-and-frame-control)
    - [Thread Contexts:](#thread-contexts)
  - [7.6 Annotation Chains and Inter-Mod Cooperation](#76-annotation-chains-and-inter-mod-cooperation)
    - [Coordinating Multiple Mods](#coordinating-multiple-mods)
    - [Exposing APIs:](#exposing-apis)
  - [7.7 Hybrid Redscript + Lua (CET) Integration](#77-hybrid-redscript-+-lua-cet-integration)
  - [7.8 UI Extension Techniques](#78-ui-extension-techniques)
    - [Null Reference Check Example](#null-reference-check-example)
  - [7.9 Reverse Engineering Tools & Tactics](#79-reverse-engineering-tools-&-tactics)
    - [Tools:](#tools)
    - [Approach:](#approach)
  - [7.10 Packaging, Testing, and Tooling](#710-packaging-testing-and-tooling)
    - [Mod Folder Structures](#mod-folder-structures)
    - [Packaging:](#packaging)
    - [Testing:](#testing)
    - [Automation:](#automation)
- [Conclusion](#conclusion)
- [Section 8: Expert Recommendations](#section-8-expert-recommendations)
  - [8.1 Type System Quick Reference](#81-type-system-quick-reference)
  - [8.2 Lifecycle Hook Summary](#82-lifecycle-hook-summary)
  - [8.3 Debug Error Glossary](#83-debug-error-glossary)
  - [8.4 Versioning Tip](#84-versioning-tip)
  - [8.5 Tooling Suggestions](#85-tooling-suggestions)
## Section 1: Language Fundamentals ✅ (CP2077 v2.21+, RED4ext v1.27+)

This section establishes the core syntax and structure of Redscript, the scripting language used to develop Cyberpunk 2077 gameplay modifications. It is intended for developers familiar with C-style languages, though no prior experience with game modding is required.

### 1.1 Syntax Basics

Redscript uses a C-style syntax with static typing, single inheritance, and no manual memory management.

```swift
let playerName: String = "V";
let maxEnemies: Int32 = 12;
```

- Semicolons terminate all statements.
- Braces `{}` denote code blocks.
- Comments use `//` for single-line or `/* */` for multi-line.

### 1.2 Data Types

**Primitive Types:**

- `Int8`, `Int16`, `Int32`, `Int64`
- `Uint8`, `Uint16`, `Uint32`, `Uint64`
- `Float`, `Double`
- `Bool`

**Game-Specific Types:**

- `String`: Unicode text
- `CName`: Engine-optimized hashed identifier
- `TweakDBID`: Identifier for database entries
- `Variant`: Holds any data type, loosely typed

**Reference Types:**

- `ref<Type>`: Strong reference
- `wref<Type>`: Weak reference
- `array<Type>`: Dynamic array

### 1.3 Variables and Constants

```swift
let health: Int32 = 100;
let message: String;
message = "Hello";
```

### 1.4 Functions

```swift
public func Add(x: Int32, y: Int32) -> Int32 {
  return x + y;
}
```

### 1.5 Control Flow

```swift
if health > 0 {
  Log("Alive");
} else {
  Log("Dead");
}
```

### 1.6 Classes

```swift
public class Weapon extends IScriptable {
  public let damage: Float;
  public func Fire() -> Void {
    Log("Bang! Damage: " + Cast<String>(damage));
  }
}
```

### 1.7 Access Modifiers

- `public`: Accessible from anywhere
- `protected`: Accessible from subclass or same module
- `private`: Accessible only within the declaring scope

### 1.8 Arrays

```swift
let enemies: array<String> = ["Maelstrom", "Tyger Claws"];
ArrayPush(enemies, "Animals");
```

### 1.9 Logging and Debugging

```swift
native func Log(const text: script_ref<String>) -> Void;
Log("Debug message");
```

### 1.10 Common Built-ins

```swift
if IsDefined(target) {
  Log("Target is valid");
}

let randomValue: Float = RandF();
Log("Random float: " + Cast<String>(randomValue));

let health: Int32 = 100;
let asString: String = Cast<String>(health);
Log("Health as string: " + asString);
```

- `IsDefined(var)`
- `RandF()`, `RandRangeInt(min, max)`
- `Cast<Type>(value)`

---

## Section 2: Advanced Concepts ✅ (CP2077 v2.21+, RED4ext v1.27+)

### 2.1 Modules and Namespacing

```swift
module MyMod.WeaponTweaks;
import Game.GameInstance;
```

### 2.2 Script Annotations

- `@wrapMethod(ClassName)`
- `@replaceMethod(ClassName)`
- `@addMethod(ClassName)`
- `@addField(ClassName)`

### 2.3 Native and External Methods

```swift
native func Log(const text: script_ref<String>) -> Void;
```

### 2.4 Persistent Data

```swift
public class ModSettings extends ScriptableSystem {
  public persistent let isEnabled: Bool;
}
```

### 2.5 Conditional Compilation

> Note: Conditional compilation in Redscript is not officially documented and may rely on RED4ext extensions or community-developed tooling. Use with caution and test thoroughly across different setups.

Conditional compilation can be useful when developing mods intended to integrate with or optionally depend on other mods. For example, if your mod provides enhanced inventory features but only when a UI extension mod is present, you can conditionally import and use its module:

```swift
@if(ModuleExists("UIEnhancer.WidgetExtensions"))
import UIEnhancer.WidgetExtensions;
```

This ensures your code only references external functionality when it’s actually available, reducing the chance of runtime errors and improving cross-mod compatibility.

#### Practical Use Case:

If you have a shared HUD extension mod and want to add a feature only when it’s installed:

```swift
@if(ModuleExists("SharedHUD.Overlays"))
import SharedHUD.Overlays;
```

This allows your base mod to remain usable even if the optional module is missing. import OtherMod.Module.Utility;

```

```swift
@if(ModuleExists("OtherMod.Module"))
import OtherMod.Module.Utility;
```

### 2.6 Event and Callback Handling

```swift
protected cb func OnGameAttached() -> Bool {
  Log("Game attached");
  return true;
}
```

### 2.7 Performance Considerations

Efficient Redscript code ensures smoother runtime behavior and avoids unnecessary overhead. Consider the following examples:

#### Inefficient Example:

```swift
for i in range(0, 100000) {
  let temp: String = "Item " + Cast<String>(i);
  Log(temp);
}
```

- Creates many temporary strings
- Excessive logging during gameplay

#### Efficient Example:

```swift
let prefix: String = "Item ";
for i in range(0, 100) {
  if i % 10 == 0 {
    Log(prefix + Cast<String>(i));
  }
}
```

- Limits the number of log calls
- Reuses a base string

#### Object Instantiation Tip:

```swift
// Instead of creating new widgets every frame...
let label: ref<inkText> = new inkText();
```

- Cache objects and reuse them when possible
- Avoid dynamic allocation inside frequently called methods

Also, always validate object references and avoid deep recursion or unnecessarily long script chains.

- Avoid tight loops
- Reuse objects
- Validate references

## Section 3: Use Case Patterns ✅ (CP2077 v2.21+, RED4ext v1.27+)

### 3.1 Modify AI Behavior

Wrap AI controller methods to change pathfinding, aggression, or reaction.

For example, to reduce aggression range on detection:

```swift
@wrapMethod(AIHumanComponent)
public func GetAggroRange() -> Float {
  let originalRange = wrappedMethod();
  return originalRange * 0.5; // Reduce aggro range by 50%
}
```

Or to adjust NPC movement speed:

```swift
@wrapMethod(CharacterMovementComponent)
public func GetMaxSpeed() -> Float {
  return 3.0; // Override movement speed
}
``` AI controller methods to change pathfinding, aggression, or reaction.

### 3.2 Inject Custom UI

Add new UI buttons or overlays using wrapped HUD functions.

```swift
@wrapMethod(PlayerHUDGameController)
protected func OnInitialize() -> Void {
  wrappedMethod();
  let text: ref<inkText> = new inkText();
  text.SetText("MOD ENABLED");
  text.SetFontSize(28);
  text.SetTintColor(new HDRColor(0.2, 1.0, 0.2, 1.0));
  this.GetRootWidget().AddChild(text);
}
```

This example adds a green label to the player's HUD confirming the mod is active. new UI buttons or overlays using wrapped HUD functions.

### 3.3 Add Gameplay Mechanics

Use a `ScriptableSystem` to introduce and persist new game logic.

### 3.4 Extend Player Abilities

Replace or wrap functions in `PlayerPuppet` to alter movement, visibility, etc.

### 3.5 Enhance Vehicle Systems

Modify vehicle physics and input handlers using `VehicleComponent` methods.

### 3.6 Custom Event Triggers

You can define and dispatch custom events between systems or objects using the built-in event infrastructure. This enables mods to communicate cleanly without tight coupling.

**Defining a Custom Event:**

```swift
public class MyModCustomEvent extends Event {
  public let triggerSource: String;
}
```

**Dispatching the Event:**

```swift
GameInstance.GetUISystem().QueueEvent(new MyModCustomEvent());
```

**Listening for the Event:**

```swift
public class MySystem extends ScriptableSystem {
  protected cb func OnMyModCustomEvent(evt: ref<MyModCustomEvent>) -> Bool {
    Log("Received custom event from: " + evt.triggerSource);
    return true;
  }
}
```

This pattern allows scalable integration between mods or modular gameplay systems.## Section 4: Best Practices ✅ (CP2077 v2.21+, RED4ext v1.27+)

### 4.1 Namespace Discipline

Use `module` consistently. Avoid collisions with game/global scope.

### 4.2 Wrapping over Replacing

Only use `@replaceMethod` when you must fully override logic. In most cases, `@wrapMethod` is safer and more compatible with other mods.

**Wrapping Example:**

```swift
@wrapMethod(PlayerPuppet)
public func OnGameAttached() -> Void {
  Log("[MyMod] Initializing player");
  wrappedMethod();
}
```

- This preserves existing game behavior and adds custom logic.

**Replacing Example (Riskier):**

```swift
@replaceMethod(PlayerPuppet)
public func OnGameAttached() -> Void {
  Log("[MyMod] Replaced logic");
}
```

- This removes all existing logic, potentially breaking other mods or core systems.

Wrapping ensures compatibility and minimizes unintended side effects when multiple mods target the same function. Only use `@replaceMethod` when you must fully override logic.

### 4.3 Defensive Programming

Check `IsDefined()` before calling on refs. Wrap critical logic in conditionals.

```swift
let target: wref<GameObject>;
if IsDefined(target) {
  let go = target.Get();
  if IsDefined(go) {
    go.Kill();
  } else {
    Log("Reference is defined but object no longer valid.");
  }
} else {
  Log("Target not found");
}
```

This approach prevents null reference crashes and gracefully handles edge cases where a weak reference may resolve to null due to in-game state changes or cleanup.

```swift
let target: wref<GameObject>;
if IsDefined(target) {
  target.Get().Kill();
} else {
  Log("Target not found");
}
```

This prevents null reference crashes and helps maintain stability during dynamic game conditions. Check `IsDefined()` before calling on refs. Wrap critical logic in conditionals.

### 4.4 Compatibility Shims

Use `@if ModuleExists()` to adapt to other mods.

### 4.5 Testing and Isolation

Test with a clean mod list. Use logging to trace logic paths.

## Section 5: Troubleshooting and Debugging ✅ (CP2077 v2.21+, RED4ext v1.27+)

### 5.1 Compile Errors

Check `r6/cache/redscript.log`. Look for syntax or reference issues.

### 5.2 Script Conflicts

Avoid multiple mods replacing the same method. Favor `@wrapMethod`.

### 5.3 Cache Corruption

Delete `final.redscripts` and retry. Always clear between major updates.

**Common Cache Directories:**

- `r6/cache/final.redscripts`: Compiled Redscript cache file
- `r6/cache/modded/`: Temporary mod cache (if REDmod is used)
- `r6/logs/redscript.log`: Compiler output and error details

Clearing these ensures clean recompilation and helps diagnose mod conflicts or errors during development. Delete `final.redscripts` and retry. Always clear between major updates.

### 5.4 REDmod Compatibility

Use `CyberCMD` or `RED4ext` to ensure Redscript functions in REDmod environments.

## Section 6: Resources and References ✅ (CP2077 v2.21+, RED4ext v1.27+)

### 6.1 Tool Links

- [REDscript GitHub](https://github.com/jac3km4/redscript) — The official source code repository for Redscript. Useful for tracking changes and raising issues.
- [REDmodding Wiki](https://wiki.redmodding.org) — Community-curated documentation and tutorials on modding Cyberpunk 2077, including Redscript, WolvenKit, and REDmod.
- [NativeDB](https://nativedb.red4ext.com) — A searchable database of native Cyberpunk 2077 types, methods, and fields exposed to scripting.
- [Cyberpunk Modding Discord](https://discord.gg/redmodding) — Active modding community with channels for scripting help, reverse engineering, and collaboration.

### 6.2 CLI and Script Utilities

To compile and validate Redscript manually:

```bash
redscript compile
```

To run with verbose output:

```bash
redscript compile --log-level debug
```

Example testing script:

```bash
#!/bin/bash
rm -rf r6/cache/final.redscripts
redscript compile
cat r6/logs/redscript.log | grep MyMod
```

Use these for integration with CI/CD pipelines or to validate that new commits don’t break mod logic.

- [REDscript GitHub](https://github.com/jac3km4/redscript) — The official source code repository for Redscript. Useful for tracking changes and raising issues.

- [REDmodding Wiki](https://wiki.redmodding.org) — Community-curated documentation and tutorials on modding Cyberpunk 2077, including Redscript, WolvenKit, and REDmod.

- [NativeDB](https://nativedb.red4ext.com) — A searchable database of native Cyberpunk 2077 types, methods, and fields exposed to scripting.

- [Cyberpunk Modding Discord](https://discord.gg/redmodding) — Active modding community with channels for scripting help, reverse engineering, and collaboration.

- [WolvenKit](https://wolvenkit.com) — Powerful open-source toolchain for inspecting and modifying Cyberpunk 2077 assets and scripts.

- [CyberCMD](https://github.com/WopsS/CyberEngineTweaks) — Advanced launcher for REDmod and scripting environments.

## Section 7: Expert Techniques ✅ (CP2077 v2.21+, RED4ext v1.27+)

### 7.1 Virtual Functions and Polymorphism

Redscript supports single inheritance and virtual-like method behavior. However, not all functions in REDengine classes are explicitly marked `virtual`, which limits traditional method overriding. Instead, Redscript modders rely on annotations like `@wrapMethod` to simulate polymorphism.

#### Key Concepts:

- If a base class method is defined in script (not native), child classes can override it.
- If a method is defined in a native class, and not exposed to Redscript as `virtual`, it cannot be overridden directly.
- Annotations like `@wrapMethod` allow layered behavior across mods and subclasses.

#### Example:

```swift
public class WeaponLogic extends GameScriptableComponent {
  public func Fire() -> Void {
    Log("Base fire");
  }
}

public class SpecialWeapon extends WeaponLogic {
  public func Fire() -> Void {
    Log("Special fire!");
  }
}
```

Use `Cast<SpecialWeapon>(weapon).Fire()` only if you're sure of the type. Otherwise, wrapping `WeaponLogic.Fire()` is safer:

```swift
@wrapMethod(WeaponLogic)
public func Fire() -> Void {
  Log("[MOD] Wrapping fire");
  wrappedMethod();
}
```

### 7.2 Lifecycle Hooks Deep Dive

#### GameObject Lifecycle:

- `OnGameAttached()`: First entry point for most game objects
- `OnInitialize()`: Used for systems and some UI objects
- `OnUninitialize()`: Cleanup logic
- `OnAction()`: Handles user inputs (e.g., button presses)

#### Best Practices:

- Delay complex setup until `OnGameAttached()` is called
- Use `IsDefined(GameInstance)` before invoking game-wide systems

```swift
protected cb func OnGameAttached() -> Bool {
  Log("Game attached");
  this.RegisterInputListener();
  return true;
}
```

### 7.3 Native Intrinsics and RTTI Access

#### Useful Intrinsics:

- `GetClassName()`: Returns a `CName` of the object's class
- `ToString()`: Printable string, useful for debug
- `Equals(other)`: Compares script objects

```swift
Log(this.GetClassName().ToString());
```

#### Advanced RTTI:

Use RED4ext to interact with full RTTI and access hidden properties:

```lua
-- In CET: get property via RTTI
Game.GetPlayer():GetRecord():GetStatByName("MaxHealth")
```

### 7.4 Memory Management and GC

Redscript handles garbage collection internally. Modders should:

- Use `ref<>` for owning references
- Use `wref<>` for non-owning references (e.g., cached lookups)
- Avoid circular references

```swift
let enemy: wref<GameObject>;
if IsDefined(enemy) {
  enemy.Get().Kill();
}
```

Avoid keeping persistent `ref<>` across frames unless needed.

### 7.5 Multithreading and Frame Control

#### Thread Contexts:

- Game Tick: Used for object updates
- UI Tick: Menu and HUD events
- Script Delay: Use `GameInstance.GetDelaySystem()` to defer execution

```swift
GameInstance.GetDelaySystem().DelayCallback(this, n"DelayedFunction", 0.5);
```

Avoid complex loops or delays in HUD updates.

### 7.6 Annotation Chains and Inter-Mod Cooperation

Multiple mods can wrap the same method. Use guard conditions or delegation to maintain compatibility.

```swift
@wrapMethod(PlayerPuppet)
public func Update() -> Void {
  if ShouldRunMyLogic() {
    DoMyModBehavior();
  }
  wrappedMethod();
}
```

#### Coordinating Multiple Mods

If two mods wrap the same method, each should check for its own activation condition to avoid interference. Here's an example:

**Mod A:**

```swift
@wrapMethod(PlayerPuppet)
public func Update() -> Void {
  if IsDefined(this.GetModAComponent()) {
    this.GetModAComponent().DoA();
  }
  wrappedMethod();
}
```

**Mod B:**

```swift
@wrapMethod(PlayerPuppet)
public func Update() -> Void {
  if IsDefined(this.GetModBComponent()) {
    this.GetModBComponent().DoB();
  }
  wrappedMethod();
}
```

If both mods are present, both wrapped versions are chained by the runtime. Each `wrappedMethod()` resolves to the next wrapper in sequence.

#### Exposing APIs:

Use `ScriptableSystem` singletons as shared bridges:

```swift
public class SharedAPI extends ScriptableSystem {
  public func RegisterModListener(listener: wref<IModListener>) -> Void {
    // implementation
  }
}
```

### 7.7 Hybrid Redscript + Lua (CET) Integration

Redscript can expose public methods that are callable from CET Lua:

In Redscript:

```swift
public class ModHooks extends ScriptableSystem {
  public func ToggleFeature() -> Void {
    Log("Toggling feature");
  }
}
```

In CET Lua:

```lua
Game.GetScriptableSystemsContainer():Get("ModHooks"):ToggleFeature()
```

Use this to bridge UI interactions or debug overlays.

### 7.8 UI Extension Techniques

You can inject into HUD controllers:

- `GameController` classes (e.g., `PlayerHUDGameController`)
- Use `inkWidget` to add icons or stats

```swift
@wrapMethod(PlayerHUDGameController)
protected func OnInitialize() -> Void {
  wrappedMethod();
  let widget: ref<inkText> = new inkText();
  widget.SetText("MOD ACTIVE");
  this.GetRootWidget().AddChild(widget);
}
```

Use `inkCanvas` for layout and `inkAnimProxy` for effects.

> ⚠️ **Performance Tip:** Avoid adding large or frequent updates to HUD elements on every frame. Overusing UI injections or modifying ink tree structures excessively may degrade frame rate, especially during combat or high-load sequences.

#### Null Reference Check Example

```swift
let label: wref<inkText>;
if IsDefined(label) {
  label.Get().SetText("Active");
}
```

### 7.9 Reverse Engineering Tools & Tactics

#### Tools:

- **WolvenKit**: View/modify game assets, scripts, UI
- **NativeDB**: Reference all exposed types and functions
- **Redscript Decompiler**: View CDPR source `.reds`

#### Approach:

- Start with a known class or UI target
- View `.reds` for method names
- Use NativeDB to confirm signatures

### 7.10 Packaging, Testing, and Tooling

#### Mod Folder Structures

**Legacy Folder Structure:**

```
Cyberpunk 2077/
├── r6/
│   └── scripts/
│       └── MyMod/
│           └── main.reds
```

**REDmod Structure:**

```
Cyberpunk 2077/
├── mods/
│   └── MyMod/
│       ├── info.json
│       └── r6/
│           └── scripts/
│               └── main.reds
```

Include an `info.json` descriptor for REDmod:

```json
{
  "name": "MyMod",
  "version": "1.0.0",
  "author": "ModderName",
  "description": "Adds new weapon behaviors.",
  "required": ["RED4ext"]
}
```

#### Packaging:

- Legacy: Place `.reds` files in `r6/scripts/MyMod`
- REDmod: Create a mod folder with descriptor

#### Testing:

- Create a `ModStatus.reds` that logs startup success
- Use `Log()` statements tagged with your mod name

#### Automation:

- Write shell scripts to clear cache and validate `redscript.log`
- Create a JSON config file for your mod metadata and versioning

```json
{
  "name": "MyAdvancedMod",
  "version": "1.2.0",
  "requires": ["RED4ext", "CET"]
}
```

## Conclusion ✅ (CP2077 v2.21+, RED4ext v1.27+)

This guide is designed to serve as a comprehensive and reliable reference for modders working with Redscript in Cyberpunk 2077. From language fundamentals to expert-level integrations and best practices, each section is intended to empower developers to create stable, compatible, and innovative mods that extend and enrich the game experience.

As the Redscript and Cyberpunk modding ecosystems continue to evolve, this document should grow alongside them. Mod responsibly, test thoroughly, and share your discoveries with the community.

Happy modding, and see you in Night City.
---

## Section 8: Expert Recommendations ✅ (CP2077 v2.21+, RED4ext v1.27+)

### 8.1 Type System Quick Reference

| Type        | Description              | Example              |
|-------------|--------------------------|----------------------|
| `ref<Type>` | Strong reference         | `ref<inkText>`       |
| `wref<Type>`| Weak reference (GC-safe) | `wref<GameObject>`   |
| `Variant`   | Dynamic typing wrapper   | `let x: Variant`     |

### 8.2 Lifecycle Hook Summary

| Event Hook     | Object Type         | Purpose                        | When It Runs              |
|----------------|---------------------|--------------------------------|---------------------------|
| `OnGameAttached` | All GameObjects      | Initialization phase           | On instance spawn         |
| `OnUninitialize` | UI, systems, actors  | Cleanup                        | On object destruction     |

### 8.3 Debug Error Glossary

| Error Type         | Meaning                                | Fix                              |
|--------------------|----------------------------------------|----------------------------------|
| `Invalid type cast`| You're casting to an incompatible type | Use `Cast<>` and check class hierarchy |
| `Unresolved symbol`| Variable/class not found               | Check module/import              |

### 8.4 Versioning Tip

Add comments to document Redscript version compatibility:

```swift
// ✅ Verified on Cyberpunk 2077 v2.1.0 Hotfix 3
@wrapMethod(PlayerPuppet)
public func OnGameAttached() -> Void {
  ...
}
```

### 8.5 Tooling Suggestions

- [WolvenKit](https://wolvenkit.com) with Redscript plugin
- VS Code + Markdown preview + syntax highlighting
- Shell scripts for auto-linting and `redscript.log` inspection