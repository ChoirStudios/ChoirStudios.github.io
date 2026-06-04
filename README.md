# Choir Studios Website - Code Consistency & Coherence Guide

This document provides comprehensive guidelines for maintaining consistency and coherence across the Choir Studios website codebase. All developers must adhere to these standards when contributing to this project.

---

## 1. Project Architecture Overview

### File Organization
The project follows a modular component-based structure:

- **Header Components**: header.html, header.css - Navigation and branding
- **Footer Components**: footer.html, footer.css - Site footer navigation and information
- **Main Styling**: main.css - Global styles, typography, theme variables, and scrollbar customization
- **Main Content**: index.html - Primary landing page structure
- **Secondary Pages**: internal_relations.html, 	team_commitment_terms.html - Policy and information pages
- **JavaScript Utilities**: include.js - HTML component inclusion via fetch API
- **HTML Entry Point**: index.html - Primary document structure

### Component Inclusion Pattern
All reusable HTML components (header, footer) are loaded dynamically using the include-html attribute:
```html
<header include-html="header.html"></header>
<footer include-html="footer.html"></footer>
```
This ensures consistency across all pages without code duplication.

---

## 2. CSS Architecture & Style Organization

### 2.1 Global CSS Structure

All CSS files must follow this mandatory section ordering:

1. **Reset** (main.css only)
   - Global margin/padding resets
   - Font family defaults
   - Border and text-decoration resets

2. **Variables** (main.css only)
   - CSS custom properties for theme colors
   - Format: --{UUID}: {value}
   - Example: --6b1ee82f-3cb6-4887-b0fd-57c6d8aeea60: #000000;

3. **Styling**
   - Colors and backgrounds
   - Text decoration and styling
   - Visual appearance properties

4. **Behaviour**
   - Animations and transitions
   - Overflow and scroll behavior
   - Cursor and pointer events
   - Hover states

5. **Layout**
   - Display properties (flexbox, grid)
   - Positioning and alignment
   - Margin and padding
   - Gap and spacing

6. **Blocks**
   - Font sizes and weights
   - Line heights
   - Sizes (width, height)
   - Text alignment
   - Individual element dimensions

### 2.2 Color & Theme Variables

**Requirement**: All colors MUST be defined as CSS variables using UUID-based naming.

**UUID Format**: --{32-character-hex-uuid} (with hyphens as shown in existing code)

**Current Theme Variables** (from main.css):
```css
--0cbc23af-08ba-4603-b0d4-faf972f132fb: #6e0f83;    /* Purple accent */
--d53153c5-2bb6-4e80-8a94-25578440e6e1: #fefefe;   /* Light text/background */
--6b1ee82f-3cb6-4887-b0fd-57c6d8aeea60: #000000;   /* Dark text/background */
--34ace198-814d-4bdd-a836-f8d0bd5d73f1: #fdf8ff;   /* Light purple variant */
--fab086d1-ad61-4538-9c7f-c8f016f77d40: #f8f4fa;   /* Very light purple variant */
```

**Usage Rules**:
- Always reference colors through variables, never hardcoded hex values
- Apply colors to specific elements, not parent containers
- Example: footer { background-color: var(--6b1ee82f-3cb6-4887-b0fd-57c6d8aeea60); }

### 2.3 Selector Specificity & Element Targeting

**Critical Rule**: Do NOT use parent-child selectors for styling. Always target elements individually with class names or IDs.

**INCORRECT**:
```css
footer nav ul li a { color: #fff; }
div section h2 { font-size: 2.5vw; }
```

**CORRECT**:
```css
footer nav ul a { color: var(--d53153c5-2bb6-4e80-8a94-25578440e6e1); }
footer #title h1 { font-size: 7.9vw; }
```

**Rationale**: Direct element targeting ensures:
- Easier style management
- Lower CSS specificity conflicts
- Clearer intent and maintainability
- Predictable cascade behavior

### 2.4 Spacing & Padding Guidelines

**Critical Rule**: Apply padding only to the elements that contain content, never to parent containers for spacing between children.

**INCORRECT**:
```css
section { padding: 5vw; }  /* Then override for every child */
```

