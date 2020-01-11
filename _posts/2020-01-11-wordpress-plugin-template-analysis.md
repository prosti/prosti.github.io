---
id: 961
title: WordPress Plugin template analysis
date: 2012-12-31 23:31:10
author: taimane
layout: post
permalink: /wordpress-plugin-template-analysis/
published: true
categories:
   -
tags:
   -
---
Here we will set the basic elements WordPress plugin should contain. We will not bother with the implementation details. We will simple try to understand the possible and the needed functionality of any WordPress plugin.
Our analysis will mainly be based on the Empty Plugin Template that one can find <a href="http://wordpress.org/extend/plugins/empty-plugin-template/">here</a>.

Here is what is stated for this plugin and we will also examine here if this is  correct list including:
<ul>
	<li>Multi language ability</li>
	<li>Uninstall routine</li>
	<li>Data delete routine (avoid orphaned data)</li>
	<li>Coding advices</li>
	<li>Security aspects (validating user inputs, nonces)</li>
	<li>External third party stuff (Ajax, CSS, Javascript)</li>
	<li>Shortcode</li>
	<li>WordPress scheduler ("cronjob")</li>
	<li>Logfiles</li>
</ul>
First there are just several pages

<pre class="prettyprint">
ept_logfilehandling.php
ept.php
ept_settings_page2.php
ept_settings_page.php
images
lang
readme.txt
</pre>

Where images and lang are images and language folders. So there are just a few files and such a great features list.

First the constants:

<pre class="prettyprint">

// some definitions used
define( 'EPT_PUGIN_NAME', 'EPT Empty Plugin Template');
define( 'EPT_PLUGIN_DIRECTORY', 'empty-plugin-template');
define( 'EPT_CURRENT_VERSION', '0.1.1.1' );
define( 'EPT_CURRENT_BUILD', '3' );
define( 'EPT_LOGPATH', str_replace('\\', '/', WP_CONTENT_DIR).'/ept-logs/');
define( 'EPT_DEBUG', false); # never use debug mode on productive systems
// i18n plugin domain for language files
define( 'EMU2_I18N_DOMAIN', 'ept' );
</pre>

In there we have the switch to include the log files or no

<pre class="prettyprint">// how to handle log files, don't load them if you don't log
require_once('ept_logfilehandling.php');</pre>

In there three hooks have been set.

<pre class="prettyprint">register_activation_hook(__FILE__, 'ept_activate');
register_deactivation_hook(__FILE__, 'ept_deactivate');
register_uninstall_hook(__FILE__, 'ept_uninstall');</pre>

Here is how the three hooks have been implemented:

<pre class="prettyprint">// activating the default values
function ept_activate() {
	add_option('ept_option_3', 'any_value');
}

// deactivating
function ept_deactivate() {
	// needed for proper deletion of every option
	delete_option('ept_option_3');
}

// uninstalling
function ept_uninstall() {
	# delete all data stored
	delete_option('ept_option_3');
	// delete log files and folder only if needed
	if (function_exists('ept_deleteLogFolder')) ept_deleteLogFolder();
}</pre>

Here is how the menu and submenu items have been set using the functions <code>add_menu_page</code> and <code>add_submenu_page</code>.

<pre class="prettyprint">
function ept_create_menu() {

	// create new top-level menu
	add_menu_page(
	__('HTML Title', EMU2_I18N_DOMAIN),
	__('HTML Title', EMU2_I18N_DOMAIN),
	0,
	EPT_PLUGIN_DIRECTORY.'/ept_settings_page.php',
	'',
	plugins_url('/images/icon.png', __FILE__));

	add_submenu_page(
	EPT_PLUGIN_DIRECTORY.'/ept_settings_page.php',
	__("HTML Title", EMU2_I18N_DOMAIN),
	__("Menu title", EMU2_I18N_DOMAIN),
	0,
	EPT_PLUGIN_DIRECTORY.'/ept_settings_page.php'
	);

	add_submenu_page(
	EPT_PLUGIN_DIRECTORY.'/ept_settings_page.php',
	__("HTML Title2", EMU2_I18N_DOMAIN),
	__("Menu title 2", EMU2_I18N_DOMAIN),
	9,
	EPT_PLUGIN_DIRECTORY.'/ept_settings_page2.php'
	);

	// or create options menu page
	add_options_page(__('HTML Title 3', EMU2_I18N_DOMAIN), __("Menu title 3", EMU2_I18N_DOMAIN), 9,  EPT_PLUGIN_DIRECTORY.'/ept_settings_page.php');

	// or create sub menu page
	$parent_slug="index.php";	# For Dashboard
	#$parent_slug="edit.php";		# For Posts
	// more examples at http://codex.wordpress.org/Administration_Menus
	add_submenu_page( $parent_slug, __("HTML Title 4", EMU2_I18N_DOMAIN), __("Menu title 4", EMU2_I18N_DOMAIN), 9, EPT_PLUGIN_DIRECTORY.'/ept_settings_page.php');
}</pre>

Now the <code>register_settings</code> function is part of the WordPress Settings API function enabling the users to automatically generate dashboard settings page. It works right out of the box for all the standard data types, and there are no restrictions if you plan to customize the layout of the form controls.

<pre class="prettyprint">
function ept_register_settings() {
	//register settings
	register_setting( 'ept-settings-group', 'new_option_name' );
	register_setting( 'ept-settings-group', 'some_other_option' );
	register_setting( 'ept-settings-group', 'option_etc' );
}

// check if debug is activated
function ept_debug() {
	# only run debug on localhost
	if ($_SERVER["HTTP_HOST"]=="localhost" && defined('EPS_DEBUG') && EPS_DEBUG==true) return true;
}
</pre>

Actually this was all in found in the <code>Empty Plugin Template</code> WordPress plugin at Jan 1st 2013th. Probable some features from the list above are still missing.

[caption id="attachment_980" align="alignnone" width="605"]<a href="https://programming-review.com/wordpress-plugin-template-analysis/new-sheet_41ulu/" rel="attachment wp-att-980"><img class=" wp-image-980" title="Plugin features" alt="Plugin features" src="https://programming-review.com/wp-content/uploads/2012/12/New-Sheet_41ulu.jpg" width="605" height="343" /></a> Plugin features[/caption]

Thanks  
