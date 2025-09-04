# Tailwind CSS - Container & Layout Best Practices

## The Container System

Our standard approach uses two main container classes for consistent page structure and content alignment across all projects.

## Core Classes

### `.full`
- Full-width container for sections
- `width: 100%` with `position: relative`
- Used as the outer wrapper for page sections

### `.content`  
- Content container with responsive padding
- Auto margins for center alignment
- Responsive horizontal padding that adjusts by screen size

## Static HTML Implementation

### Basic Page Structure
```html
<section class="full">
    <div class="content">
        <h2>Section Heading</h2>
        <p>Section content goes here...</p>
    </div>
</section>
```

### Full-Width Background with Contained Content
```html
<section class="full bg-gray-100 py-16">
    <div class="content">
        <div class="max-w-4xl mx-auto text-center">
            <h2>Centered Content</h2>
            <p>This content is contained while the background extends full-width.</p>
        </div>
    </div>
</section>
```

### Multi-Section Layout
```html
<main>
    <section class="full bg-white">
        <div class="content py-12">
            <h1>Hero Section</h1>
        </div>
    </section>
    
    <section class="full bg-gray-50">
        <div class="content py-16">
            <h2>About Section</h2>
        </div>
    </section>
    
    <section class="full bg-blue-600">
        <div class="content py-20 text-white">
            <h2>Call to Action</h2>
        </div>
    </section>
</main>
```

### Grid Layout within Container
```html
<section class="full">
    <div class="content py-16">
        <div class="grid lg:grid-cols-3 gap-8">
            <div class="col-span-1">
                <h3>Feature One</h3>
                <p>Feature description</p>
            </div>
            <div class="col-span-1">
                <h3>Feature Two</h3>
                <p>Feature description</p>
            </div>
            <div class="col-span-1">
                <h3>Feature Three</h3>
                <p>Feature description</p>
            </div>
        </div>
    </div>
</section>
```

## WordPress Implementation

### Basic WordPress Section
```php
<section class="full">
    <div class="content">
        <h2><?php echo esc_html(get_field('section_heading')); ?></h2>
        <div><?php echo wp_kses_post(get_field('section_content')); ?></div>
    </div>
</section>
```

### WordPress Page Template
```php
<?php get_header(); ?>

<main>
    <?php while (have_posts()): the_post(); ?>
        <section class="full">
            <div class="content py-16">
                <h1><?php the_title(); ?></h1>
                <div class="prose max-w-none">
                    <?php the_content(); ?>
                </div>
            </div>
        </section>
    <?php endwhile; ?>
    
    <?php 
    // Load page modules if using ACF flexible content
    if (have_rows('page_modules')):
        while (have_rows('page_modules')): the_row();
            include get_template_directory() . '/inc/modules/page_modules/' . get_row_layout() . '.php';
        endwhile;
    endif;
    ?>
</main>

<?php get_footer(); ?>
```

### ACF Module Structure
```php
<?php
/**
 * Module: Text Block
 * ACF Layout: text_block
 */

$heading = get_sub_field('heading');
$content = get_sub_field('content');
$background = get_sub_field('background_color');
?>

<section class="module-text_block full <?php echo $background ? 'bg-gray-50' : 'bg-white'; ?>">
    <div class="content py-16">
        <?php if ($heading): ?>
            <h2><?php echo esc_html($heading); ?></h2>
        <?php endif; ?>
        
        <?php if ($content): ?>
            <div class="prose max-w-none">
                <?php echo wp_kses_post($content); ?>
            </div>
        <?php endif; ?>
    </div>
</section>
```

### Archive/Loop Implementation
```php
<section class="full">
    <div class="content py-16">
        <h1><?php echo esc_html(get_the_archive_title()); ?></h1>
        
        <div class="grid lg:grid-cols-3 gap-8 mt-12">
            <?php while (have_posts()): the_post(); ?>
                <article class="col-span-1">
                    <h3><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h3>
                    <div class="text-sm text-gray-600 mb-4">
                        <?php echo get_the_date(); ?>
                    </div>
                    <div><?php the_excerpt(); ?></div>
                </article>
            <?php endwhile; ?>
        </div>
    </div>
</section>
```

## CSS Implementation

Add these classes to your Tailwind CSS input file:

```css
@layer components {
    .full {
        @apply 
        w-full 
        relative
        ;
    }

    .content {
        @apply 
        relative 
        mx-auto 
        w-full
        md:px-[2%]
        px-[4%]
        ;
    }
}
```

## Benefits

1. **Consistent Structure** - Every page section follows the same pattern
2. **Responsive Design** - Automatic padding adjustments across screen sizes  
3. **Full-Width Flexibility** - Easy to add full-width backgrounds while keeping content contained
4. **Maintainable** - Changes to container behavior happen in one place
5. **Scalable** - Works for simple pages and complex layouts

## Do's and Don'ts

### ✅ Do
- Always wrap sections with `.full` for consistent structure
- Use `.content` for inner content containment
- Combine with utility classes for spacing (`py-16`, etc.)
- Keep the nesting pattern: `full > content > actual content`

### ❌ Don't
- Don't skip `.full` wrapper - it provides important structural consistency
- Don't add padding directly to `.full` - use `.content` or inner elements
- Don't nest `.content` inside `.content`
- Don't override the container widths without good reason

## Browser Support

This container system works across all modern browsers and provides a consistent foundation for responsive layouts.