# Zero Programming Language - Feature Demonstrations

This document summarizes all the Zero language demonstrations created, showcasing the language's capabilities.

## Available Demos

### 1. **demo-simple.0** - Quick Feature Overview
A compact demonstration of Zero's core features:
- ✅ Shapes (struct-like data types)
- ✅ Base64 encoding
- ✅ CRC32 checksums
- ✅ Wall clock time
- ✅ Seeded random number generation
- ✅ Cryptographic hashing

**Run it:**
```bash
./bin/zero check demo-simple.0
```

---

### 2. **demo-encoding.0** - Data Encoding & Serialization
Comprehensive demonstration of encoding utilities:

#### Features Demonstrated:
- **Base64 Encoding**: Binary-safe string encoding
- **Hexadecimal Encoding**: Byte array to hex string
- **URL Encoding**: Percent-encoding for URLs
- **CRC32 Checksums**: Fast data integrity checks
- **Varint Encoding**: Variable-length integer encoding (used in Protocol Buffers)
- **JSON Validation**: Parse and validate JSON
- **JSON Token Streaming**: Count tokens in JSON documents
- **JSON String Encoding**: Escape strings for JSON

**Key Code Example:**
```zero
let mut b64_buf: [64]u8 = [0, ...] // Fixed buffer
let data = std.mem.span("Hello, Zero!")
let b64_result = std.codec.base64Encode(b64_buf, data)

if b64_result.has {
    check world.out.write(b64_result.value)
}
```

**Run it:**
```bash
./bin/zero check demo-encoding.0
```

---

### 3. **demo-crypto.0** - Cryptographic Functions
Security-focused demonstrations:

#### Features Demonstrated:
- **CRC32 Hash**: Fast non-cryptographic hash
- **HMAC Authentication**: Message authentication codes
- **Constant-Time Comparison**: Prevents timing attacks (critical for password/token comparison)
- **Secure Random Numbers**: Cryptographically secure RNG
- **Hex Encoding**: Display binary data

**Key Insight:**
```zero
// NEVER use == for password comparison!
// Use constant-time comparison instead:
if std.crypto.constantTimeEql(password1, password2) {
    // Prevents timing side-channel attacks
}
```

**Run it:**
```bash
./bin/zero check demo-crypto.0
```

---

### 4. **demo-time-random.0** - Time & Randomness
Time handling and random number generation:

#### Features Demonstrated:
- **Time Durations**: Create and manipulate time spans
- **Wall Clock Time**: Unix timestamps
- **Monotonic Clock**: Measure elapsed time (unaffected by system clock changes)
- **Seeded RNG**: Deterministic random numbers (reproducible)
- **Entropy Source**: Non-deterministic randomness
- **Random Number Ranges**: Generate values in specific ranges

**Key Code Example:**
```zero
// Deterministic (same seed = same sequence)
let mut rng1 = std.rand.seed(42_u32)
let value1 = std.rand.nextU32(&mut rng1)

// Same seed produces same first value
let mut rng2 = std.rand.seed(42_u32)
let value2 = std.rand.nextU32(&mut rng2)
// value1 == value2 is true!

// Non-deterministic entropy
let random = std.rand.entropyU32()
```

**Run it:**
```bash
./bin/zero check demo-time-random.0
```

---

### 5. **demo-json-logger.0** - Structured Logging
JSON-based logging pattern (Zero has no built-in logging):

#### Concept:
Since Zero uses **capability-based I/O**, there's no hidden `log()` function. Instead, we build structured output manually:

```zero
fun logInfo(world: ref<World>, msg: String) -> Void raises {
    let ts = std.time.wallSeconds()
    check world.out.write("{\"level\":\"INFO\",\"message\":\"")
    check world.out.write(msg)
    check world.out.write("\",\"timestamp\":")
    // ... add timestamp
    check world.out.write("}\n")
}
```

This demonstrates:
- **Explicit output**: No hidden globals
- **Structured data**: JSON for machine parsing
- **Capability passing**: `world` is passed explicitly

---

## Key Zero Language Features Demonstrated

### 1. **Capability-Based I/O**
```zero
pub fun main(world: World) -> Void raises {
    check world.out.write("output")  // Must have World capability
}
```

### 2. **Explicit Error Handling**
```zero
fun risky() -> i32 raises { ErrorName } {
    raise ErrorName  // Explicit error
}

check risky()  // Propagate error upward
```

### 3. **Fixed Buffers (No Hidden Allocation)**
```zero
let mut buf: [64]u8 = [0, 0, ...] // Stack-allocated
let result = std.codec.base64Encode(buf, data)
```

