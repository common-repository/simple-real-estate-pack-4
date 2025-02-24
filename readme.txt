==== Simple Real Estate Pack ====
Contributors: maxchirkov, rmcalculator
Donate link: https://www.cancer.org/aspx/Donation/DON_1_Donate_Online_Now.aspx
Tags: mortgage, mortgage calculator, real estate, realty, widget, plugin, listing, AJAX, homes, neighborhood, schools, yelp, zillow, map, trulia, altos, charts, statistics, real estate market
Requires at least: 3.0
Tested up to: 4.9.1
Stable tag: 1.4.8

Package of real estate tools and widgets designed specifically for real estate industry blogs and sites.

== Description ==
Simple Real Estate Pack is a package of real estate tools and widgets designed specifically for real estate industry blogs and web sites. The plugin includes mortgage and home affordability calculators, closing cost estimator, live mortgage rates, Trulia and ALTOS statistical charts, local schools, business listings from Yelp and Google Maps. Optionally, Simple Real Estate Pack can function as an extension for Great Real Estate (GRE) plugin, and will add new features to the GRE if it's installed. Take a look at live example of this functionality at [ScottsdaleHomes.com](http://www.scottsdalehomes.com/properties/kierland-greens-condo/).

**DEPRECATED FEATURES**

* Schools tab in Neighborhood profile currently returns no data due to API discontinuation from Education.com.
* Mortgage Rates - API is discontinued by Zillow.
* Trulia Market Charts - Trulia no longer provides them. Please use the Altos charts instead.


**API data on businesses, schools and real estate statistics is not available outside of the US.**

**Features Include:**

1.  Calculators
       * Mortgage Calculator (widget and shortcode)
       * Affordability Calculator (widget and shortcode)
       * Closing Costs Estimator (widget and shortcode)

2. Schools - shortcode widget provides a list of schools within selected location (via Education.com API). Can group schools by type, grade level, school district or zip code.

3. Market trends and statistical graphs/charts via ALTOS Research.

4. Embed Google Maps with a click of a mouse with grocery stores, restaurants, gas stations, banks, golf courses and hospitals (optional) within 3 mile radius of the main marker (via Yelp API).

5. Publish Yelp listings (shortcode) within 3 mile radius from a specified point into you content. Grouped with tabs by business type (i.e. grocery stores, restaurants etc.).

6. Walk Score via Walkscore.com

7. Extension of GRE plugin (optional) via Neighborhood Profile options - mapping local grocery stores, restaurants, gas stations, banks, golf courses and hospitals within 3 mile radius of the property. Includes property location, contact information, ratings via Yelp API., as well as financial tools and statistical charts.

8. Easy to use API for third party widget integration into the Neighborhood Profiles.

