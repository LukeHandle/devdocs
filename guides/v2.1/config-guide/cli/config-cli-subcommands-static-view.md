---
layout: default
group:  config-guide
subgroup: 04_CLI
title: Deploy static view files
menu_title: Deploy static view files
menu_node: 
menu_order: 300
version: 2.1
github_link: config-guide/cli/config-cli-subcommands-static-view.md
redirect_from: /guides/v1.0/config-guide/cli/config-cli-subcommands-static-view.html
---

## Overview of static view files deployment {#config-cli-static-overview}
The static view files deployment command enables you to write static files to the Magento file system when the Magento software is set for <a href="{{page.baseurl}}config-guide/bootstrap/magento-modes.html#mode-production">production mode</a>.

The term *static view file* refers to the following:

*	"Static" means it can be cached for a site (that is, the file is not dynamically generated). Examples include images and CSS generated from LESS.
*	"View" refers to presentation layer (from MVC).

Static view files are located in the `<your Magento install dir>/pub/static` directory, and some are cached in the `<your Magento install dir>/var/view_preprocessed` directory as well.

Static view files deployment is affected by Magento modes as follows:

*	<a href="{{page.baseurl}}config-guide/bootstrap/magento-modes.html#mode-default">The default</a> and <a href="{{page.baseurl}}config-guide/bootstrap/magento-modes.html#mode-developer">developer mode</a>: Magento generates them on demand, but the rest are cached in a file for speed of access.
*	<a href="{{page.baseurl}}config-guide/bootstrap/magento-modes.html#mode-production">The production</a> mode: Static files are *not* generated or cached. 

	You must write static view files to the Magento file system manually using the command discussed in this topic; after that, you can restrict permissions to limit your vulnerabilities and to prevent accidental or malicious overwriting of files.

<div class="bs-callout bs-callout-warning" markdown="1">
_Developer mode only_: When you install or enable a new module, it might load new JavaScript, CSS, layouts, and so on. To avoid issues with static files, you must clean the old files to make sure you get all the changes for the new module.

You can clean generated static view files in several ways, see the <a href="{{page.baseurl}}howdoi/clean_static_cache.html">Clean static files cache topic for details</a>.
</div>

## First steps {#config-cli-before}
{% include install/first-steps-cli.html %}
In addition to the command arguments discussed here, see <a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands.html#config-cli-subcommands-common">Common arguments</a>.

## Deploy static view files {#config-cli-subcommands-xlate-dict}
To deploy static view files:

1.	Log in to the Magento server as, or <a href="{{page.baseurl}}install-gde/prereq/file-sys-perms-over.html">switch to</a>, the Magento file system owner.
2.	Delete the contents of `<your Magento install dir>/pub/static`.
3.	Run the static view files deployment tool `<your Magento install dir>/bin/magento setup:static-content:deploy`.
<!-- 4.	Set read-only file permissions for the `pub/static` directory, its subdirectories, and files. -->
	
	<div class="bs-callout bs-callout-info" id="info">
		<span class="glyphicon-class">
  		<p>If you enable static view file merging in the Magento Admin, the <code>pub/static</code> directory system must be writable.</p></span>
	</div>

Command options:

	magento setup:static-content:deploy [<list of languages>] [-t|--theme[="<theme>"]] [--exclude-theme[="<theme>"]] [-l|--language[="<language>"]] [--exclude-language[="<language>"]] [-a|--area[="<area>"]] [--exclude-area[="<area>"]] [-j|--jobs[="<number>"]]  [--no-javascript] [--no-css] [--no-less] [--no-images] [--no-fonts] [--no-html] [--no-misc] [--no-html-minify] [-d|--dry-run] 

The following table discusses the meanings of this command's parameters and values. 