### 4. **Maybe Type (Option)**
```zero
let result: Maybe<String> = std.codec.hexEncode(buf, bytes)
if result.has {
    check world.out.write(result.value)
}
```

### 5. **Shapes (Structs)**
```zero
shape Point {
    x: i32,
    y: i32,
}

let p = Point { x: 10, y: 20 }
```

### 6. **Explicit References**
```zero
fun mutate(rng: mutref<Rng>) -> u32 {
    return std.rand.nextU32(rng)  // &mut in Rust
}

let mut rng = std.rand.seed(42_u32)
let value = std.rand.nextU32(&mut rng)
```

---

## Use Cases Demonstrated

### 1. **Data Processing Pipelines**
- Encode/decode data (Base64, Hex, URL)
- Compute checksums (CRC32)
- Validate formats (JSON)

### 2. **Security Applications**
- Password verification (constant-time comparison)
- Message authentication (HMAC)
- Cryptographic hashing
- Secure random token generation

### 3. **Time-Based Systems**
- Timestamps for logs
- Elapsed time measurement
- Duration calculations

### 4. **Reproducible Testing**
- Seeded RNG for deterministic tests
- Controlled randomness
- Predictable behavior

### 5. **CLI Tools**
- Structured output (JSON logs)
- Data transformation utilities
- File processing

---

## Zero vs. Other Languages

| Feature | Zero | Rust | Go | Python |
|---------|------|------|-----|--------|
| **Capability-based I/O** | ✅ | ❌ | ❌ | ❌ |
| **No hidden allocations** | ✅ | ✅ | ❌ | ❌ |
| **Explicit error sets** | ✅ | ❌ | ❌ | ❌ |
| **Compile-time JSON output** | ✅ | ❌ | ❌ | ❌ |
| **No garbage collector** | ✅ | ✅ | ❌ | ❌ |
| **Fixed-buffer APIs** | ✅ (default) | ✅ (manual) | ❌ | ❌ |

---

## Standard Library Coverage

### Demonstrated Modules:

1. **`std.codec`**
   - `base64Encode()`, `hexEncode()`, `urlEncode()`
   - `crc32()`, `encodedVarintLen()`

2. **`std.crypto`**
   - `hash32()`, `hmac32()`
   - `constantTimeEql()`, `secureRandomU32()`

3. **`std.json`**
   - `validate()`, `streamTokens()`
   - `writeString()`

4. **`std.time`**
   - `seconds()`, `ms()`, `sub()`
   - `wallSeconds()`, `monotonic()`, `asMsFloor()`

5. **`std.rand`**
   - `seed()`, `nextU32()`
   - `entropyU32()`

6. **`std.mem`**
   - `span()`, `eql()`

---

## Running the Demos

### Type Check Only:
```bash
./bin/zero check demo-simple.0
./bin/zero check demo-encoding.0
./bin/zero check demo-crypto.0
./bin/zero check demo-time-random.0
./bin/zero check demo-json-logger.0
```

### Check All Demos:
```bash
for f in demo-*.0; do
    echo "Checking $f..."
    ./bin/zero check "$f" || echo "FAILED: $f"
done
```

### JSON Output (for agents):
```bash
./bin/zero check --json demo-simple.0
```

---

## What Makes Zero Special

### 1. **Agent-First Design**
- Structured compiler output (JSON)
- Explicit effects (no surprises)
- Predictable behavior
- Clear error messages

### 2. **No Hidden Costs**
- Every allocation is visible
- No automatic heap growth
- Fixed-buffer APIs by default
- Explicit memory control

### 3. **Capability Security**
- I/O requires explicit capabilities
- No ambient authority
- Trackable effects

### 4. **Small Binaries**
- No runtime overhead
- Direct code generation
- Pay-only-for-what-you-use

---

## Next Steps

1. **Explore examples/**: 70+ more examples covering all language features
2. **Read docs**: `npm run docs:dev` to start the documentation site
3. **Build real programs**: Check [examples/zero-hash/](#workspace-file=examples/zero-hash) for a complete CLI tool
4. **Try WASM**: Compile to WebAssembly with `--target wasm32-web`

---

## Notes About qlog

Zero doesn't have a built-in `qlog` (QUIC logging) library currently. However, you could build one using:
- `std.codec` for encoding
- `std.json` for structured output
- `std.time` for timestamps
- Capability-based file I/O

The demos above show all the primitives needed to build custom logging solutions!

---

**Zero is the programming language for agents** - designed to be predictable, explicit, and machine-readable.

