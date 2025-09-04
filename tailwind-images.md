# Tailwind CSS - Image Handling Best Practices

## The PB-Bottom Method

Our standard approach for handling images uses the **padding-bottom method** combined with **absolute positioned backgrounds** for consistent aspect ratios and responsive behavior.

## Core Pattern

```html
<div class="relative pb-[X%] overflow-hidden">
    <img class="absolute inset-0 w-full h-full object-cover" 
         src="image.jpg" 
         alt="Description">
</div>
```

## Key Components

### 1. Container Setup
- `relative` - Creates positioning context
- `pb-[X%]` - Sets aspect ratio using padding-bottom percentage
- `overflow-hidden` - Clips content and enables rounded corners

### 2. Image Positioning
- `absolute` - Removes from document flow
- `inset-0` - Positions to all edges (top-0 right-0 bottom-0 left-0)
- `w-full h-full` - Fills container completely
- `object-cover` - Maintains aspect ratio, crops if needed

## Common Aspect Ratios

Common aspect ratios used in projects:

```css
/* Landscape Ratios */
pb-[60%]     /* 5:3 ratio - Featured images */
pb-[74.07%]  /* 4:3 ratio - Portfolio items */
pb-[76%]     /* 4:3 ratio - Standard landscape */

/* Square */
pb-[100%]    /* 1:1 ratio - Square images */

/* Portrait Ratios */
pb-[125%]    /* 4:5 ratio - Team member photos */
pb-[140%]    /* 5:7 ratio - Medium portrait */
pb-[150%]    /* 2:3 ratio - Tall portrait */
```

## Real-World Examples

### Portfolio Item (4:3 ratio)
```html
<div class="relative pb-[74.07%] overflow-hidden rounded-lg">
    <img src="portfolio-image.jpg" 
         alt="Project name"
         class="absolute inset-0 w-full h-full object-cover transition-transform duration-500 ease-in-out group-hover:scale-105">
</div>
```

### With Fallback Image (when no image provided)
```html
<div class="relative pb-[74.07%] overflow-hidden rounded-lg">
    <img src="https://dummyimage.com/400x300/BABFBC/BABFBC" 
         alt="Placeholder image"
         class="absolute inset-0 w-full h-full object-cover">
</div>
```

### Team Profile (Portrait)
```html
<div class="relative pb-[125%] mb-4 overflow-hidden">
    <img src="team-member.jpg" 
         alt="Team member name"
         class="absolute inset-0 w-full h-full object-cover">
</div>
```

### Video Container (Square)
```html
<div class="relative pb-[100%] overflow-hidden rounded-lg">
    <video autoplay muted loop playsinline 
           class="absolute inset-0 w-full h-full object-cover">
        <source src="video.mp4" type="video/mp4">
    </video>
</div>
```

## Advanced Patterns

### With Overlay Content
```html
<div class="relative pb-[60%] overflow-hidden">
    <!-- Background Image -->
    <img class="absolute inset-0 w-full h-full object-cover" 
         src="background.jpg" alt="Background">
    
    <!-- Overlay -->
    <div class="absolute inset-0 bg-black/20 flex flex-col justify-end">
        <div class="p-6 text-white">
            <h3>Overlay Content</h3>
            <p>Additional text content</p>
        </div>
    </div>
</div>
```

### Responsive Aspect Ratios
```html
<div class="relative lg:pb-[60%] pb-[110%] overflow-hidden">
    <!-- Desktop: Landscape (60%) -->
    <!-- Mobile: Portrait (110%) -->
    <img class="absolute inset-0 w-full h-full object-cover" 
         src="responsive-image.jpg" alt="Responsive image">
</div>
```

## Fallback Images

When no image is provided, use dummyimage.com with appropriate dimensions and colors:

```html
<!-- Standard fallback pattern -->
<img src="https://dummyimage.com/[width]x[height]/[background]/[text]" 
     alt="Placeholder image">
     
<!-- Example: 400x300 with neutral colors -->
<img src="https://dummyimage.com/400x300/BABFBC/BABFBC" 
     alt="Placeholder image">
```

### Common Fallback Dimensions
- **4:3 Landscape**: `400x300`, `800x600`
- **Square**: `400x400`, `600x600` 
- **Portrait**: `300x400`, `400x500`
- **Wide**: `800x400`, `600x300`

Use neutral colors like `#BABFBC` (background) with matching text color for clean placeholders.

## Benefits of This Method

1. **Consistent Aspect Ratios** - Images maintain proportions across different screen sizes
2. **No Layout Shift** - Container reserves space before image loads
3. **Flexible Content** - Works with images, videos, and other media
4. **Responsive Design** - Easy to adjust ratios for different breakpoints
5. **Performance** - No JavaScript required for aspect ratio maintenance
6. **Graceful Fallbacks** - Placeholder images maintain layout when content is missing

## Do's and Don'ts

### ✅ Do
- Always use `relative` on the container
- Use `overflow-hidden` for rounded corners or clipping
- Use semantic `object-cover` for consistent cropping
- Test aspect ratios match your design requirements
- Use dummyimage.com fallbacks when no image is provided

### ❌ Don't
- Don't use arbitrary aspect-ratio values without purpose
- Don't forget `overflow-hidden` when using `rounded-*` classes
- Don't skip the `absolute inset-0` positioning
- Don't use `object-fill` (will distort images)
- Don't forget alt text for accessibility

## Browser Support

This method has excellent browser support and works consistently across all modern browsers without requiring CSS aspect-ratio property support.