# CSS Architecture Technical Documentation

## CSS File Structure Overview

The Treact stylesheet follows a systematic approach combining modern CSS techniques with responsive design principles. The architecture prioritizes maintainability, performance, and cross-browser compatibility.

---

## Font System and Typography Foundation

### Google Fonts Integration
```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400;500;600;700;800;900&display=swap');
```

**Technical Implementation:**
- **`@import` placement**: Must be first rule in CSS file per specification
- **Weight range**: 100-900 covers all typographic needs (thin to black)
- **`display=swap`**: Performance optimization - shows fallback font until Inter loads
- **Variable font loading**: Single request loads all weights, reducing HTTP calls

### Global Typography Reset
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Inter', system-ui;
    color: #243e63;
}
```

**Universal Selector Strategy:**
- **`margin: 0; padding: 0`**: Eliminates browser default spacing inconsistencies
- **`box-sizing: border-box`**: Makes width/height calculations include padding/border
- **Font stack**: `'Inter', system-ui` provides fallback to system fonts if web font fails
- **Global color**: `#243e63` (dark blue-gray) ensures consistent text color hierarchy

**Performance Impact:**
- Universal selector has minimal performance cost in modern browsers
- Box-sizing change affects all elements but prevents layout calculation errors
- System font fallback ensures text remains readable during font loading

---

## Layout System Architecture

### Body and Viewport Control
```css
body {
    max-width: 100vw;
    overflow-x: hidden;
}
```

**Viewport Management:**
- **`max-width: 100vw`**: Prevents horizontal overflow on mobile devices
- **`overflow-x: hidden`**: Eliminates horizontal scrollbar for edge cases
- **Mobile Safari fix**: Prevents zoom issues when rotating device

### Container System
```css
.row {
    width: 100%;
    max-width: 1280px;
    margin: 0 auto;
}

.row__narrow {
    width: 100%;
    max-width: 1024px;
    margin: 0 auto;
}

.container {
    padding: 96px 0;
}
```

**Layout Philosophy:**
- **`.row`**: Primary content container with 1280px max-width for wide screens
- **`.row__narrow`**: Tighter container (1024px) for better readability on large displays
- **Auto margins**: `margin: 0 auto` centers containers horizontally
- **`.container`**: Vertical spacing system (96px top/bottom) creates consistent section rhythm

**Responsive Behavior:**
- Containers automatically shrink on smaller screens
- Consistent max-widths prevent content from becoming too wide on large monitors
- Padding system maintains vertical rhythm across all sections

---

## Color System and Design Tokens

### Primary Color Usage
```css
.purple {
    color: #6415ff;
}
```

**Brand Color Strategy:**
- **`#6415ff`**: Primary brand purple used consistently across components
- **Utility class approach**: Single-purpose class for color application
- **Semantic naming**: `.purple` clearly indicates visual purpose

### Section Styling Pattern
```css
section {
    padding: 0 32px;
}

main {
    margin-bottom: 96px;
}
```

**Spacing System:**
- **Horizontal padding**: 32px provides consistent edge spacing on all sections
- **Main margin**: 96px bottom creates separation between main content and footer
- **Mobile consideration**: 32px padding sufficient for mobile touch zones

---

## Navigation System Deep Dive

### Navigation Container
```css
nav {
    display: flex;
    padding-top: 32px;
}

.nav__row {
    display: flex;
    justify-content: space-between;
}
```

**Layout Mechanics:**
- **Flexbox implementation**: Creates flexible, responsive navigation layout
- **`justify-content: space-between`**: Automatically distributes logo and links to opposite ends
- **Top padding**: 32px provides breathing room from viewport edge

### Logo Component
```css
.nav__logo {
    display: flex;
    align-items: center;
    margin: 8px 0;
    padding-bottom: 4px;
}
```

**Component Design:**
- **Flexbox alignment**: Centers logo image and text vertically
- **Margin/padding**: Fine-tunes vertical positioning within navigation
- **Modular approach**: Self-contained logo component for reusability

### Mobile Menu Button
```css
.btn__menu {
    display: none;
    background: transparent;
    border: none;
    transition: all 300ms ease;
}

.btn__menu--svg:hover {
    filter: invert(26%) sepia(94%) saturate(7476%) hue-rotate(261deg) brightness(94%) contrast(115%);
}
```

**Interactive Design:**
- **Hidden by default**: `display: none` hides on desktop, shown via media query
- **Transparent styling**: No background/border for clean icon appearance
- **CSS filter hover**: Complex filter creates purple color effect on SVG
- **Smooth transitions**: 300ms ease creates polished interaction feel