**CORRECT**:
```css
section content { padding: 2vw; }
section img { padding: 1vw; }
```

**Rationale**: 
- Padding on content elements provides explicit, predictable spacing
- Enables per-element control
- Prevents layout cascading issues

### 2.5 Responsive Units

Use **viewport-relative units** (vw, vh) for all sizing to maintain responsive design:
- padding: 1.2vw 3vw; - Responsive padding
- font-size: 2vw; - Responsive typography
- width: 24vw; - Responsive dimensions
- border-radius: 1vw; - Responsive borders

**Rationale**: Viewport units scale proportionally across all screen sizes without media queries for basic sizing.

---

## 3. HTML Structure Guidelines

### 3.1 Semantic HTML

Use semantic elements appropriately:
- <header> for site header navigation
- <main> for primary content
- <footer> for site footer
- <section> for distinct content sections
- <nav> for navigation menus
- <content> for custom content grouping (as used in this project)

### 3.2 Component Structure

**Header** (`header.html`):
- Logo image in dedicated section
- Navigation list with linked items
- Patreon button call-to-action
- Title section with h1

**Footer** (`footer.html`):
- Logo image
- Navigation menu with internal links
- Consistent structure matching header

**Main Content** (`index.html`):
- Alternating section layouts with image and content
- Content always wrapped in `<content>` tags
- Images paired with text sections
- Consistent section structure for visual rhythm

### 3.3 Accessibility & Attributes

**Required**:
- All images must have descriptive `alt` attributes
- Use semantic HTML elements
- Maintain logical heading hierarchy (h1  h2  h3, etc.)
- Use `id` attributes for component targeting in CSS

**Example**:
```html
<section id="title">
  <h1>CHOIR STUDIOS</h1>
</section>

<img src="wilyicaro_waving.png" alt="Wilyicaro character waving" />
```

---

## 4. JavaScript Practices

### 4.1 HTML Component Inclusion

**Pattern**: Use `include.js` for dynamic HTML component loading

**How it Works**:
```javascript
function includeHTML() {
  var includeElements = document.querySelectorAll('[include-html]');
  
  includeElements.forEach(function(element) {
      var filePath = element.getAttribute('include-html');
      
      if (filePath) {
          fetch(filePath)
          .then(response => response.text())
          .then(html => { element.innerHTML = html; })
          .catch(error => { console.error('Failed to load file: ' + filePath, error); });
      }
  });
}

document.addEventListener('DOMContentLoaded', includeHTML);
```

**Usage**:
```html
<header include-html="header.html"></header>
<footer include-html="footer.html"></footer>
<script src="include.js"></script>
```

**Benefits**:
- Single source of truth for header/footer
- Automatic component updates across all pages
- No code duplication
- Centralized maintenance

### 4.2 Error Handling

Always include error logging:
```javascript
.catch(error => {
    console.error('Failed to load file: ' + filePath, error);
});
```

---

## 5. Document Structure Consistency

### 5.1 HTML Document Template

All pages MUST follow this structure:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="header.css" />
    <link rel="stylesheet" href="main.css" />
    <link rel="stylesheet" href="footer.css" />
    <link rel="website icon" type="png" href="logo.png" />
    <title>Choir Studios - [Page Name]</title>
  </head>
  <body>
    <header include-html="header.html"></header>
    <main>
      <!-- Page-specific content -->
    </main>
    <footer include-html="footer.html"></footer>
    <script src="include.js"></script>
  </body>
</html>
```

### 5.2 CSS Import Order

Maintain this stylesheet order in all HTML files:
1. `header.css` - Navigation and header styling
2. `main.css` - Global variables, reset, main content styling
3. `footer.css` - Footer-specific styling

**Rationale**: Ensures proper CSS cascade and prevents specificity conflicts.

### 5.3 Script Loading

- Always load `include.js` LAST, after HTML elements are parsed
- Use `document.addEventListener('DOMContentLoaded', includeHTML);`

---

## 6. Naming Conventions

### 6.1 Class & ID Naming

- Use descriptive, lowercase names
- Use hyphens for multi-word names (kebab-case)
- Examples: `
av-menu`, `main-content`, `footer-section`

### 6.2 CSS Variable Naming

- Use UUID-based variable names for colors
- Format: `--{UUID}: {hex-value};`
- Document the color's purpose in comments if not obvious

### 6.3 File Naming

- Use lowercase with hyphens
- Component files: `{component}.html`, `{component}.css`
- Examples: `header.html`, `footer.css`, `include.js`

---

## 7. Styling Patterns in Practice

### 7.1 Footer Example (footer.css)

This file exemplifies all guidelines:

```css
/* Styling - Colors and backgrounds */
footer {
  background-color: var(--6b1ee82f-3cb6-4887-b0fd-57c6d8aeea60);
}

