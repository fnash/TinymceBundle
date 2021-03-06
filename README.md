# TinymceBundle

Bundle is destined to add TinyMCE WYSIWYG editor to your Symfony2 project.
By analogy with the bundle https://github.com/ihqs/WysiwygBundle

## Installation

Download the code by adding the git module or editing the deps file in the root project.

### Download via git submodule

    git submodule add git://github.com/stfalcon/TinymceBundle.git vendor/bundles/Stfalcon/Bundle/TinymceBundle

### Download by editing deps file

    [TinymceBundle]
        git=git://github.com/stfalcon/TinymceBundle.git
        target=/bundles/Stfalcon/Bundle/TinymceBundle


Modify your autoloader if you didn't installer another Stfalcon Bundle yet.
Register namespace :

    // app/autoload.php
    $loader->registerNamespaces(array(
        // ...
        'Stfalcon'                       => __DIR__.'/../vendor/bundles',
    ));

Instantiate Bundle in your app/AppKernel.php file

    public function registerBundles()
    {
        $bundles = array(
            // ...
            new Stfalcon\Bundle\TinymceBundle\StfalconTinymceBundle(),
        );
    }

## Configuration

Configure your application

    // app/config.yml
    stfalcon_tinymce:
        include_jquery: true
        tinymce_jquery: true
        textarea_class: "tinymce"
        theme:
            simple:
                mode: "textareas"
                theme: "advanced"
                theme_advanced_buttons1: "mylistbox,mysplitbutton,bold,italic,underline,separator,strikethrough,justifyleft,justifycenter,justifyright,justifyfull,bullist,numlist,undo,redo,link,unlink"
                theme_advanced_buttons2: ""
                theme_advanced_buttons3: ""
                theme_advanced_toolbar_location: "top"
                theme_advanced_toolbar_align: "left"
                theme_advanced_statusbar_location: "bottom"
                plugins: "fullscreen"
                theme_advanced_buttons1_add: "fullscreen"
            advanced:
                theme: "advanced"
                plugins: "pagebreak,style,layer,table,save,advhr,advimage,advlink,emotions,iespell,inlinepopups,insertdatetime,preview,media,searchreplace,print,contextmenu,paste,directionality,fullscreen,noneditable,visualchars,nonbreaking,xhtmlxtras,template"
                theme_advanced_buttons1: "save,newdocument,|,bold,italic,underline,strikethrough,|,justifyleft,justifycenter,justifyright,justifyfull,styleselect,formatselect,fontselect,fontsizeselect"
                theme_advanced_buttons2: "cut,copy,paste,pastetext,pasteword,|,search,replace,|,bullist,numlist,|,outdent,indent,blockquote,|,undo,redo,|,link,unlink,anchor,image,cleanup,help,code,|,insertdate,inserttime,preview,|,forecolor,backcolor"
                theme_advanced_buttons3: "tablecontrols,|,hr,removeformat,visualaid,|,sub,sup,|,charmap,emotions,iespell,media,advhr,|,print,|,ltr,rtl,|,fullscreen"
                theme_advanced_buttons4: "insertlayer,moveforward,movebackward,absolute,|,styleprops,|,cite,abbr,acronym,del,ins,attribs,|,visualchars,nonbreaking,template,pagebreak"
                theme_advanced_toolbar_location: "top"
                theme_advanced_toolbar_align: "left"
                theme_advanced_statusbar_location: "bottom"
                theme_advanced_resizing: true
            medium:
                mode: "textareas"
                theme: "advanced"
                plugins: "table,advhr,advlink,paste,xhtmlxtras,spellchecker"
                theme_advanced_buttons1: "bold,italic,underline,strikethrough,|,justifyleft,justifycenter,justifyright,justifyfull,|,forecolor,backcolor,|,hr,removeformat,|,sub,sup,|,spellchecker"
                theme_advanced_buttons2: "cut,copy,paste,pastetext,pasteword,|,bullist,numlist,|,undo,redo,|,link,unlink,anchor,cleanup,code,|,tablecontrols"
                theme_advanced_buttons3: ""
                theme_advanced_toolbar_location: "top"
                theme_advanced_toolbar_align: "left"
                theme_advanced_statusbar_location: ""
                paste_auto_cleanup_on_paste: true
                spellchecker_languages: "+English=en,Dutch=nl"
            bbcode:
                mode: "none"
                theme: "advanced"
                plugins: "bbcode"
                theme_advanced_buttons1: "bold,italic,underline,undo,redo,link,unlink,image,forecolor,styleselect,removeformat,cleanup,code"
                theme_advanced_buttons2: ""
                theme_advanced_buttons3: ""
                theme_advanced_toolbar_location: "bottom"
                theme_advanced_toolbar_align: "center"
                theme_advanced_styles: "Code=codeStyle;Quote=quoteStyle"
                entity_encoding: "raw"
                add_unload_trigger: false
                remove_linebreaks: false
                inline_styles: false
                convert_fonts_to_spans: false

run the command

    php app/console assets:install web/

to copy the resources to the projects web directory.

By default, tinymce is enabled for all textareas on the page, but if you want to customize it, do the following:

Add class "tinymce" to textarea field to initialize TinyMCE.

    <textarea class="tinymce"></textarea>

and add the parameter `textarea_class` to tinymce config. Something like that:
	stfalcon_tinymce:
			...
		    textarea_class: "tinymce"
			...

If you want to use the editor without jQuery dependancy, you can switch it to use non-jQuery version.

    stfalcon_tinymce:
        include_jquery: false
        tinymce_jquery: false
        ...

The option `include_jquery` allow to load external jQuery library from the Google CDN.

If you are using FormBuilder, use an array to add the class, you can also use the `theme` option to change the
used theme to something other than 'simple' (i.e. on of the other defined themes in your config - the example above
defined 'medium').  e.g.:

        $builder->add('introtext', 'textarea', array(
            'attr' => array(
                'class' => 'tinymce',
                'data-theme' => 'medium' // simple, advanced, bbcode
            )
        ));

Add script to your templates/layout at the bottom of your page (for faster page display).

    {{ tinymce_init() }}

## Localization

You can change language of your tiny_mce by adding language selector into theme, something like

    stfalcon_tinymce:
        ...
        theme:
            advanced:
                language: ru

### Custom buttons

You can add some custom buttons to editor's toolbar (See: http://www.tinymce.com/tryit/custom_toolbar_button.php)

First of all you should describe it in your config:

    stfalcon_tinymce:
        tinymce_buttons:
            stfalcon: # Id of your button
                title: 'Stfalcon'
                image: 'http://stfalcon.com/favicon.ico'

        theme:
            simple:
                mode: "textareas"
                theme: "advanced"
                theme_advanced_buttons1: "stfalcon,bold,italic,...

And you should create a callback function `tinymce_button_` for your button, based on your button ID:


```javascript

function tinymce_button_stfalcon(ed) {
    ed.focus();
    ed.selection.setContent('Hello from stfalcon.com :)');
}

```