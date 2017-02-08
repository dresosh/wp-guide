# Loop

```
<?php $args = array( 'post_type' => 'development', 'posts_per_page' => -1 ); ?>
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
