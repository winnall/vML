# vML – Visual Markup Language

**vML** is a *visual, indentation-based, typographically structured language* for representing hierarchical data.  
It provides a clean, human-readable alternative to XML while remaining fully compatible with both **XML** and **YAML** serialisations.

> **Version:** 0.5 (draft)  
> **Specification:** [vml-core-specification-v0.5.md](./vml-core-specification-v0.5.md)  
> **Author:** © 2025 Stephen Winnall et al.

---

## ✨ Overview

vML uses **visual cues** — indentation, font style, and layout — instead of angle brackets and closing tags.  
It retains XML’s structure and expressiveness but reads with YAML’s calm simplicity.

Every valid YAML document is also valid vML.  
Every vML document can be losslessly serialised to canonical XML.

---

## 🧩 Core Features

| Concept | Example | Description |
|----------|----------|-------------|
| **Elements** | `note(date 2025-10-26)` | Parentheses for attributes |
| **Attributes** | `(priority high)` | No `=`, commas optional on newlines |
| **Hierarchy** | Indentation defines structure | |
| **Text** | inline or as block scalars (`|`, `>`) | |
| **Comments** | `# comment` | YAML-compatible |
| **Processing instructions** | `?xml(version "1.0", encoding "UTF-8")` | XML-compatible |
| **Namespaces** | `xs:schema(...)` | Retained verbatim |
| **Round-trip safe** | YAML ⊂ vML ⊂ XML | |

---

## 🧱 Design Principles

- **Visual-first**: The rendered, typographic form is canonical.  
- **Declarative**: No procedural logic or embedded scripting.  
- **Compatible**: Serialises to valid XML and YAML.  
- **Tool-neutral**: Readable in plain text, renderable in styled environments.  
- **Extensible**: Uses annexes for advanced features (DTD, CSS, preprocessors, etc.).

---

## 📘 Specification Contents

| Section | Title | Description |
|----------|--------|-------------|
| **1–10** | Core Language | Syntax, grammar, types, examples, errors |
| **Annex A** | Extended Features | DTDs, Entities, Anchors, Tags, Multi-docs, XInclude |
| **Annex B** | Preprocessors and Pipelines | XSLT, XQuery, Jinja2, workflow chaining |
| **Annex C** | Rendering and CSS Styling | CSS selectors, pseudo-elements, linked/inline styles |
| **Annex D** | Stylesheet Preprocessors | Sass/LESS integration, MIME types, multiple outputs |

Future annexes (E–F) may define schema validation and native templating.

---

## 🎨 Rendering

vML can be rendered using a **CSS stylesheet** that assigns fonts and colours to semantic roles:

```css
element { font-weight: bold; }
element::attr(name) { font-style: italic; color: gray; }
element::attr(value) { color: darkgreen; }
```

In the absence of an explicit `?vml-stylesheet()`, the default `vml-default.css` is applied.

---

## ⚙️ Preprocessor Compatibility

vML is preprocessor-agnostic:

| Type | Example | Integration |
|------|----------|-------------|
| **Textual** | Jinja2, Mustache | Run before parsing vML |
| **Structural** | XSLT, XQuery | Run after serialisation to XML |
| **Stylesheet** | Sass, LESS | Compiled to CSS before rendering |

Renderers merge multiple CSS outputs into a single cascade using deterministic rules.

---

## 🚀 Goals

- Provide a **human-readable** visual markup language for structured data.  
- Enable **seamless transformation** through existing XML and YAML ecosystems.  
- Define a **standardised rendering model** using CSS or Sass themes.  
- Serve as a **notation** for visual editing tools and document pipelines.

---

## 🧭 Roadmap

| Version | Focus |
|----------|--------|
| **0.5** | Core spec + Annexes A–D (current) |
| **0.6** | Schema & validation model (Annex E) |
| **0.7** | Native vML templating syntax (Annex F) |
| **1.0** | Stable reference implementation and renderer |

---

## 🧑‍💻 Contributing

Contributions are welcome — discussion and refinement are open.  
Ideas for extensions, implementations, and renderers are particularly encouraged.

Please:
1. Fork this repository  
2. Create a feature branch (`git checkout -b feature/your-topic`)  
3. Commit your changes and open a Pull Request

---

## 📜 License

This draft is released under a permissive open documentation license.  
Use and modify freely with attribution.

---

> **vML** — _Visual structure, textual compatibility, human clarity._