<table>
	<col width="15%">
  	<col width="65%">
  	<col width="25%">
	<tbody>
		<tr>
			<th>Option</th>
			<th>Description</th>
			<th>Required?</th>
		</tr>
	<tr>
        <td>&lt;lang></td>
        <td><p>List of <a href="http://www.loc.gov/standards/iso639-2/php/code_list.php" target="_blank">ISO-636</a> language codes for which to output static view files. (Default is <code>en_US</code>.)</p>
        <p>You can find the list by running <code>magento info:language:list</code>.</p></td>
    <td><p>No</p></td>
    </tr>
	<tr>
        <td>--language (-l)</td>
        <td><p>Generate files only for the specified languages. The default, with no option specified, is to generate files for all <a href="http://www.loc.gov/standards/iso639-2/php/code_list.php" target="_blank">ISO-636</a> language codes. You can specify the name of one language code at a time.</p>
            <p>For example, <code>--language en_US --language es_ES</code></p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--exclude-language</td>
        <td><p>Generate files for the specified language codes. The default, with no option specified, is to exclude nothing. You can specify the name of one language code or a comma-separated list of language codes.</p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--theme &lt;theme></td>
        <td><p>Themes for which to deploy static content.</p>
            <p>For example, <code>--theme Magento/blank --theme Magento/luma</code></p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--exclude-theme &lt;theme></td>
        <td><p>Themes to exclude when deploying static content.</p>
            <p>For example, <code>--exclude-theme Magento/blank --theme Magento/luma</code></p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--area (-a)</td>
        <td><p>Generate files only for the specified areas. The default, with no option specified, is to generate files for all areas. Valid values are <code>adminhtml</code> and <code>frontend</code>.</p>
            <p>For example, <code>--area adminhtml</code></p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--exclude-area</td>
        <td><p>Do not generate files for the specified areas. The default, with no option specified, is to exclude nothing. </p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--jobs (-j)</td>
        <td><p>Enable parallel processing using the specified number of jobs. The default is 4. To cause the task to run in one process (for example, if your system does not support process forking), use <code>--jobs 1</code>.</p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--no-javascript</td>
        <td><p>Do not deploy JavaScript files</p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--no-css</td>
        <td><p>Do not deploy CSS files. </p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--no-less</td>
        <td><p>Do not deploy LESS files. </p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--no-images</td>
        <td>Do not deploy images.</td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--no-fonts</td>
        <td><p>Do not deploy font files.</p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td><p>--no-html</p></td>
        <td><p>Do not deploy HTML files.</p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--no-misc</td>
        <td><p> Do not deploy other types of files (that is <code>.md</code>, <code>.jbf</code>, <code>.csv</code>, <code>.json</code>, <code>.txt</code>, <code>.htc</code>, or <code>.swf</code> files). </p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--no-html-minify</td>
        <td><p> Do not minify HTML files.  </p></td>
        <td><p>No</p></td>
    </tr>
    <tr>
        <td>--dry-run (-d)</td>
        <td><p>Include to view the files output by the tool without outputting anything.</p></td>
        <td><p>No</p></td>
    </tr>
    

	</tbody>
</table>

<div class="bs-callout bs-callout-info" id="info" markdown="1">
*   If you specify values for both `lang` and `--language`, `<lang>` takes precedence.
*   The following parameters were added in version 2.1.1: `--exclude-language`, `--theme <theme>`, `--exclude-theme <theme>`, `--area (-a)`, `--exclude-area`, `--jobs (-j)`, `--no-javascript`, `--no-css`, `--no-less`, `--no-images`, `--no-fonts`, `--no-html`, `--no-misc`, `--no-html-minify`.
</div>

### Deploy static view files without installing Magento {#deploy_without_db}
We regret that this information was added in error. You _cannot_ yet deploy static view files without a connection to the Magento database. We expect this ability will be added in a future release. We apologize for any inconvenience this might have caused.

You should be able to [compile code]({{ page.baseurl}}config-guide/cli/config-cli-subcommands-compiler.html) without a connection to the Magento database.


<h2 id="view-file-trouble">Troubleshooting the static view files deployment tool</h2>
<a href="{{page.baseurl}}install-gde/bk-install-guide.html">Install the Magento software first</a>; otherwise, you cannot run the static view files deployment tool.

**Symptom**: The following error is displayed when you run the static view files deployment tool:

	ERROR: You need to install the Magento application before running this utility.

**Solution**:

Use the following steps:

1.	Install the Magento software in any of the following ways:

	*	<a href="{{page.baseurl}}install-gde/install/cli/install-cli.html">Command line</a>
	*	<a href="{{page.baseurl}}install-gde/install/web/install-web.html">Web Setup wizard</a>

1.	Log in to the Magento server as, or <a href="{{page.baseurl}}install-gde/prereq/file-sys-perms-over.html">switch to</a>, the Magento file system owner.
2.	Delete the contents of `<your Magento install dir>/pub/static` directory.
3.	<a href="#config-cli-subcommands-xlate-dict">Run the static view files deployment tool</a>.
<!-- 4.	Set read-only file permissions for the `pub/static` directory, its subdirectories, and files. -->
	
	<!-- <div class="bs-callout bs-callout-info" id="info">
		<span class="glyphicon-class">
  		<p>If you enable static view file merging in the Magento Admin, the <code>pub/static</code> directory system must be writable.</p></span>
	</div> -->

## Tip for developers customizing the static content deployment tool
When creating a custom implementation of the static content deployment tool, use only [atomic](https://en.wikipedia.org/wiki/Linearizability){:target="_blank"} file writing for files that should be available on the client. If you use non-atomic file writing, those files might be loaded on the client with partial content. 

One of the options for making it atomic is to write to files stored in a temporary directory and copying or moving them to the destination directory (from where they are loaded to client) after writing is over. For details about writing to files, see [http://php.net/manual/en/function.fwrite.php](http://php.net/manual/en/function.fwrite.php){:target="_blank"}.

#### Related topics
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-cache.html">Manage the cache</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-index.html">Manage the indexers</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-cron.html">Configure and run cron</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-compiler.html">Code compiler</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-mode.html">Set the Magento mode</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-urn.html">URN highlighter</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-depen.html">Dependency reports</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-i18n.html">Translation dictionaries and language packages</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-less-sass.html">Create symlinks to LESS files</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-test.html">Run unit tests</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-layout-xml.html">Convert layout XML files</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-perf-data.html">Generate data for performance testing</a>