**CSS Filter Breakdown:**
- **`invert(26%)`**: Inverts colors partially
- **`sepia(94%)`**: Adds sepia tone for color base
- **`saturate(7476%)`**: Heavily saturates for vibrant color
- **`hue-rotate(261deg)`**: Rotates to purple hue
- **`brightness/contrast`**: Fine-tunes final color appearance

---

## Modal System Implementation

### Modal Container
```css
.modal {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    height: 378px;
    margin: 24px 16px;
    padding: 32px;
    background-color: #fff;
    border: 1px solid rgb(229, 231, 235);
    border-radius: 8px;
    z-index: 4;
    visibility: hidden;
    opacity: 0;
    transform: translateX(300%);
    transition: all 300ms ease;
}
```

**Technical Breakdown:**
- **Fixed positioning**: Overlays viewport regardless of scroll position
- **Stretch positioning**: `top: 0; left: 0; right: 0` creates full-width base
- **Fixed height**: 378px provides consistent modal size
- **Margin system**: 24px top, 16px sides create edge spacing
- **Border styling**: Subtle gray border matches design system
- **Z-index**: Layer 4 ensures modal appears above all content

**Animation System:**
- **Initial state**: Hidden (`visibility: hidden, opacity: 0`)
- **Transform**: `translateX(300%)` positions off-screen to right
- **Transition**: Smooth 300ms animation for all properties

### Modal Content Layout
```css
.modal__links {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.modal__link {
    color: #1a202c;
    font-size: 18px;
    font-weight: 600;
    line-height: 1.5;
    margin: 8px 0;
    padding-bottom: 4px;
    border-bottom: 2px solid transparent;
}

.modal__link--primary {
    color: #f7fafc;
    padding: 12px 32px;
    background: #6415ff;
    border-radius: 500px;
    border: none;
}
```

**Layout Strategy:**
- **Full container**: Links container fills entire modal space
- **Column flexbox**: Vertical stack with center alignment
- **Link styling**: Consistent typography with transparent border for hover states
- **Primary CTA**: Distinguished with background color and rounded styling

### Modal Animation States
```css
.menu--open .modal {
    visibility: visible;
    opacity: 1;
    transform: translateX(0);
}

.btn__menu--close {
    position: fixed;
    top: 24px;
    right: 18px;
}
```

**State Management:**
- **`.menu--open` class**: Applied to body via JavaScript to trigger modal
- **Transform reset**: `translateX(0)` slides modal into view
- **Visibility/opacity**: Makes modal visible and interactive
- **Close button**: Fixed position in top-right corner for easy access

---

## Section Layout Patterns

### Header Layout System
```css
header .row {
    display: flex;
    align-items: center;
    justify-content: space-between;
}
```

**Two-Column Strategy:**
- **Flexbox layout**: Creates responsive side-by-side arrangement
- **Center alignment**: Vertically centers content columns
- **Space distribution**: `space-between` creates automatic gap

### Features Section
```css
#features {
    position: relative;
}

#features .row__narrow {
    display: flex;
    flex-direction: column;
    align-items: center;
}
```

**Centered Layout:**
- **Relative positioning**: Allows for absolute positioned decorative elements
- **Column flexbox**: Stacks header and content vertically
- **Center alignment**: Creates symmetrical centered design

### Two-Column Sections Pattern
```css
#quality .row,
#steps .row,
#values .row {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
```

**Consistent Layout:**
- **Multiple selectors**: DRY principle - same styling for similar sections
- **Flexbox implementation**: Reliable cross-browser layout method
- **Center alignment**: Ensures content alignment regardless of height differences

---

## Pricing Section Architecture

### Pricing Container
```css
#pricing .row {
    display: flex;
    flex-direction: column;
    align-items: center;
}
```

**Centered Stack:**
- **Column direction**: Stacks header above pricing cards
- **Center alignment**: Creates symmetrical pricing presentation
- **Flexible container**: Adapts to content height automatically

### Testimonials Layout
```css
#testimonials .row {
    display: flex;
}
```

**Simple Flexbox:**
- **Default flex direction**: Row layout for side-by-side content
- **Implicit behavior**: Flex items automatically size to content

---

## Call-to-Action Section

### CTA Container
```css
#start .row {
    position: relative;
    overflow: hidden;
}
```

**Container Setup:**
- **Relative positioning**: Allows for absolutely positioned decorative elements
- **Overflow hidden**: Clips any decorative elements that extend beyond bounds
- **Foundation for effects**: Prepared for background patterns or animations

