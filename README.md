# Useful WordPress Functions

* [Hide WordPress Update Nag to All But Admins](#Hide-WordPress-Update-Nag-to-All-But-Admins) 
* [Create Custom WordPress Dashboard Widget](#Create-Custom-WordPress-Dashboard-Widget) 


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
	echo "
		<h2>Custom Dashboard Widget</h2>
		<p>Custom content here</p>
	";
} 

function add_dashboard_widgets() {
	wp_add_dashboard_widget('custom_dashboard_widget', 'Custom Dashoard Widget', 'dashboard_widget_function');
}
add_action('wp_dashboard_setup', 'add_dashboard_widgets' );
```
