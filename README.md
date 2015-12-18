# Useful WordPress Functions

* [Hide WordPress Update Nag to All But Admins](#Hide WordPress Update Nag to All But Admins)
* [Create Custom WordPress Dashboard Widget]()
* [Remove All Dashboard Widgets]()
* [Insert Custom Login Logo]()
* [Modify Admin Footer Text]()
* [Enqueue Styles and Scripts]()
* [Enqueue Google Fonts]()
* [Modify Excerpt Length]()
* [Change Read More Link]()
* [Change More Excerpt]()
* [Disable Emoji Mess]()
* [Change Media Gallery URL]()
* [Create Custom Thumbnail Size]()
* [Add Categories for Attachments]()
* [Add Tags for Attachments]()
* [Create a Global Variable]()
* [Support Featured Images]()
* [Support Search Form]()

### Hide WordPress Update Nag to All But Admins

```php
// Hide WordPress Update Nag to All But Admins
function hide_update_notice_to_all_but_admin() {
	if (!current_user_can('update_core')) {
		remove_action( 'admin_notices', 'update_nag', 3 );
	}
}
add_action( 'admin_head', 'hide_update_notice_to_all_but_admin', 1 );
```

### Create Custom WordPress Dashboard Widget

```php
// Create Custom WordPress Dashboard Widget
function dashboard_widget_function() {
	echo '
		<h2>Custom Dashboard Widget</h2>
		<p>Custom content here</p>
	';
} 

function add_dashboard_widgets() {
	wp_add_dashboard_widget('custom_dashboard_widget', 'Custom Dashoard Widget', 'dashboard_widget_function');
}
add_action( 'wp_dashboard_setup', 'add_dashboard_widgets' );
```
 
### Remove All Dashboard Widgets

```php
// Remove All Dashboard Widgets
function remove_dashboard_widgets() {
    global $wp_meta_boxes;
    unset($wp_meta_boxes['dashboard']['side']['core']['dashboard_quick_press']);
    unset($wp_meta_boxes['dashboard']['normal']['core']['dashboard_incoming_links']);
    unset($wp_meta_boxes['dashboard']['normal']['core']['dashboard_right_now']);
    unset($wp_meta_boxes['dashboard']['normal']['core']['dashboard_plugins']);
    unset($wp_meta_boxes['dashboard']['normal']['core']['dashboard_recent_drafts']);
    unset($wp_meta_boxes['dashboard']['normal']['core']['dashboard_recent_comments']);
    unset($wp_meta_boxes['dashboard']['side']['core']['dashboard_primary']);
    unset($wp_meta_boxes['dashboard']['side']['core']['dashboard_secondary']);
  remove_meta_box( 'dashboard_activity', 'dashboard', 'normal' );
}
add_action('wp_dashboard_setup', 'remove_dashboard_widgets');
```
### Insert Custom Login Logo

```php
// Insert Custom Login Logo
function custom_login_logo() {
    echo '
    	<style>
        .login h1 a { background-image:url(IMAGE)!important;background-size: 234px 67px;width:234px;height:67px;display:block; }
    	</style>
    	';
}
add_action( 'login_head', 'custom_login_logo' );
```

### Modify Admin Footer Text

```php
// Modify Admin Footer Text
function modify_footer() {
  echo 'Created by <a href="mailto:you@example.com">you</a>.';
}
```

### Enqueue Styles and Scripts

```php
// Enqueue Styles and Scripts
function custom_scripts() {
	wp_enqueue_style( 'bootstrap', get_template_directory_uri() . '/css/bootstrap.min.css', array(), '3.3.6' );
	wp_enqueue_style( 'style', get_template_directory_uri() . '/css/style.css' );
	wp_enqueue_script( 'bootstrap', get_template_directory_uri() . '/js/bootstrap.min.js', array('jquery'), '3.3.6', true );
	wp_enqueue_script( 'script', get_template_directory_uri() . '/js/script.js' );
}

add_action( 'wp_enqueue_scripts', 'custom_scripts' );
```

### Enqueue Google Fonts

```php
// Enqueue Google Fonts
function google_fonts() {
				wp_register_style('OpenSans', '//fonts.googleapis.com/css?family=Open+Sans:400,600,700,800');
				wp_enqueue_style( 'OpenSans');
		}

add_action( 'wp_print_styles', 'google_fonts' );
```

### Modify Excerpt Length

```php
// Modify Excerpt Length
function custom_excerpt_length( $length ) {
	return 25;
}
add_filter( 'excerpt_length', 'custom_excerpt_length', 999 );
```

### Change Read More Link

```php
// Change Read More Link
function custom_read_more_link() {
	return '<a href="' . get_permalink() . '">Read More</a>';
}
add_filter( 'the_content_more_link', 'custom_read_more_link' );
```

### Change More Excerpt

```php
// Change More Excerpt
function custom_more_excerpt( $more ) {
	return '...';
}
add_filter('excerpt_more', 'custom_more_excerpt');
```

### Disable Emoji Mess

```php
// Disable Emoji Mess
function disable_wp_emojicons() {
  remove_action( 'admin_print_styles', 'print_emoji_styles' );
  remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
  remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
  remove_action( 'wp_print_styles', 'print_emoji_styles' );
  remove_filter( 'wp_mail', 'wp_staticize_emoji_for_email' );
  remove_filter( 'the_content_feed', 'wp_staticize_emoji' );
  remove_filter( 'comment_text_rss', 'wp_staticize_emoji' );
  add_filter( 'tiny_mce_plugins', 'disable_emojicons_tinymce' );
}
add_action( 'init', 'disable_wp_emojicons' );
function disable_emojicons_tinymce( $plugins ) {
  if ( is_array( $plugins ) ) {
    return array_diff( $plugins, array( 'wpemoji' ) );
  } else {
    return array();
  }
}
```
### Change Media Gallery URL

```php
// Change Media Gallery URL
update_option( 'upload_url_path', 'http://s3.website.com/wp-content/uploads' );
```

### Create Custom Thumbnail Size

```php
// Add 250 x 250 Custom Thumbnail Size
add_image_size( 'themename-nav-thumbnail', 250, 250, true );
```

### Add Categories for Attachments

```php
// Add Categories for Attachments
function add_categories_for_attachments() {
	register_taxonomy_for_object_type( 'category', 'attachment' ); 
} 
add_action( 'init' , 'add_categories_for_attachments' ); 
```

### Add Tags for Attachments

```php
// Add Tags for Attachments
function add_tags_for_attachments() {
	register_taxonomy_for_object_type( 'post_tag', 'attachment' ); 
} 
add_action( 'init' , 'add_tags_for_attachments' );
```

### Create a Global Variable

```php
// Create a Global Variable
function global_variable(){
     return 'String';
}
```

### Support Featured Images

```php
// Support Featured Images
add_theme_support( 'post-thumbnails' );
```

### Support Search Form

```php
// Support Search Form
add_theme_support( 'html5', array( 'search-form' ) );
```
