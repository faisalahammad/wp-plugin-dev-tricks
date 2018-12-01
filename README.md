# WordPress Plugin Development Tricks

### Show message after Active the plugin
```php
register_activation_hook( __FILE__, 'fx_admin_notice_example_activation_hook' );

function fx_admin_notice_example_activation_hook() {
    set_transient( 'fx-admin-notice-example', true, 5 );
}

add_action( 'admin_notices', 'fx_admin_notice_example_notice' );

function fx_admin_notice_example_notice(){

    /* Check transient, if available display notice */
    if( get_transient( 'fx-admin-notice-example' ) ){
        ?>
        <div class="updated notice is-dismissible">
            <p>Thank you for using this plugin! <strong>You are awesome</strong>.</p>
        </div>
        <?php
        /* Delete transient, only display this notice once. */
        delete_transient( 'fx-admin-notice-example' );
    }
}
```

<hr>

### Exclude Post from WP_Query if has not Feature image
```php
$thumbs = array(
	'post_type'      => 'post',
	'posts_per_page' => $total_img,
	'meta_query'     => array(
		array(
			'key' => '_thumbnail_id',
		),
	),
);

$custom_query = new WP_Query( $thumbs );
```

<hr>

### Check if image requested size is exist
```php
global $_wp_additional_image_sizes;
if ( array_key_exists( 'custom-size', $_wp_additional_image_sizes ) ) {
	the_post_thumbnail( 'custom-size' );
} else {
	the_post_thumbnail( 'medium' );
}
```