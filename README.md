# Useful WordPress Functions

This is a list of useful WordPress functions that I often reference to enhance or clean up my sites. Please be careful and make backups.

* [Hide WordPress Update Nag to All But Admins](#hide-wordpress-update-nag-to-all-but-admins)
* [Create Proper WordPress Titles](#create-proper-wordpress-titles)
* [Create Custom WordPress Dashboard Widget](#create-custom-wordpress-dashboard-widget)
* [Remove All Dashboard Widgets](#remove-all-dashboard-widgets)
* [Include Navigation Menus](#include-navigation-menus)
* [Insert Custom Login Logo](#insert-custom-login-logo)
* [Modify Admin Footer Text](#modify-admin-footer-text)
* [Enqueue Styles and Scripts](#enqueue-styles-and-scripts)
* [Enqueue Google Fonts](#enqueue-google-fonts)
* [Modify Excerpt Length](#modify-excerpt-length)
* [Change Read More Link](#change-read-more-link)
* [Change More Excerpt](#change-more-excerpt)
* [Disable Emoji Mess](#disable-emoji-mess)
* [Remove Comments](#remove-comments)
* [Change Media Gallery URL](#change-media-gallery-url)
* [Create Custom Thumbnail Size](#create-custom-thumbnail-size)
* [Add Categories for Attachments](#add-categories-for-attachments)
* [Add Tags for Attachments](#add-tags-for-attachments)
* [Add Custom Excerpt to Pages](#add-custom-excerpt-to-pages)
* [Create a Global String](#create-a-global-string)
* [Support Featured Images](#support-featured-images)
* [Support Search Form](#support-search-form)
* [Excluding pages from search](#excluding-pages-from-search)
* [Disable XMLRPC](#disable-xmlrpcphp)
* [Escape HTML in Posts](#escape-html-in-posts)
* [Create Custom Global Settings](#create-custom-global-settings)
* [Remove WordPress Admin Bar](#remove-wordpress-admin-bar)
* [Add Open Graph Meta Tags](#add-open-graph-meta-tags)
* [Add Custom Post Type](#add-custom-post-type)
* [Implement Preconnect to Google Fonts in Themes](#implement-preconnect-to-google-fonts-in-themes)
* [Add Thumbnail Column to Post Listing](#add-thumbnail-column-to-post-listing)
* [Add Lead Class to First Paragraph](#add-lead-class-to-first-paragraph)
* [Exclude Custom Post Type from Search](#exclude-custom-post-type-from-search)
* [Remove Query String from Static Resources](#remove-query-string-from-static-resources)
* [Disable Website Field From Comment Form](#disable-website-field-from-comment-form)
* [Modify jQuery](#modify-jquery)
* [Disable JSON Rest API](#disable-json-rest-api)

## Hide WordPress Update Nag to All But Admins

```php
/**
 * Hide WordPress Update Nag to All But Admins
 */
 
function hide_update_notice_to_all_but_admin() {
	if ( !current_user_can( 'update_core' ) ) {
		remove_action( 'admin_notices', 'update_nag', 3 );
	}
}
add_action( 'admin_head', 'hide_update_notice_to_all_but_admin', 1 );
```

## Create Proper WordPress Titles

*Update*: As of WP 4.1, the long version is no longer required - simply add the following to functions.php and remove the `<title>` tag from your header.

```php
/**
 * Create Proper WordPress Titles
 */

add_theme_support( 'title-tag' );
```

## Create Custom WordPress Dashboard Widget

```php
/**
 * Create Custom WordPress Dashboard Widget
 */
 
function dashboard_widget_function() {
	echo '
		<h2>Custom Dashboard Widget</h2>
		<p>Custom content here</p>
	';
}

function add_dashboard_widgets() {
	wp_add_dashboard_widget( 'custom_dashboard_widget', 'Custom Dashoard Widget', 'dashboard_widget_function' );
}
add_action( 'wp_dashboard_setup', 'add_dashboard_widgets' );
```

## Remove All Dashboard Widgets

```php
/**
 * Remove All Dashboard Widgets
 */
 
function remove_dashboard_widgets() {
	global $wp_meta_boxes;
	unset( $wp_meta_boxes['dashboard']['side']['core']['dashboard_quick_press'] );
	unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_incoming_links'] );
	unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_right_now'] );
	unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_plugins'] );
	unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_recent_drafts'] );
	unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_recent_comments'] );
	unset( $wp_meta_boxes['dashboard']['side']['core']['dashboard_primary'] );
	unset( $wp_meta_boxes['dashboard']['side']['core']['dashboard_secondary'] );
	remove_meta_box( 'dashboard_activity', 'dashboard', 'normal' );
}
add_action( 'wp_dashboard_setup', 'remove_dashboard_widgets' );
```

## Include Navigation Menus

```php
/** 
 * Include navigation menu
 */

function register_my_menu() {
  register_nav_menu('nav-menu',__( 'Navigation Menu' ));
}
add_action( 'init', 'register_my_menu' );
```

Insert this where you want it to appear, and save the menu in **Appearance -> Menus**.

```php
<?php wp_nav_menu( array( 'theme_location' => 'nav-menu' ) ); ?>
```

Here's the code for multiple menus/

```php
function register_my_menus() {
	register_nav_menus(
		array(
			'new-menu' => __( 'New Menu' ),
			'another-menu' => __( 'Another Menu' ),
			'an-extra-menu' => __( 'An Extra Menu' )
		)
	);
}
add_action( 'init', 'register_my_menus' );
```

## Insert Custom Login Logo

```php
/**
 * Insert Custom Login Logo
 */
 
function custom_login_logo() {
	echo '
		<style>
			.login h1 a { background-image: url(image.jpg) !important; background-size: 234px 67px; width:234px; height:67px; display:block; }
		</style>
	';
}
add_action( 'login_head', 'custom_login_logo' );
```

## Modify Admin Footer Text

```php
/**
 * Modify Admin Footer Text
 */
 
function modify_footer() {
	echo 'Created by <a href="mailto:you@example.com">you</a>.';
}
add_filter( 'admin_footer_text', 'modify_footer' );
```

## Enqueue Styles and Scripts

```php
/**
 * Enqueue Styles and Scripts
 */
 
function custom_scripts() {
	wp_enqueue_style( 'bootstrap', get_template_directory_uri() . '/css/bootstrap.min.css', array(), '3.3.6' );
	wp_enqueue_style( 'style', get_template_directory_uri() . '/css/style.css' );
	wp_enqueue_script( 'bootstrap', get_template_directory_uri() . '/js/bootstrap.min.js', array('jquery'), '3.3.6', true );
	wp_enqueue_script( 'script', get_template_directory_uri() . '/js/script.js' );
}

add_action( 'wp_enqueue_scripts', 'custom_scripts' );
```

## Enqueue Google Fonts

```php
// Enqueue Google Fonts
add_action( 'after_setup_theme', 'mytheme_theme_setup' );
if ( ! function_exists( 'mytheme_theme_setup' ) ) {
	/**
	 * Sets up theme defaults and registers support for various WordPress features.
	 *
	 * Note that this function is hooked into the after_setup_theme hook, which
	 * runs before the init hook. The init hook is too late for some features, such
	 * as indicating support for post thumbnails.
	 *
	 * @since  1.0.0
	 */
	function mytheme_theme_setup() {
		add_action( 'wp_enqueue_scripts', 'mytheme_frontend_scripts' );
	}
}


if ( ! function_exists( 'mytheme_frontend_scripts' ) ) {
	/**
	 * Enqueue scripts and styles.
	 *
	 * @since 1.0.0
	 */
	function mytheme_frontend_scripts() {

		wp_enqueue_style( 'google_fonts', google_fonts_url(), array(), null );

	}
}

if ( ! function_exists( 'google_fonts_url' ) ) {
	/**
	 * Register Google fonts
	 *
	 * @since 1.0.0
	 * @return string Google fonts URL for the theme.
	 */
	function google_fonts_url() {
		$fonts_url = '';
		$ubuntu   = esc_html_x( 'on', 'Ubuntu font: on or off', 'mytheme' );
		$vollkorn = esc_html_x( 'on', 'Vollkorn font: on or off', 'mytheme' );
		$roboto   = esc_html_x( 'on', 'Roboto font: on or off', 'mytheme' );

		/*
		 * Translators: If there are characters in your language that are not supported
		 * by Ubuntu, Vollkorn or Roboto, translate this to 'off'. Do not translate into your own language.
		 */
		if ( 'off' !== $ubuntu || 'off' !== $vollkorn || 'off' !== $roboto ) {
			$font_families = array();
			if ( 'off' !== $ubuntu ) {
				$font_families[] = 'Ubuntu:300';
			}
			if ( 'off' !== $vollkorn ) {
				$font_families[] = 'Vollkorn:400italic';
			}
			if ( 'off' !== $roboto ) {
				$font_families[] = 'Roboto Condensed:700';
			}
			$query_args = array(
				'family' => rawurlencode( implode( '|', $font_families ) ),
			);
			$fonts_url = add_query_arg( $query_args, 'https://fonts.googleapis.com/css' );
		}
		return esc_url_raw( $fonts_url );
	}
}
```

## Modify Excerpt Length

```php
/**
 * Modify Excerpt Length
 */
 
function custom_excerpt_length( $length ) {
	return 25;
}
add_filter( 'excerpt_length', 'custom_excerpt_length', 999 );
```

## Change Read More Link

```php
/**
 * Change Read More Link
 */
 
function custom_read_more_link() {
	return '<a href="' . get_permalink() . '">Read More</a>';
}
add_filter( 'the_content_more_link', 'custom_read_more_link' );
```

## Change More Excerpt

```php
/**
 * Change More Excerpt
 */
 
function custom_more_excerpt( $more ) {
	return '...';
}
add_filter( 'excerpt_more', 'custom_more_excerpt' );
```

## Disable Emoji Mess

```php
/**
 * Disable Emoji Mess
 */
 
function disable_wp_emojicons() {
	remove_action( 'admin_print_styles', 'print_emoji_styles' );
	remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
	remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
	remove_action( 'wp_print_styles', 'print_emoji_styles' );
	remove_filter( 'wp_mail', 'wp_staticize_emoji_for_email' );
	remove_filter( 'the_content_feed', 'wp_staticize_emoji' );
	remove_filter( 'comment_text_rss', 'wp_staticize_emoji' );
	add_filter( 'tiny_mce_plugins', 'disable_emojicons_tinymce' );
	add_filter( 'emoji_svg_url', '__return_false' );
}
add_action( 'init', 'disable_wp_emojicons' );
function disable_emojicons_tinymce( $plugins ) {
	return is_array( $plugins ) ? array_diff( $plugins, array( 'wpemoji' ) ) : array();
}
```

## Remove Comments

```php
/**
 * Remove Comments
 */
 
// Removes from admin menu
add_action( 'admin_menu', 'my_remove_admin_menus' );
function my_remove_admin_menus() {
	remove_menu_page( 'edit-comments.php' );
}
// Removes from post and pages
add_action( 'init', 'remove_comment_support', 100 );
function remove_comment_support() {
	remove_post_type_support( 'post', 'comments' );
	remove_post_type_support( 'page', 'comments' );
}
// Removes from admin bar
function mytheme_admin_bar_render() {
	global $wp_admin_bar;
	$wp_admin_bar->remove_menu( 'comments' );
}
add_action( 'wp_before_admin_bar_render', 'mytheme_admin_bar_render' );
```

## Change Media Gallery URL

```php
/**
 * Change Media Gallery URL
 */
 
update_option( 'upload_url_path', 'http://assets.website.com/wp-content/uploads' );
```

## Create Custom Thumbnail Size

```php
/**
 * Create Custom Thumbnail Size
 */
 
add_image_size( 'custom-thumbnail', 250, 250, true );
```

**Retrieve Thumbnail**

 ```php
 <?php $thumb = wp_get_attachment_image_src( get_post_thumbnail_id($post->ID), 'custom-thumbnail' );

 echo $thumb[0]; ?>
 ```
 
 Since WordPress 4.4.0 you can use:
 
 ```php
 the_post_thumbnail_url( $size );
 ```

## Add Categories for Attachments

```php
/**
 * Add Categories for Attachments
 */
 
function add_categories_for_attachments() {
	register_taxonomy_for_object_type( 'category', 'attachment' );
}
add_action( 'init' , 'add_categories_for_attachments' );
```

## Add Tags for Attachments

```php
/**
 * Add Tags for Attachments
 */
 
function add_tags_for_attachments() {
	register_taxonomy_for_object_type( 'post_tag', 'attachment' );
}
add_action( 'init' , 'add_tags_for_attachments' );
```

## Add Custom Excerpt to Pages

```php
/**
 * Add Custom Excerpt to Pages
 */
 
function add_page_excerpt() {
	add_post_type_support( 'page', array( 'excerpt' ) );
}
add_action( 'init', 'add_page_excerpt' );
```

## Create a Global String

```php
/**
 * Create a Global String
 */
 
function global_string() {
	return 'String';
}
```

**Retrieve Field**

```php
<?php echo global_string(); ?>
```

## Support Featured Images

```php
/**
 * Support Featured Images
 */
 
add_theme_support( 'post-thumbnails' );
```

## Support Search Form

```php
/**
 * Support Search Form
 */
 
add_theme_support( 'html5', array( 'search-form' ) );
```

## Excluding pages from search

```php
/**
 * Excluding pages from search
 */
 
function exclude_pages_from_search() {
	global $wp_post_types;
	$wp_post_types['page']->exclude_from_search = true;
}
add_action( 'init', 'exclude_pages_from_search' );
```

## Disable xmlrpc.php

```php
/**
 * Disable xmlrpc.php
 */
 
add_filter( 'xmlrpc_enabled', '__return_false' );
remove_action( 'wp_head', 'rsd_link' );
remove_action( 'wp_head', 'wlwmanifest_link' );
```

## Escape HTML in Posts

```php
/**
 * Escape HTML in <code> or <pre><code> tags.
 */
 
function escapeHTML($arr) {
	if (version_compare(PHP_VERSION, '5.2.3') >= 0) {
		$output = htmlspecialchars($arr[2], ENT_NOQUOTES, get_bloginfo('charset'), false);
	}
	else {
		$specialChars = array(
			'&' => '&amp;',
			'<' => '&lt;',
			'>' => '&gt;'
		);

		// decode already converted data
		$data = htmlspecialchars_decode( $arr[2] );
		// escapse all data inside <pre>
		$output = strtr( $data, $specialChars );
	}
	if (! empty($output)) {
		return  $arr[1] . $output . $arr[3];
	}	else 	{
		return  $arr[1] . $arr[2] . $arr[3];
	}
}
function filterCode($data) { // Uncomment if you want to escape anything within a <pre> tag
	//$modifiedData = preg_replace_callback( '@(<pre.*>)(.*)(<\/pre>)@isU', 'escapeHTML', $data );
	$modifiedData = preg_replace_callback( '@(<code.*>)(.*)(<\/code>)@isU', 'escapeHTML', $data );
	$modifiedData = preg_replace_callback( '@(<tt.*>)(.*)(<\/tt>)@isU', 'escapeHTML', $modifiedData );

	return $modifiedData;
}
add_filter( 'content_save_pre', 'filterCode', 9 );
add_filter( 'excerpt_save_pre', 'filterCode', 9 );
```

Modified from [Escape HTML](https://wordpress.org/plugins/escape-html/).

## Create Custom Global Settings

```php
/**
 * Create Custom Global Settings
 */
 
function custom_settings_page() { ?>
	<div class="wrap">
	<h1>Custom Settings</h1>
	<form method="post" action="options.php">
		<?php
			settings_fields( 'section' );
			do_settings_sections( 'theme-options' );
			submit_button();
		?>
	</form>
	</div>
<?php }

function custom_settings_add_menu() {
	add_theme_page( 'Custom Settings', 'Custom Settings', 'manage_options', 'custom-settings', 'custom_settings_page', null, 99 );
}
add_action( 'admin_menu', 'custom_settings_add_menu' );

// Example setting
function setting_twitter() { ?>
	<input type="text" name="twitter" id="twitter" value="<?php echo get_option('twitter'); ?>" />
<?php }

function custom_settings_page_setup() {
	add_settings_section( 'section', 'All Settings', null, 'theme-options' );
	add_settings_field( 'twitter', 'Twitter Username', 'setting_twitter', 'theme-options', 'section' );
	register_setting( 'section', 'twitter' );
}
add_action( 'admin_init', 'custom_settings_page_setup' );
```

Retrieve Field

```php
<?php echo get_option( 'twitter' ); ?>
```

Modified from [Create a WordPress Theme Settings Page with the Settings API](http://www.sitepoint.com/create-a-wordpress-theme-settings-page-with-the-settings-api/).

## Remove WordPress Admin Bar

```php
/**
 * Remove WordPress Admin Bar
 */

function remove_admin_bar() {
	remove_action( 'wp_head', '_admin_bar_bump_cb' );
}
add_action( 'get_header', 'remove_admin_bar' );
```

## Add Open Graph Meta Tags

```php
/**
 * Add Open Graph Meta Tags
 */

function meta_og() {
	global $post;
	if ( is_single() ) {
		if( has_post_thumbnail( $post->ID ) ) {
			$img_src = wp_get_attachment_image_src( get_post_thumbnail_id( $post->ID ), 'thumbnail' );
		} 
		$excerpt = strip_tags($post->post_content);
		$excerpt_more = '';
		if ( strlen($excerpt ) > 155) {
			$excerpt = substr($excerpt,0,155);
			$excerpt_more = ' ...';
		}
		$excerpt = str_replace( '"', '', $excerpt );
		$excerpt = str_replace( "'", '', $excerpt );
		$excerptwords = preg_split( '/[\n\r\t ]+/', $excerpt, -1, PREG_SPLIT_NO_EMPTY );
		array_pop( $excerptwords );
		$excerpt = implode( ' ', $excerptwords ) . $excerpt_more;
		?>
<meta name="author" content="Your Name">
<meta name="description" content="<?php echo $excerpt; ?>">
<meta property="og:title" content="<?php echo the_title(); ?>">
<meta property="og:description" content="<?php echo $excerpt; ?>">
<meta property="og:type" content="article">
<meta property="og:url" content="<?php echo the_permalink(); ?>">
<meta property="og:site_name" content="Your Site Name">
<meta property="og:image" content="<?php echo $img_src[0]; ?>">
<?php
	} else {
			return;
	}
}
add_action('wp_head', 'meta_og', 5);
```

## Add Custom Post Type

```php
/**
 * Add Custom Post Type
 */

function create_custom_post() {
	register_post_type( 'custom-post', // slug for custom post type
		array(
		'labels' => array(
			'name' => __( 'Custom Post' ),
		),
		'public' => true,
		'hierarchical' => true, 
		'has_archive' => true,
		'supports' => array(
			'title',
			'editor',
			'excerpt',
			'thumbnail'
		), 
		'can_export' => true,
		'taxonomies' => array(
				'post_tag',
				'category'
		)
	));
}
add_action('init', 'create_custom_post');
```

## Implement Preconnect to Google Fonts in Themes

```php
/**
 * Implement Preconnect to Google Fonts in Themes
 */

function twentyfifteen_resource_hints( $urls, $relation_type ) {
	// Checks whether the subject is carrying the source of fonts google and the `$relation_type` equals preconnect.
	// Replace `enqueue_font_id` the `ID` used in loading the source.
	if ( wp_style_is( 'enqueue_font_id', 'queue' ) && 'preconnect' === $relation_type ) {
		// Checks whether the version of WordPress is greater than or equal to 4.7
		// to ensure conmpatibilidade with older versions
		// because the 4.7 has become necessary to return an array instead of string
		if ( version_compare( $GLOBALS['wp_version'], '4.7-alpha', '>=' ) ) {
			// Array with url google fonts and crossorigin
			$urls[] = array(
				'href' => 'https://fonts.gstatic.com',
				'crossorigin',
			);
		} else {
			// String with url google fonts
			$urls[] = 'https://fonts.gstatic.com';
		}
	}
	return $urls;
}
add_filter( 'wp_resource_hints', 'twentyfifteen_resource_hints', 10, 2 ); 
```

## Add Thumbnail Column to Post Listing

```php
/**
 * Add Thumbnail Column to Post Listing
 */

add_image_size( 'admin-list-thumb', 80, 80, false );

function wpcs_add_thumbnail_columns( $columns ) {
     
    if ( !is_array( $columns ) )
        $columns = array();
    $new = array();

    foreach( $columns as $key => $title ) {
        if ( $key == 'title' ) // Put the Thumbnail column before the Title column
            $new['featured_thumb'] = __( 'Image');
        $new[$key] = $title;
    }
    return $new;
}

function wpcs_add_thumbnail_columns_data( $column, $post_id ) {
    switch ( $column ) {
    case 'featured_thumb':
        echo '<a href="' . $post_id . '">';
        echo the_post_thumbnail( 'admin-list-thumb' );
        echo '</a>';
        break;
    }
}

if ( function_exists( 'add_theme_support' ) ) {
    add_filter( 'manage_posts_columns' , 'wpcs_add_thumbnail_columns' );
    add_action( 'manage_posts_custom_column' , 'wpcs_add_thumbnail_columns_data', 10, 2 );
}
```
## Add Lead Class to First Paragraph

```php
/**
 * Add Lead Class to First Paragraph
 */

function first_paragraph( $content ) {
	return preg_replace( '/<p([^>]+)?>/', '<p$1 class="lead">', $content, 1 );
}
add_filter( 'the_content', 'first_paragraph' );
```

Adds a `lead` class to the first paragraph in [the_content](https://developer.wordpress.org/reference/functions/the_content/).

## Exclude Custom Post Type from Search

```php
/**
 * Exclude Custom Post Type from Search
 */

function excludePages( $query ) {
if ( $query->is_search ) {
	$query->set( 'post_type', 'post' );
}
	return $query;
}
add_filter( 'pre_get_posts','excludePages' );
```

## Remove Query String from Static Resources

```php
/**
 * Remove Query String from Static Resources 
 */
 
function remove_cssjs_ver( $src ) {
 if( strpos( $src, '?ver=' ) )
 $src = remove_query_arg( 'ver', $src );
 return $src;
}
add_filter( 'style_loader_src', 'remove_cssjs_ver', 10, 2 );
add_filter( 'script_loader_src', 'remove_cssjs_ver', 10, 2 );
```

## Modify jQuery

```php
/**
 * modify jquery
 */
function modify_jquery() {
    if ( !is_admin() && !is_login_page() ) {
        // comment out the next two lines to load the local copy of jQuery
        wp_deregister_script('jquery');
        // wp_register_script('jquery', 'https://ajax.googleapis.com/ajax/libs/jquery/2.2.4/jquery.js', false, '2.2.4');
        wp_register_script('jquery', 'https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js', false, '3.2.1');
        wp_enqueue_script('jquery');
    }
}
add_action( 'init', 'modify_jquery' );
```

##  Disable Website Field From Comment Form

```php
/** 
 * Disable Website Field From Comment Form
 */

function disable_website_field( $field ) { 
	if( isset($field['url']) ) {
		unset( $field['url'] );
	}
	return $field;
}

add_filter('comment_form_default_fields', 'disable_website_field');
```
##  Disable JSON Rest API

```php
/** 
 * Disable JSON Rest API  
 */

add_filter('json_enabled', '__return_false');
add_filter('json_jsonp_enabled', '__return_false');
```
