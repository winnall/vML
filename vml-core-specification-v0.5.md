# vML Core Language Specification  
*(Visual Markup Language)*

Version 0.5 (draft)  
© 2025 Stephen Winnall et al.

---

## 1  Overview

**vML** — the **Visual Markup Language** — is a *visual, indentation-based, typographically structured language* for representing hierarchical data.  
It uses font, spacing, and layout as primary syntactic cues rather than punctuation.  

**XML** and **YAML** are both *textual serialisations* of vML — each encodes the same structure, but neither is its native form.  
vML’s own representation is inherently visual; text-based serialisations exist for compatibility, persistence, and interchange.

Every valid YAML document is valid vML.  
Every valid vML document can be serialised deterministically to canonical XML.

---

## 2  Syntax summary

| Construct | Syntax | Notes |
|------------|---------|-------|
| **Element** | `name[(attr value[, attr value]...)] [inline text]` | Indentation defines hierarchy. |
| **Attributes** | Parentheses after tag. No `=`. Commas required only when two or more attributes share a line. |
| **Text** | Inline, quoted, or YAML block scalar. |
| **Comments** | `# comment` (YAML syntax) |
| **Processing instructions** | `?target(attr value[, attr value]...)` |
| **CDATA / literal text** | `|` (YAML literal block) → CDATA section on serialisation |
| **Folded text** | `>` (YAML folded block) → escaped text node |
| **Empty element** | tag without content |
| **Mixed content** | alternate text and child elements within one block |
| **Namespaces** | prefixes retained verbatim (`xs:schema`) |
| **Lists / siblings** | repeated keys or YAML sequence (`-`) |
| **Processing order** | indentation = nesting = XML structure |

---

## 3  Lexical rules

- Indentation: spaces only.  
- Tag and attribute names: valid XML Names (may include `:`, `-`, `_`).  
- Attribute values: YAML scalars (bare token or quoted string).  
- Quoting: YAML’s rules apply; double or single quotes allowed.  
- Whitespace between tokens separates them; commas delimit inline pairs.  
- vML implementations **may** render elements, attributes, and literals in distinct fonts or colours.

---

## 4  Grammar (EBNF)

```
document       ::= node+
node           ::= element | comment | pi
element        ::= tag attributes? ( inline_text | block )?
tag            ::= NAME ( ":" NAME )*
attributes     ::= "(" attrlist ")"
attrlist       ::= attribute ( "," attribute )*
                 | NEWLINE INDENT attribute ( NEWLINE INDENT attribute )*
attribute      ::= NAME ( ":" NAME )* WS value
value          ::= scalar
scalar         ::= QUOTED | BARE
inline_text    ::= WS text_scalar
block          ::= NEWLINE INDENT content_block
content_block  ::= ( node | text_line )+
text_line      ::= QUOTED | YAML_BLOCK_SCALAR
comment        ::= "#" .*$ 
pi             ::= "?" NAME attributes?
```

---

## 5  Typing and scalars

YAML type inference is preserved,  
but all attribute and text values are **serialised as strings** in XML.

| vML literal | YAML type | XML attribute/text |
|---------------|------------|--------------------|
| `42` | int | `"42"` |
| `3.14` | float | `"3.14"` |
| `"1.0"` | string | `"1.0"` |
| `true` / `false` | bool | `"true"` / `"false"` |
| `#abc` | string | `"#abc"` |

---

## 6  Examples

### 6.1  Simple element
```vml
note(date 2025-10-26, priority high)
  to Alice
  from Bob
  body Lunch?
```

→
```xml
<note date="2025-10-26" priority="high">
  <to>Alice</to><from>Bob</from><body>Lunch?</body>
</note>
```

### 6.2  Nested structure
```vml
article(lang en)
  title Compact Notation for XML
  author(id sw)
    name Stephen Winnall
  body
    "A lightweight representation..."
```

### 6.3  Namespaces and schema fragment
```vml
xs:schema(
  xmlns:xs http://www.w3.org/2001/XMLSchema,
  targetNamespace http://www.w3.org/2001/XMLSchema,
  elementFormDefault qualified
)
  xs:simpleType(name string, final #all)
    xs:restriction(base xs:anySimpleType)
  xs:simpleType(name boolean, final #all)
    xs:restriction(base xs:anySimpleType)
      xs:pattern(value "0|1|true|false")
```

### 6.4  Mixed content
```vml
p
  "This is "
  b strong
  " text."
```

### 6.5  Processing instructions
```vml
?xml(version "1.0", encoding "UTF-8")
?xml-stylesheet(type "text/xsl", href "style.xsl")
```

### 6.6  Literal blocks (CDATA)
```vml
script(type text/javascript)
  |
    if (a < b && b > c) {
      console.log("Hello");
    }
```

### 6.7  Folded text
```vml
body
  >
    Lines are folded
    into a single string.
```

---

## 7  CDATA and block text semantics

| vML form | YAML scalar | XML output | Escaping |
|------------|--------------|-------------|-----------|
| `|` block | literal | CDATA (recommended) | none |
| `>` block | folded | normal text | escaped |
| inline | plain | normal text | escaped |

---

## 8  Canonical serialisation rules

1. Empty element → `<tag/>`.  
2. Attribute order unspecified (unless canonicalised).  
3. Namespace prefixes preserved verbatim.  
4. Literal block (`|`) → CDATA or unescaped text.  
5. Comments and PIs → XML forms (`<!-- ... -->`, `<?...?>`).  
6. Indentation irrelevant to XML semantics.  

---

## 9  Errors

- Missing comma between inline attributes.  
- Attribute name without value.  
- Inconsistent indentation.  
- Duplicate attribute names.  
- Invalid XML names.

---

## 10  Summary

- **YAML ⊂ vML ⊂ XML** (semantics).  
- Parentheses for attributes unify syntax across elements and PIs.  
- YAML block scalars map naturally to XML text and CDATA.  
- Every vML document serialises cleanly to XML and round-trips without loss.  
- The **visual form** of vML is canonical; textual serialisations are interoperable encodings.

---

## Annex A – Extended Features

(Full content omitted here for brevity: DTDs, Entities, Anchors, Tags, Multi-docs, XInclude)

---

## Annex B – Preprocessors and Pipelines

(Full content omitted here for brevity: XSLT, Jinja2, workflows, chaining)

---

## Annex C – Rendering and CSS Styling

(Full content omitted here for brevity: CSS rendering, pseudo-elements, selectors, linked stylesheets)

---

## Annex D – Stylesheet Preprocessors

(Full content omitted here for brevity: Sass/LESS integration, MIME types, multi-output rules)
