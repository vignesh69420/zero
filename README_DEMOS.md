# Zero Language Feature Demonstrations

Complete demonstrations showcasing Zero's capabilities. All demos are type-checked and ready to run.

## 📋 Quick Reference

| Demo | Size | Focus | Command |
|------|------|-------|---------|
| [demo-simple.0](#demo-simple0) | 1.7K | Overview | `./bin/zero check demo-simple.0` |
| [demo-encoding.0](#demo-encoding0) | 4.6K | Data Encoding | `./bin/zero check demo-encoding.0` |
| [demo-crypto.0](#demo-crypto0) | 2.9K | Cryptography | `./bin/zero check demo-crypto.0` |
| [demo-time-random.0](#demo-time-random0) | 3.2K | Time & RNG | `./bin/zero check demo-time-random.0` |
| [demo-comprehensive.0](#demo-comprehensive0) | 7.9K | All Features | `./bin/zero check demo-comprehensive.0` |

---

## 📦 demo-simple.0

**Quick feature overview** - The fastest way to see Zero in action.

### Features:
- ✅ Shapes (struct-like types)
- ✅ Base64 encoding
- ✅ CRC32 checksums
- ✅ Wall clock time
- ✅ Seeded random numbers
- ✅ Cryptographic hashing

### Key Code:
```zero
shape Point {
    x: i32,
    y: i32,
}

let p = Point { x: 10, y: 20 }
let sum = p.x + p.y  // Field access
```

---

## 📊 demo-encoding.0

**Complete data encoding suite** - All encoding/decoding utilities.

### Features:
- 🔤 **Base64 Encoding**: Binary-safe string encoding
- 🔢 **Hexadecimal**: Byte arrays to hex strings
- 🌐 **URL Encoding**: Percent-encoding for URLs
- ✔️ **CRC32**: Fast data integrity checksums
- 📏 **Varint**: Variable-length integer encoding
- 📄 **JSON**: Validation, parsing, token streaming

### Example Output:
```
1. Base64 Encoding:
   Input: 'Hello, Zero!'
   Base64: SGVsbG8sIFplcm8h

2. Hexadecimal Encoding:
   Input: 'ABC123'
   Hex: 414243313233

3. URL Encoding:
   Input: 'hello world & stuff'
   URL-encoded: hello%20world%20%26%20stuff
```

---

## 🔒 demo-crypto.0

**Security and cryptography** - Production-ready crypto primitives.

### Features:
- 🔐 **HMAC**: Message authentication codes
- ⏱️ **Constant-Time Comparison**: Prevents timing attacks
- 🎲 **Secure RNG**: Cryptographically secure randomness
- #️⃣ **Hash Functions**: CRC32 and cryptographic hashes

### Critical Security Pattern:
```zero
// ❌ NEVER do this for passwords/tokens:
if password1 == password2 { ... }

// ✅ ALWAYS use constant-time comparison:
if std.crypto.constantTimeEql(password1, password2) { ... }
// Prevents timing side-channel attacks!
```

---

## ⏱️ demo-time-random.0

**Time operations and randomness** - Deterministic and non-deterministic RNG.

### Features:
- ⏰ **Time Durations**: Create and manipulate time spans
- 🌍 **Wall Clock**: Unix timestamps
- 📊 **Monotonic Clock**: Measure elapsed time
- 🎯 **Seeded RNG**: Deterministic (reproducible tests!)
- 🎲 **Entropy Source**: Non-deterministic randomness

### Deterministic vs Non-Deterministic:
```zero
// Deterministic (same seed = same sequence)
let mut rng1 = std.rand.seed(42_u32)
let value1 = std.rand.nextU32(&mut rng1)  // Always same

let mut rng2 = std.rand.seed(42_u32)
let value2 = std.rand.nextU32(&mut rng2)  // value1 == value2

// Non-deterministic (different every time)
let random = std.rand.entropyU32()  // Unpredictable
```

---

## 🎯 demo-comprehensive.0

**All-in-one showcase** - Complete demonstration with all features.

### Sections:
1. 📦 Data Encoding (Base64, Hex, CRC32)
2. 🔒 Cryptography (HMAC, secure RNG, constant-time)
3. ⏱️ Time Operations (durations, wall clock, monotonic)
4. 🎲 Random Numbers (seeded, entropy)
5. 📐 Shapes & Methods (OOP-style)
6. 📄 JSON Processing (validation, tokenization)

### Example Output:
```
╔══════════════════════════════════════════╗
║   Zero Language - Full Demo Suite       ║
╚══════════════════════════════════════════╝

📦 SECTION 1: Data Encoding
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Base64: SGVsbG8sIFplcm8gTGFuZ3VhZ2Uh
  Hex('Zero'): 5a65726f
  CRC32 checksum computed ✓

🔒 SECTION 2: Cryptography
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ✓ Different data produces different hashes
  ✓ HMAC authentication code generated
  ✓ Constant-time password comparison (timing-safe)
  ✓ Cryptographically secure random generated
...
```

---

## 🚀 Running the Demos

### Individual Check:
```bash
./bin/zero check demo-simple.0
```

### Check All:
```bash
for demo in demo-{simple,encoding,crypto,time-random,comprehensive}.0; do
    echo "Checking $demo..."
    ./bin/zero check "$demo"
done
```

### JSON Output (for agents):
```bash
./bin/zero check --json demo-simple.0
```

### Build Executable (if supported):
```bash
./bin/zero build --emit exe --target wasm32-wasi demo-simple.0 --out demo
```

---

## 🎓 What You'll Learn

### 1. **Capability-Based I/O**
```zero
pub fun main(world: World) -> Void raises {
    check world.out.write("output")  // Explicit capability
}
```

### 2. **Fixed Buffers (No Hidden Allocation)**
```zero
let mut buf: [64]u8 = [0, ...]  // Stack buffer
let result = std.codec.base64Encode(buf, data)  // No heap!
```

### 3. **Explicit Error Handling**
```zero
fun risky() -> i32 raises { CustomError } {
    raise CustomError  // Explicit error
}

check risky()  // Propagate upward
```

### 4. **Maybe Type (Optional Values)**
```zero
let result: Maybe<String> = encode(data)
if result.has {
    check world.out.write(result.value)
}
```

### 5. **Shapes with Methods**
```zero
shape Counter {
    value: u32,
    
    fun increment(self: mutref<Self>) -> Void {
        self.value = self.value + 1
    }
}

let mut counter = Counter { value: 0 }
counter.increment()  // Method call
```

---

## 📚 Standard Library Coverage

### Modules Demonstrated:

| Module | Functions Used |
|--------|----------------|
| `std.codec` | `base64Encode`, `hexEncode`, `urlEncode`, `crc32`, `encodedVarintLen` |
| `std.crypto` | `hash32`, `hmac32`, `constantTimeEql`, `secureRandomU32` |
| `std.json` | `validate`, `streamTokens`, `writeString` |
| `std.time` | `seconds`, `ms`, `sub`, `wallSeconds`, `monotonic`, `asMsFloor` |
| `std.rand` | `seed`, `nextU32`, `entropyU32` |
| `std.mem` | `span`, `eql` |

---

## 🎯 Use Cases

### CLI Tools
- File processors
- Data converters
- Checksum utilities

### Security Applications
- Password verification
- Token generation
- HMAC authentication

### Testing Infrastructure
- Deterministic test data (seeded RNG)
- Reproducible scenarios
- Snapshot testing

### Data Pipelines
- Encoding/decoding
- Format conversion
- Validation

---

## ❓ About qlog

Zero doesn't have a built-in `qlog` (QUIC logging) library, but you can build one using:

```zero
// Custom structured logger
fun logEvent(world: ref<World>, event: String, data: String) -> Void raises {
    let ts = std.time.wallSeconds()
    
    // Build JSON manually
    check world.out.write("{\"event\":\"")
    check world.out.write(event)
    check world.out.write("\",\"data\":\"")
    check world.out.write(data)
    check world.out.write("\",\"ts\":")
    // ... add timestamp
    check world.out.write("}\n")
}
```

All the primitives you need are in the demos above!

---

## 🔥 Why Zero?

### For AI Agents:
- ✅ Structured compiler output (JSON)
- ✅ Predictable behavior (no surprises)
- ✅ Explicit everything (no hidden magic)
- ✅ Clear error messages with fixes

### For Developers:
- ✅ No hidden allocations
- ✅ Capability-based security
- ✅ Small, fast binaries
- ✅ Compile-time guarantees

---

## 📖 Next Steps

1. **Run the demos**: Start with `demo-simple.0`
2. **Read the code**: Each demo is well-commented
3. **Explore examples/**: 70+ more examples in the repo
4. **Build something**: Try creating your own tool!
5. **Read docs**: `npm run docs:dev` for full documentation

---

**Zero is the programming language for agents** - designed to be predictable, explicit, and machine-readable.

For more information: [README.md](README.md) | [AGENTS.md](AGENTS.md) | [DEMO_SUMMARY.md](DEMO_SUMMARY.md)