---

## Footer Styling

### Footer Container
```css
footer {
    padding: 96px 32px;
    background-color: rgb(100, 21, 255);
    margin-bottom: -32px;
    position: relative;
    overflow: hidden;
}
```

**Footer Design:**
- **Large padding**: 96px vertical creates substantial footer presence
- **Brand background**: Purple background reinforces brand identity
- **Negative margin**: -32px pulls footer up slightly for visual connection
- **Overflow hidden**: Prevents decorative elements from extending outside

---

## Responsive Design System

### Breakpoint Strategy
```css
@media(max-width: 1280px) {
    header .row {
        align-items: flex-end;
    }
}

@media (max-width: 1024px) {
    .btn__menu {
        display: block;
    }
    header .row {
        flex-direction: column;
        align-items: center;
    }
    footer {
        padding: 80px 32px;
    }
}
```

**Breakpoint Logic:**
- **1280px**: Minor adjustments for large tablets/small desktops
- **1024px**: Major layout shift - mobile menu appears, columns stack
- **Desktop-first approach**: Styles cascade down with mobile overrides

### Mobile Layout Transformations
```css
@media(max-width: 768px) {
    #quality .row,
    #values .row {
        flex-direction: column-reverse;
    }
    #steps .row,
    #testimonials .row {
        flex-direction: column;
        align-items: center;
    }
    #pricing .row {
        margin-top: 40px;
    }
}
```

**Layout Adaptations:**
- **Column reverse**: Changes visual order for better mobile flow
- **Standard column**: Simple vertical stack for mobile readability
- **Spacing adjustments**: Maintains visual hierarchy on smaller screens

### Small Mobile Optimizations
```css
@media(max-width: 640px) {
    .container {
        padding: 80px 0;
    }
    .nav__logo {
        margin: 8px 0;
        padding-bottom: 4px;
    }
    main {
        margin-bottom: 80px;
    }
}
```

**Fine-Tuning:**
- **Reduced padding**: 80px instead of 96px conserves mobile screen space
- **Logo adjustments**: Maintains proportion on smallest screens
- **Consistent spacing**: Proportional reduction maintains design rhythm

---

## CSS Architecture Principles

### Naming Convention Strategy
```css
.nav__logo          /* Component with element */
.btn__menu          /* Component with element */
.btn__menu--close   /* Component with element and modifier */
.modal__links       /* Component with element */
.modal__link        /* Component with element */
.modal__link--primary /* Component with element and modifier */
```

**BEM-Inspired Methodology:**
- **Block**: `.nav`, `.btn`, `.modal` represent components
- **Element**: `__logo`, `__menu`, `__links` are parts of components
- **Modifier**: `--close`, `--primary` are variations of elements
- **Clarity**: Names clearly indicate structure and purpose

### CSS Organization Structure
1. **Global imports and resets**
2. **Layout utilities** (`.row`, `.container`)
3. **Component styles** (navigation, modal)
4. **Section-specific styles** (organized by page flow)
5. **Responsive modifications** (media queries last)

### Performance Considerations
```css
* {
    box-sizing: border-box;
}

.btn__menu {
    transition: all 300ms ease;
}
```

**Optimization Techniques:**
- **Box-sizing**: Prevents layout recalculation errors
- **Efficient transitions**: `all` property with short duration prevents jank
- **Minimal specificity**: Flat hierarchy prevents selector performance issues

---

## Flexbox Implementation Patterns

### Primary Layout Method
```css
/* Container setup */
display: flex;
justify-content: space-between;
align-items: center;

/* Direction control */
flex-direction: column;
flex-direction: column-reverse;

/* Alignment variations */
align-items: flex-end;
align-items: center;
```

**Flexbox Strategy:**
- **Primary layout tool**: Chosen over CSS Grid for broader browser support
- **Alignment control**: Precise control over content positioning
- **Responsive behavior**: Natural adaptation to different screen sizes
- **Direction changes**: Easy mobile adaptations with direction reversal

### Common Flex Patterns Used
1. **Space-between**: Navigation logo/links, footer elements
2. **Center alignment**: Modal content, section headers
3. **Column direction**: Mobile layouts, card content
4. **Column-reverse**: Mobile image/text reordering

---

## Color and Visual Design System

### Color Palette Usage
```css
#243e63  /* Primary text color - dark blue-gray */
#6415ff  /* Brand purple - buttons, accents */
#1a202c  /* Darker text for emphasis */
#718096  /* Secondary text color */
#7c8ba1  /* Muted text color */
```