footer * {
  color: var(--d53153c5-2bb6-4e80-8a94-25578440e6e1);
}

/* Behaviour - Interactions */
footer nav button:hover {
  cursor: pointer;
  background: var(--34ace198-814d-4bdd-a836-f8d0bd5d73f1);
}

/* Layout - Flexbox and spacing */
footer nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.2vw 3vw;
}

/* Blocks - Sizes and typography */
footer nav button {
  font-size: 1vw;
  padding: 1.2vw 5vw;
}
```

### 7.2 Main Content Example (main.css)

Demonstrates color variables, responsive units, and section styling:

```css
/* Variables */
:root {
  --6b1ee82f-3cb6-4887-b0fd-57c6d8aeea60: #000000;
  --d53153c5-2bb6-4e80-8a94-25578440e6e1: #fefefe;
}

/* Styling */
main section:nth-child(even) {
  background: var(--34ace198-814d-4bdd-a836-f8d0bd5d73f1);
}

/* Layout */
main section {
  padding: 5vw 11vw;
  display: flex;
  flex-direction: row;
  gap: 2vw;
}

/* Blocks */
main section h2 {
  font-size: 2.5vw;
  line-height: 2vw;
}
```

---

## 8. Maintenance & Updates

### 8.1 Adding New Components

1. Create `{component}.html` with semantic structure
2. Create `{component}.css` following the 6-section format
3. Add import in main HTML files
4. Use `include-html` attribute for reusable components
5. Update this documentation

### 8.2 Modifying Existing Components

1. Maintain the 6-section CSS structure
2. Use existing color variables
3. Test across all pages using the component
4. Update documentation if patterns change

### 8.3 Adding New Colors

1. Generate new UUID-based variable name
2. Add to `:root` in `main.css`
3. Document the color's purpose
4. Use throughout project consistently

---

## 9. Quality Checklist

Before committing changes, verify:

- [ ] All colors use CSS variables (no hardcoded hex)
- [ ] CSS sections follow required order (Styling  Behaviour  Layout  Blocks)
- [ ] No parent-child selectors used for styling
- [ ] Padding applied to content elements, not containers
- [ ] Responsive units (vw/vh) used for sizing
- [ ] All images have descriptive alt attributes
- [ ] HTML documents include proper meta tags and DOCTYPE
- [ ] Stylesheet import order maintained (header  main  footer)
- [ ] `include.js` loaded after all HTML elements
- [ ] Component structure consistent across pages
- [ ] Documentation updated for new patterns

---

## 10. Technology Stack

- **Markup**: HTML5
- **Styling**: CSS3 (Custom Properties, Flexbox, Responsive Units)
- **Interactivity**: Vanilla JavaScript (ES6+)
- **Fonts**: Merriweather, Public Sans (Google Fonts)
- **Component Loading**: Fetch API with error handling
- **Responsive Design**: Viewport-relative units (vw/vh)

---

## Quick Reference

### CSS Section Order
1. Reset
2. Variables
3. Styling
4. Behaviour
5. Layout
6. Blocks

### Color Variables Template
```css
:root {
  --{UUID}: {hex-value};  /* Purpose */
}
```

### Component Inclusion
```html
<header include-html="header.html"></header>
<script src="include.js"></script>
```

### Responsive Units
- Padding: `1.2vw 3vw`
- Font size: `2vw`
- Dimensions: `24vw`
- Border radius: `1vw`

---

**Last Updated**: January 2026  
**Version**: 1.0  
**Project**: KYBSTU Website - Choir Studios

