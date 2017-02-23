# Loop

```
<?php $args = array( 'post_type' => 'post_type', 'posts_per_page' => -1 ); ?>
<?php $loop = new WP_Query( $args ); ?>
<?php while ( $loop->have_posts() ) : $loop->the_post(); ?>

<?php endwhile;  wp_reset_query();?>
```


# Custom post types

### Add Thumbnail image

`<?php the_post_thumbnail_url(); ?>`

### Add custom fields

`<?php echo the_field(); ?>`

### Add Title
`<?php the_title(); ?>`

### Get post ID
`<?php echo get_the_ID(); ?>`

### Get template URL
`<?php echo get_template_directory_uri(); ?>`

### Show if field exists
```
<?php if( get_field() ): ?>
<?php endif; ?>
```

### Next button
```
<?php $posts = query_posts($query_string); while (have_posts()) : the_post(); ?>
    <?php next_post_link( '%link', ' previous story' ); ?>
    <?php previous_post_link( '%link', 'next story ' ); ?>
<?php endwhile;?>
```

### Get post date
`<?php the_time( get_option( 'date_format' ) ); ?>`


### Permalink
`<?php the_permalink(); ?>`


### Update dashboard

```
// Removes default posts and comments from at glance
add_action( 'do_meta_boxes', 'custom_do_meta_boxes', 99, 2 );

function custom_do_meta_boxes( $screen, $place )
{
    if( 'dashboard' === $screen && 'normal' === $place )
    {
        add_filter( 'wp_count_posts', 'custom_wp_count_posts' );
        add_filter( 'wp_count_comments', 'custom_wp_count_comments' );
    }
}

function custom_wp_count_posts( $stats )
{
    static $filter_posts = 0;
    if( 1 === $filter_posts )
        remove_filter( current_filter(), __FUNCTION__ );

    $filter_posts++;
    return null;
}

function custom_wp_count_comments( $stats )
{
    static $filter_comments = 0;
    if( 1 === $filter_comments )
        remove_filter( current_filter(), __FUNCTION__ );

    $filter_comments++;
    return array( 'total_comments' => 0 );
}


// Adds custom posts to at glance
add_action( 'dashboard_glance_items', 'cpad_at_glance_content_table_end' );
function cpad_at_glance_content_table_end() {
    $args = array(
        'public' => true,
        '_builtin' => false
    );
    $output = 'object';
    $operator = 'and';

    $post_types = get_post_types( $args, $output, $operator );
    foreach ( $post_types as $post_type ) {
        $num_posts = wp_count_posts( $post_type->name );
        $num = number_format_i18n( $num_posts->publish );
        $text = _n( $post_type->labels->singular_name, $post_type->labels->name, intval( $num_posts->publish ) );
        if ( current_user_can( 'edit_posts' ) ) {
            $output = '<a href="edit.php?post_type=' . $post_type->name . '">' . $num . ' ' . $text . '</a>';
            echo '<li class="post-count ' . $post_type->name . '-count">' . $output . '</li>';
        }
    }
}

```
