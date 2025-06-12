# Redscript Coding Guide

## Section 1: Language Fundamentals

This section establishes the core syntax and structure of Redscript, designed for modders with general programming experience—especially those familiar with C-style languages—but who may be new to scripting in REDengine or modding Cyberpunk 2077.

### 1.1 Syntax Basics

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

...

## Conclusion

This guide is designed to serve as a comprehensive and reliable reference for modders working with Redscript in Cyberpunk 2077. From language fundamentals to expert-level integrations and best practices, each section is intended to empower developers to create stable, compatible, and innovative mods that extend and enrich the game experience.

As the Redscript and Cyberpunk modding ecosystems continue to evolve, this document should grow alongside them. Mod responsibly, test thoroughly, and share your discoveries with the community.

Happy modding, and see you in Night City.