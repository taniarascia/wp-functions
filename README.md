# Useful WordPress Functions

* [Hide WordPress Update Nag to All But Admins]()
* [Create Custom WordPress Dashboard Widget]()
* [Remove All Dashboard Widgets]()
* [Insert Custom Login Logo]()


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