**Color Strategy:**
- **Monochromatic base**: Various shades of blue-gray for text hierarchy
- **Single accent**: Purple brand color for interactive elements
- **Accessibility**: Sufficient contrast ratios for readability
- **Consistency**: Limited palette ensures cohesive design

### Typography Scale
```css
/* Implied from usage */
font-size: 12px;   /* Small labels */
font-size: 16px;   /* Body text */
font-size: 18px;   /* Large body, nav links */
font-size: 20px;   /* Subheadings */
font-size: 24px;   /* Card titles */
font-size: 32px;   /* Section titles */
font-size: 48px;   /* Major headings */
```

**Scale Rationale:**
- **16px base**: Standard body text for good readability
- **Consistent increments**: Logical progression through sizes
- **Mobile consideration**: Minimum 16px prevents iOS zoom

---

## Transition and Animation System

### Transition Strategy
```css
transition: all 300ms ease;
```

**Animation Philosophy:**
- **Consistent duration**: 300ms provides responsive feel without sluggishness
- **Ease timing**: Natural acceleration/deceleration mimics physical movement
- **All properties**: Covers any potential property changes for flexibility

### Transform Usage
```css
transform: translateX(300%);  /* Hide modal off-screen */
transform: translateX(0);     /* Show modal in view */
```

**Performance Benefits:**
- **Transform over position**: Triggers GPU acceleration, smoother animation
- **Percentage values**: Relative to element size, works at any modal width
- **Composite layer**: Transform creates new stacking context for optimization

---

## Cross-Browser Compatibility

### Modern CSS Features Used
```css
display: flex;           /* IE10+ support */
border-radius: 8px;      /* IE9+ support */
rgba() colors;           /* IE9+ support */
CSS transitions;         /* IE10+ support */
calc() function;         /* IE9+ support */
```

**Compatibility Strategy:**
- **Flexbox**: Excellent modern browser support, graceful degradation
- **No CSS Grid**: Avoided for broader compatibility
- **Progressive enhancement**: Core layout works without advanced features
- **Vendor prefixes**: Omitted as target browsers support unprefixed versions

### Fallback Patterns
```css
font-family: 'Inter', system-ui;  /* Web font with system fallback */
background: transparent;           /* Safe default background */
```

**Graceful Degradation:**
- **Font stacking**: System fonts when web fonts fail
- **Color fallbacks**: Safe defaults for older browsers
- **Layout fallbacks**: Block-level behavior when flexbox unavailable

---

## Performance Optimization Techniques

### CSS Delivery
```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400;500;600;700;800;900&display=swap');
```

**Loading Optimization:**
- **Font display swap**: Shows fallback font until web font loads
- **Variable font range**: Single request for all weights
- **CDN delivery**: Google Fonts CDN for caching and performance

### Selector Efficiency
```css
.nav__logo { }           /* Class selector - efficient */
header .row { }          /* Type + class - moderate */
#features .row__narrow { } /* ID + class - efficient */
```

**Selector Performance:**
- **Class-based**: Primary selector type for performance
- **Shallow nesting**: Minimal selector depth prevents slow matching
- **Avoid universal**: `*` selector used sparingly for resets only

### Animation Performance
```css
transform: translateX(300%);  /* GPU-accelerated property */
opacity: 0;                   /* GPU-accelerated property */
visibility: hidden;           /* Layout-friendly hiding */
```

**Hardware Acceleration:**
- **Transform/opacity**: Properties that trigger GPU acceleration
- **Avoid layout triggers**: No width/height/margin animations
- **Composite layers**: Isolated animation layers prevent repaints

---

## Maintenance and Scalability

### Component Modularity
```css
/* Navigation component */
.nav__row { }
.nav__logo { }

/* Button component */
.btn__menu { }
.btn__menu--close { }

/* Modal component */
.modal { }
.modal__links { }
.modal__link { }
```

**Scalable Architecture:**
- **Component isolation**: Styles contained within component namespace
- **Modifier pattern**: Easy variations without class duplication
- **Predictable naming**: Clear component/element/modifier hierarchy

### Future Enhancement Points
1. **CSS Custom Properties**: Could replace hardcoded colors/spacing
2. **Container Queries**: When supported, better component responsiveness
3. **CSS Grid**: Future adoption for complex layouts
4. **Animation API**: JavaScript-driven animations for complex interactions

This CSS architecture provides a solid foundation for maintainable, performant, and scalable web design while supporting the complete Treact landing page experience.