# yaml

[![CI](https://github.com/alya-lang/yaml/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/yaml/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/alya-lang/yaml?color=blue&label=License)](LICENSE)
[![Alya](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fyaml%2Fmain%2Falya.toml&query=%24.package.alya-version&label=Alya&color=orange&prefix=%3E%3D)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fyaml%2Fmain%2Falya.toml&query=%24.package.version&label=Version&color=brightgreen)](alya.toml)

Fast, zero-dependency YAML 1.2 parser and serializer for Alya

---

## 🌟 Features

- ⚡ **Fast & Zero-Dependency**: Pure native Alya implementation parsing over 750,000 documents per second without external dependencies
- 🧩 **Comprehensive Specification**: Supports block mappings, block sequences, nested hierarchies, compact inline sequence-mappings (`- key: val`), and multi-document YAML (`---`)
- 🌊 **Flow-Style Collections**: Full support for inline JSON-compatible sequences (`[1, 2, 3]`) and mappings (`{ a: 1, b: 2 }`)
- 📜 **Multiline Block Scalars**: Supports literal block scalars (`|`) preserving newlines and folded block scalars (`>`)
- 🛡️ **Typed Literals & Comments**: Handles strings (single/double/unquoted), integers (dec, hex `0x`, oct `0o`, bin `0b`), floats, booleans, nulls (`null`, `~`), and full-line/inline comments (`#`)
- 🧪 **Well Tested**: Comprehensive test suite, roundtrip verification, and realistic micro-benchmarks

---

## 📁 Project Architecture

```
yaml/
├── alya.toml               # Package manifest
├── src/
│   ├── lib.alya            # Public API facade
│   ├── types.alya          # Type constants, YamlDoc struct & string helpers
│   ├── scalar.alya         # Typed scalar parser, comment stripper & scalar formatter
│   ├── flow.alya           # Flow-style inline sequence & mapping parser
│   ├── parser.alya         # Indentation-aware block parser & multi-doc splitter
│   └── serializer.alya     # Recursive YAML emitter & block formatter
├── examples/
│   └── demo.alya           # Docker Compose & Kubernetes deployment demo
├── tests/
│   ├── test_basic.alya     # Scalars, numbers, booleans, comments & file I/O tests
│   ├── test_nested.alya    # Hierarchical mappings, sequences & multi-doc tests
│   ├── test_flow.alya      # Flow collections & multiline block scalar tests
│   └── test_serializer.alya# Serializer & roundtrip verification tests
└── benches/
    └── bench_basic.alya    # Parse, stringify & roundtrip micro-benchmarks
```

> [!NOTE]
> **Modular Source:** Modules are cleanly separated inside `src/` (`types.alya`, `scalar.alya`, `flow.alya`, `parser.alya`, `serializer.alya`) and unified under the `src/lib.alya` facade for zero-overhead imports.

---

## 📦 Installation

Add `yaml` to the `[dependencies]` section in your `alya.toml`:

```toml
[dependencies]
yaml = { git = "https://github.com/alya-lang/yaml", branch = "main" }
```

Or install it directly using the Alya package CLI:

```bash
alyac add yaml --git https://github.com/alya-lang/yaml --branch main
alyac install
```

---

## 🚀 Quick Start

```alya
import "yaml" as yaml

function main()
    let config_yaml = "server:\n"
    config_yaml += "  host: localhost\n"
    config_yaml += "  port: 8080\n"
    config_yaml += "tags: [ 'api', 'v1' ]\n"

    # Parse YAML into native Alya data structures
    let doc = yaml::parse(config_yaml)
    say "Host: " + doc["server"]["host"]
    say "Port: " + str(doc["server"]["port"])

    # Serialize native data back to formatted YAML
    let out = yaml::stringify(doc, 2)
    say out
end

main()
```

---

## 📖 API Reference

| Function | Arguments | Returns | Description |
|---|---|---|---|
| `parse(yaml_str)` | `yaml_str: string` | `map / array / scalar` | Parses a YAML document string into native Alya data structures. |
| `parse_all(yaml_str)` | `yaml_str: string` | `array` | Parses a multi-document YAML string (separated by `---`) into an array of documents. |
| `stringify(data, indent_size)` | `data, indent_size = 2` | `string` | Serializes an Alya data structure into a formatted YAML document string. |
| `dump(data, indent_size)` | `data, indent_size = 2` | `string` | Alias for `stringify`. |
| `load_file(file_path)` | `file_path: string` | `map / array / scalar` | Reads a YAML file from disk and parses its content. |
| `dump_file(file_path, data, indent_size)` | `file_path: string, data, indent_size = 2` | `void` | Serializes data and writes it to a YAML file on disk. |

---

## 🧪 Running Tests & Benchmarks

Run the test suite using `alyac`:

```bash
alyac test
```

Run the benchmark suite:

```bash
alyac run benches/bench_basic.alya
```

Run the example demo:

```bash
alyac run examples/demo.alya
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository and clone it locally
2. Install dependencies:
   ```bash
   alyac install
   ```
3. Create your feature branch (`git checkout -b feature/my-feature`)
4. Verify tests and formatting before opening a PR:
   ```bash
   alyac test
   alyac fmt . --check
   ```
5. Commit your changes (`git commit -m "feat: add feature"`) and open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.