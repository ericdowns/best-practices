# WordPress - ACF Module Architecture Best Practices

## Unified Module System

This system organizes ALL modules in a single directory but makes them available through multiple ACF Flexible Content field groups. Modules are automatically loaded based on their filename - no manual registration required.

## Directory Structure

```
/inc/modules/page_modules/
├── _module-template.php           # Template for new modules
├── hero_page.php                  # Shared module (available in multiple contexts)
├── text_statement.php             # Shared module
├── portfolio_images.php           # Portfolio-specific module
├── portfolio_project_details.php  # Portfolio-specific module
├── team_profiles.php              # Page module
└── image_carousel.php             # Shared module
```

**Important**: All modules share the same directory - there are NO separate subfolders.

## Module Contexts & Field Groups

The system uses multiple ACF field groups to control module availability:

### 1. Page Modules
- **Field Group**: `group_page_modules`
- **Field**: `page_modules`  
- **Loader**: `/inc/page_modules.php`
- **Used on**: Regular pages (except specific exclusions)

### 2. Portfolio Modules  
- **Field Group**: `group_portfolio_modules`
- **Field**: `portfolio_modules`
- **Loader**: `/inc/portfolio_modules.php`
- **Used on**: Portfolio post type and specific pages

### 3. Custom Context Modules
- **Field Group**: `group_[context]_modules`
- **Field**: `[context]_modules`
- **Loader**: `/inc/[context]_modules.php`
- **Used on**: Context-specific pages/post types

## Module Implementation

### Basic Module Structure
```php
<?php
/**
 * Module: Text Block
 * ACF Layout: text_block
 * Description: Simple text content block
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

### Module with Content/Options Tabs
```php
<?php
/**
 * Module: Image Carousel
 * ACF Layout: image_carousel
 * Description: Image carousel with navigation options
 */

// Content Tab Fields
$heading = get_sub_field('heading');
$images = get_sub_field('images');

// Options Tab Fields
$show_navigation = get_sub_field('show_navigation');
$autoplay = get_sub_field('autoplay');
$layout_style = get_sub_field('layout_style');
?>

<section class="module-image_carousel full py-16">
    <div class="content">
        <?php if ($heading): ?>
            <h2 class="mb-8"><?php echo esc_html($heading); ?></h2>
        <?php endif; ?>
        
        <div class="swiper <?php echo $layout_style ? 'swiper-' . $layout_style : ''; ?>" 
             data-autoplay="<?php echo $autoplay ? 'true' : 'false'; ?>">
            <div class="swiper-wrapper">
                <?php foreach ($images as $image): ?>
                    <div class="swiper-slide">
                        <div class="relative pb-[75%] overflow-hidden rounded-lg">
                            <img src="<?php echo esc_url($image['url']); ?>" 
                                 alt="<?php echo esc_attr($image['alt']); ?>"
                                 class="absolute inset-0 w-full h-full object-cover">
                        </div>
                    </div>
                <?php endforeach; ?>
            </div>
            
            <?php if ($show_navigation): ?>
                <div class="swiper-button-next"></div>
                <div class="swiper-button-prev"></div>
            <?php endif; ?>
        </div>
    </div>
</section>
```

## Module Loading System

### Loader File Pattern (`/inc/page_modules.php`)
```php
<?php
/**
 * Page Modules Loader
 * Automatically includes modules based on ACF layout name
 */

if (have_rows('page_modules')):
    while (have_rows('page_modules')): the_row();
        $layout = get_row_layout();
        $module_file = get_template_directory() . '/inc/modules/page_modules/' . $layout . '.php';
        
        if (file_exists($module_file)) {
            include $module_file;
        } else {
            // Fallback for missing modules
            echo '<!-- Module not found: ' . esc_html($layout) . ' -->';
        }
    endwhile;
endif;
```

### Page Template Integration
```php
<?php get_header(); ?>

<main>
    <?php while (have_posts()): the_post(); ?>
        <!-- Page Header -->
        <section class="full">
            <div class="content py-16">
                <h1><?php the_title(); ?></h1>
                <?php if (get_the_content()): ?>
                    <div class="prose max-w-none">
                        <?php the_content(); ?>
                    </div>
                <?php endif; ?>
            </div>
        </section>
        
        <!-- Load Page Modules -->
        <?php include get_template_directory() . '/inc/page_modules.php'; ?>
    <?php endwhile; ?>
</main>

<?php get_footer(); ?>
```

## Module Naming Conventions

### Shared Modules (No Prefix)
- `text_statement.php`
- `hero_page.php` 
- `image_carousel.php`
- **Usage**: Can be included in multiple field groups

### Context-Specific Modules
- `portfolio_images.php` (Portfolio-specific)
- `team_profiles.php` (Page-specific)
- `shop_product_grid.php` (E-commerce-specific)
- **Usage**: Typically only in specific field groups

## Creating New Modules

### 1. Using Module Template
```bash
# Copy the template
cp /inc/modules/page_modules/_module-template.php /inc/modules/page_modules/new_module.php
```

### 2. Update Module Header
```php
<?php
/**
 * Module: New Module Name
 * ACF Layout: new_module
 * Description: Description of what this module does
 */
```

### 3. Add to ACF Field Group
1. Navigate to ACF Field Groups
2. Edit the relevant field group (Page Modules, Portfolio Modules, etc.)
3. Add new layout to Flexible Content field
4. Set layout name to match filename (without .php)
5. Add fields for the module

### 4. Module Options Pattern
When modules need configuration options, organize with tabs:

```json
{
  "key": "field_new_module_tab_content",
  "label": "Content",
  "type": "tab"
},
// Content fields here...
{
  "key": "field_new_module_tab_options", 
  "label": "Options",
  "type": "tab"
}
// Option fields here...
```

## Benefits of This System

1. **Single Directory**: All modules in one location - easy to find and maintain
2. **Flexible Availability**: Modules can be shared or context-specific  
3. **Auto-Discovery**: No manual registration - filename = layout name
4. **Scalable**: Easy to add new contexts and modules
5. **Organized**: Clear separation between content and configuration options
6. **Consistent**: All modules follow the same structure and naming

## Best Practices

### ✅ Do
- Follow the module template structure
- Use consistent naming conventions
- Organize fields with Content/Options tabs when needed
- Include proper error checking for fields
- Use semantic CSS classes with module prefix
- Always escape output with `esc_html()`, `esc_url()`, etc.

### ❌ Don't  
- Don't create separate subdirectories for different module types
- Don't forget to add modules to ACF field groups
- Don't use random field keys - follow existing patterns
- Don't skip the module header comments
- Don't forget fallback content for missing fields

## File Structure Example

```
wp-content/themes/your-theme/
├── inc/
│   ├── modules/
│   │   └── page_modules/
│   │       ├── _module-template.php
│   │       ├── hero_page.php
│   │       ├── text_block.php
│   │       ├── image_carousel.php
│   │       ├── portfolio_images.php
│   │       └── team_profiles.php
│   ├── page_modules.php           # Loader for page modules
│   ├── portfolio_modules.php      # Loader for portfolio modules
│   └── [context]_modules.php      # Loader for custom contexts
├── page.php                       # Includes page_modules.php
├── single-portfolio.php           # Includes portfolio_modules.php
└── functions.php                  # ACF field group registrations
```