For detailed usage instructions visit the [official site](http://www.phoenixhomes.com/tech/simple-real-estate-pack "Simple Real Estate Pack by PhoenixHomes.com").


* Author: Max Chirkov
* Author URI: [http://www.PhoenixHomes.com](http://www.PhoenixHomes.com "Phoenix Real Estate")
* Copyright: Released under GNU GENERAL PUBLIC LICENSE


== Installation ==

**Install like any other basic plugin:**

1. Unzip and copy the simple-real-estate-pack folder to your /wp-content/plugins/ directory

2. Activate the Simple Real Estate Pack on your plugins page.

3. Go to the Real Estate Pack Settings Page and adjust options to fit your needs.

4. A lot of the functionality of the plugin depends on third party APIs. To take advantage of all the features it's highly recommended that you obtain (free) API keys for each service.


**Using Neighborhood Profiles in Templates**

*Note: This requires good understanding of HTML markup and experience in editing WordPress templates.*

= Extending GRE listings with SRP Neighborhood Profiles =

Simple Real Estate Pack can output information like map, schools, local businesses and market statistics within certain radius of the location of the property listing, presented by GRE plugin.

**GRE supports 2 ways of presenting listings:**
1. Via default auto-generated templates of listings summary and individual listings pages.
2. Via custom templates that have be located inside your theme’s folder.

**First Method doesn’t require any HTML or PHP skills, but some knowledge of CSS may be needed.**
1.  After GRE and SRP are installed, assure that Permalinks for your website are activated. You can do that via Settings -> Permalinks. This step is required by the GRE plugin.
2.  Go into GRE Settings. Make sure that the Main Listings Page is selected (if you don’t have one, create a blank one and then select it in the settings).
3.  Listing Summary and Individual Listings Pages must be checked. Leave the rest of the checkboxes unchecked.
4.  Make sure that comments and trackbacks are deactivated on your Main Listings Page and all individual listings as well.
5.  That’s it – SRP information should appear below each listing on the individual listing pages. Follow the GRE instructions on how to create and manage listings.

*Even though GRE and SRP plugins come with some pre-defined styles, which can be activated via their respective settings pages, remember that only one style should be used, otherwise they will conflict. Some themes may conflict with the default styles as well, so you may need to make adjustmetns in your own theme's style.css file.*

**Second Method:  via custom listings templates. In short - you MUST know what you’re doing.**
If you don’t know what “loop” means in WordPress, and how to create page templates – this section will not teach you how to do it, so you better find someone who can do it for you.

Create a new page template. This is how I do it:

**1.** Create a copy of your page.php template and name it listingpage.php. It has to be in your theme's folder where other templates are (page.php, post.php etc.).

**2.** Open the file to edit, and place the following code at the top of the file:

`
<?php
/*
Template Name: SRP Listing Page
*/
?>
`

**3.** Place the following code inside the loop of the page template. You might also want to delete everything else from the loop, otherwise you'll endup with some duplicate content on the same page.

`<?php
if(SRP_DIR)
    include (SRP_DIR . '/templates/listing_page.php');
?>`

**4.** Now, you should be able to use SRP Listing Page template from the drop-down selection list when editing your listings.

= Using SRP Neighborhood Profiles in Other Custom Templates =

Assuming that a number of values that refer to property location will be passes to your custom template, you need to define the following global variable as an associative array with preset keys followed by the `srp_profile()` function:

`<?php
$srp_property_values = array(
    'lat' => '',
    'lng' => '',
    'address' => '',
    'city' => '',
    'state' => '',
    'zip_code' => '',
    'listing_price' => '',
    'bedrooms'  => '', //optional
    'bathrooms' => '', //optional
    'html' => '', //optional
);
if(function_exists('srp_profile')){
    srp_profile();
}
?>`
*All parameters are required except the noted ones. The variable's name (`$srp_property_values`) required to be exact.*

**Using SRP API**

Simple Real Estate Pack's API allows developers to add their own widgets to the Neighborhood Profile output. Those widgets will play by the same rules as the built-in ones - they can load statically into the page or via AJAX, can be presented via tabs and their tab names and subtitles can be customized via plugin settings page by the end-user.

This implementation consists of 3 simple steps:

**1.** The actual function that returns your widget's content. You can use global `$srp_property_values` variable to get access to the initial property/location parameters if needed.

`<?php
function my_custom_srp_widget_content() {
    global $srp_property_values;

    ..do something to generate content..

    return $content;
}
?>`

**2.** The function that initializes your widget and adds all the necessary information about it to the $srp_widgets object:

`<?php
function my_custom_srp_widget_content_init($init) {
    $array = array(
            'name' => 'widget_name', //lower case and no special characters
            'title'  => 'My Custom Widget Title', //will used as subtitle within content
            'tab_name' => 'Widget Tab Name', //short 1-2 word tab name
            'callback_function' => 'my_custom_srp_widget_content', //callback widget content function
            'init_function' => __FUNCTION__, //don't change - this is reference to the current function
            'ajax' => false, //bool - false or true grants an option for your widget to be loaded via AJAX.
            'save_to_buffer' => false, //bool - change to true if your widget's content needs to be cached before the output. For example your widget content function doesn't return the data, but instead outputs it directly. If you change to TRUE, it will cache the output and return in correctly.
            );
    $init[] = $array;
    return $init;
}
?>`

*Note: all parameters of the $array are required. Make sure your _init function references the $init argument and always returns $init.*

**3.** The last step is to add a filter that will add your new widget init to the object preparation process:

`<?php
add_filter('srp_prepare_widgets_object', 'my_custom_srp_widget_content_init');
?>`

*Note: if you want to change an order in which widgets load, simply add 3rd numerical parameter to the filter. Keep in mind that Google Map is set to load first (and should stay that way), due to some unsolved issue.*

== Screenshots ==

1. Mapping - outputs schools, grocery stores, restaurants, banks, gas stations, golf courses and hospitals in the neighborhood of property listings (Can also work as extension to [Great Real Estate plugin](http://wordpress.org/extend/plugins/great-real-estate/)).
2. Financial Tools.
3. Trulia Market Trends.
4. Local Schools.
5. Settings page for Mortgage Calculator and Closing Costs Estimator.
6. Altos Market Charts.
7. Local businesses via Yelp.
8. Walk Score.
9. Tabbed presentation of data.
10. Neighborhood profile options.
11. Custom tab names and subtitles of the widgets.
12. TinyMCE widgets in post/page editor.
13. Yelp TinyMCE widget to insert a shortcode into the post/page.
14. Financial Tools TinyMCE widget to insert a shortcode into the post/page.
15. Google Maps TinyMCE widget to insert a shortcode into the post/page.
16. Altos Stats TinyMCE widget to insert a shortcode into the post/page.
17. Trulia Stats TinyMCE widget to insert a shortcode into the post/page.

== Frequently Asked Questions ==

= Yelp Places do not get rendered on the map =

Simple Real Estate Pack loads Google Maps API. Sometimes a theme or another plugin can load a copy of the same API on their own, which can create a conflict.
If there's a JavaScript error in your browser's console similar to this: "You have included the Google Maps API multiple times on this page. This may cause unexpected errors."
You can unload SREP's Google Maps API by pasting the following code into your theme's functions.php file:

`<?php
add_action('wp_print_scripts', 'remove_SREP_googleMapsAPI', 100);

function remove_SREP_googleMapsAPI()
{
	wp_dequeue_script( 'google-maps-api-v3' );
}
?>`

Clear your browser cache and reload the page.

**If something doesn't work, please try to troubleshoot the issue by checking is any JavaScript errors are reported. Also, try disabling other plugins and leave the SRP enabled just to make sure there are no conflicts.**

== Changelog ==

**Version 1.4.8**
- Fixes: mortgage calc not taking into effect some of the shortcode attributres.
- Fixes: neighborhood shortcode's location of the nearby businesses.

**Version 1.4.7**
- Fixes small issue with the Thickbox modals.
- Fixes tinymce widgets compatibility with the new TinMCE 4.x

**Version 1.4.6**
- Updates deprecated class constructors.
- Updates Walkscore widget to the latest version.
- Improvement: makes mortgage calculator thickbox dialogs mobile friendly.

**Version 1.4.5**
- Deprecates Mortgage Rates due to discontinuation of the API by Zillow.
- Fixes negative results in the Affordability Calculator.

**Version 1.4.4**
- Adds support for Yelp API 2.0
- Adds support for Google Map API Key (which is now required by Google)
- Fixes PHP 7 fatal error.

**Version 1.4.3**
- Fixes: mortgage calculator widget not inheriting default settings.

**Version 1.4.1**
- WordPress 4.3 compatibility update (deprecated widget constructors).
- Fixes: PHP warnings.
- Fixes: places weren't getting rendered on the neighborhood map.

**Version 1.4.0**
- Numerous bug fixes.

**Version 1.3.0**
- WP 3.9 compatibility fixes.
- Fixed: Altos Research charts weren't getting displayed due to a new parameter missing in the URL.
- Other widget fixes and minor updates.

**Version 1.2.10**
- Bug Fix: Schools weren't appearing if radius was not set in the Neighborhood settings.
- Bug Fix: JS error was thrown on pages that didn't have tabs.
- Improved: Google Maps API script only loads on pages with maps.
- Improved: Prefixed UI Tabs CSS with SRP specific ID.

**Version 1.2.9**

- Bug Fix: Schools title wasn't displaying correctly.
- Cleaned up PHP notices and updated deprecated functions.

**Version 1.2.7**

- Minor JS fixes for WP 3.5 compatibility.

**Version 1.2.7**

- Changed wp_enqueue() handle for Google Maps API JS to avoid potential conflicts.
- Bug Fix: Regular Google Map embedded via shortcode wasn't loading javascripts.

**Version 1.2.6**

- Bug Fix: mortgage calculator widgets weren't working when Tabbed Presentation was turned off in the settings.
- Map width now can be set in % - just include % symbol into the width field on the Neighborhoods settings page.
- CSS fix for themes that have broken tabs or tabs with bullets.
- CSS fix for themes that have unintentionally extended footer caused by hidden tabbed information overflow (schools, yelp data).
- CSS fix for checkbox selectors on the map to display them in concise fashion.

**Version 1.2.5**

- Bug Fix: "number_format() expects parameter 1 to be double" - explicitely defined (float).
- Misc. fixes and code clean up.

**Version 1.2.4**

- Fixes CSS issue where Schools were overlaying main content on the page.
- Changed TinyMCE plugins to open windows via Ajax rather than direct calls.
- Changed to load JS only on the pages that utilizing the plugin.
- Misc. clean up.

**Version 1.2.3**

- Bug Fix: reworked yelp logic to fix "no businesses" issue, that was appearing for all categories even if only one was empty.
- Bug Fix: Thickbox incompatibility with new jQuery 1.6.x. Needed additional function to kill propagation of uload event.

**Version 1.2.2**

- Fixed: blank emails were sent out on each visit of listing details pages.
- Fixed: Education API requires lat/lng explicitly defined if distance parameter is used. Added a condition where parameter distance is dropped if lat/lng is not set.

**Version 1.2.0**

- Added support for default templates if "Listings Summary" and "Individual Listings Page" settings of GRE are checked.
- Added a simple inquiry form for the GRE Listing Page template; also an option to enter shortcodes for custom forms supplied by other plugins.
- Added radius options for Yelp and Schools that can be customized for the Neighborhood Profiles.
- Added chart and map dimensions options for the Neighborhood Profile outputs.
- Fixed "double-window" issue with the ThickBox effect.
- Discontinued support for the Rento-Meter - the service sucks and never really worked well.

**Version 1.1.4**

- Switched stats image check from fopen to curl. Works much faster since we're pretty much getting only response header.

**Version 1.1.3**

- Added messages for schools and businesses if no records returned within specified radius. The same applies to the maps when clicking on checkboxes.
- Added dummy image output with a message if market stats is not available.
- Fixed bug for multi-word cities in school requests.

== Upgrade Notice ==

It is strongly recommended to re-visit the plugin's settings pages and save them.

If you have been using the Great Real Estate Plugin prior to upgrade, you need to update your listingpage.php file by following the Installation instructions in readme.txt.
