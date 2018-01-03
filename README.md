
security :
wp-login.php 

authro area
in 
wp-admin folder

plugin 
rename  wp-login.php 

rename wp-login.php 


wp-config.php
define('WP_AUTO_UPDATE','true')


????? ????????? ?? ?? ???? ???????? ?????? ??????


//remove Paragraph Element From Posts 

function remove_p()
{
   remove_filter('the_content','wpautop');
   return $content;
}

// add tag , function , order 
add_filter('the_content','remove_p',0);


//breadcrumbs

html code
include('includes/breadcrumb.php');
<?php  
  $the_cats = get_the_category();
?>
<div class="breadcrumbs-holder">
<div class='container'>
    <ol>
        <li><a href='<?php echo get_home_url();?>'> Home
                <?php echo get_bloginfo('name'); ?>
         </a></li>
        <li>
          <a href='<?php echo esc_url(get_category_link($the_cats[0]->item_id)); ?>'><?php echo esc_html($the_cats[0]->name); ?></a>
        </li>
        <li>
          <?php echo get_the_title() ; ?>
        </li>
    </ol>
</div>


//widgets القائمة الجانبية
//statisti  for category posts
<?php
  //get categorys counts 
  $comments_arg = array('status' => 'approve');
  $comments_counts = 0;
  $all_comments = get_comments($comments_arg);
  foreach($all_comments as $comment)
  {
     $post_id = $comment->comment_post_ID;
     if(! in_category('linux',$post_id))
     {
        continue;
     }
     $comment_counts++;
  }
?>
<h2><?php echo single_cat_title(); ?> Statistics</h2>
//posts in category
$cat = get_queried_object();
$postscounts = $cat->count;

//latest posts

<h3>Latest Categroy</h3>
<ul>
<?php 
$query = new WP_Query(['posts_per_page' => 5,'cat' => 8]);

if($query->have_posts())
while($query->have_posts())
{
    $query->the_post()
	echo "<li><a href='". the_permalink() ."'>".the_title()."</a></li>";
}
wp_reset_postdata();
?>
</ul>

//hot posts by comments
<?php 
$query = new WP_Query(['posts_per_page'=> 2,'orderby' => 'comment_count']);

if($query->have_posts())
while($query->have_posts())
{
    $query->the_post()
	echo "<li><a href='". the_permalink() ."'>".the_title()."</a></li>";
}
wp_reset_postdata();
?>

//dynimc sidebar 
<?php 
  if(is_active_sidebar('main-sidebar'))
  {
      dynamic_sidebar('main-sidebar');
  }
//in functions.php 

rester_sidebar

function theme_sidebar()
{
   register_sidebar(
   array(
   'name' => 'Main Sidebar',   
   'id' => 'main-sidebar',
   'description' => '',
   'class' =>'',
   'before_widget' => '',
   'after_widget' =>'',
   'before_title' => '',
   'after_title' => '')
   );
}

add_action('widget_init','theme_sidebar');

//make sidebar 
sidebar.php in theme folder

//category 
category.php in theme folder

<div class='category-information'>
  <h1><?php single_cat_title(); ?></h1>
  <p><?php echo category_description();</p>
</div>

//category-slug or name or id
category-javascript.php in theme folder

//شرح تتميز تعليق الكاتب داخل صفحة المقال
change css style in comment box

.post-page .bypostauthor:after{
	content:"Author"
}

.post-page .bypostauthor{
	
}

//404.page 
make 404.page in theme folder

//pagintions 
in function.php 

function number_page()
{
	global $wp_query;
	$all_pages = $wp_query->max_num_pages;
	$current_page = max(1,get_query_var('paged'));
	if($all_pages > 1)
	{
	   return paginate_links([
	     'base' => get_pagenum_link().'%_%',
	     'format' => '/page/%#%',
	     'current' => $current_page,
	     //mid_size default 2 make before and after size of pages
	     'mid_size' => 1,
	     'end_size' => 1
	   ]);
	}	
}

prev 
if(get_previous_posts_link())
{
	previous_posts_link('<i>');
}
next
if(get_next_posts_link())
{
	next_posts_link('<i>');
}
