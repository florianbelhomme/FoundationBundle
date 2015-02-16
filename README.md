# [Flob Foundation Bundle](https://github.com/florianbelhomme/FlobFoundationBundle)

By [Florian Belhomme](http://florianbelhomme.com)

[![Total Downloads](https://poser.pugx.org/florianbelhomme/flob-foundation-bundle/downloads.svg)](https://packagist.org/packages/florianbelhomme/flob-foundation-bundle)
[![Build Status](https://travis-ci.org/florianbelhomme/FlobFoundationBundle.svg?branch=tests)](https://travis-ci.org/florianbelhomme/FlobFoundationBundle)
[![SensioLabsInsight](https://insight.sensiolabs.com/projects/4ffe439b-5a0c-4caa-914e-005d21591c3d/mini.png)](https://insight.sensiolabs.com/projects/4ffe439b-5a0c-4caa-914e-005d21591c3d)

## About

This bundle integrates the functionalities of the responsive framework Foundation, from ZURB (thanks guys), into Symfony by providing templates, Twig extensions, services and commands. You can quickly setup a responsive theme for an interface for your project. It will have the "look'n'feel", the responsiveness and the simplicity of Foundation.

**BE AWARE: THIS BUNDLE WILL NOT ADD THE FOUNDATION FRAMEWORK BUT RATHER FUNCTIONALITIES FOR SYMFONY TO WORK WITH IT**

To include all the libraries you can use a CDN like [CloudFlare CDN](http://cdnjs.com/).

## Requirements

- [Symfony 2.3+](http://symfony.com).
- [jQuery 2+](http://jquery.com/) a JavaScript library.
- [Foundation 5+](http://foundation.zurb.com/) an advanced responsive framework.
- [Font Awesome 4+](http://fontawesome.io/) which comes with 369+ icons.

## Recommended

- the [KnpMenuBundle](https://github.com/KnpLabs/KnpMenuBundle) for menus, this bundle can theme them for you.
- the [KpnPaginatorBundle](https://github.com/KnpLabs/KnpPaginatorBundle) for pagination, this bundle can theme them for you.

## Changelog

v2
- namespace renaming
- new form types : button_group and button_bar
- rework the twig form template
- rework the KnpMenu template and make it compatible with KnpMenu 2.0
- fixes to support Foundation 5.5.0

## TODO to go stable

- [X] Pagination with PagerFanta + Configuration + Doc
- [X] Breadcrumb with KnpMenu
- [X] Sidebar with KnpMenu
- [ ] Refactor the doc and update it
- [ ] Provide the demo
- [ ] Setup & writes tests
- [ ] Tarvis

## In the future

- [ ] Thumbnails + Clearing Lightbox
- [ ] Icon Bar
- [ ] Support for off-canvas
- [ ] Modal
- [ ] Icon with labels
- [ ] Accordion
- [ ] Tabs
- [ ] Abide Validation

## Installation and configuration

First, edit your `composer.json` and add :

```JSON
{
    ...
    "require": {
        ...
        "florianbelhomme/flob-foundation-bundle": "dev-master"
        ...
    }
    ...
}
```

Now run a `composer update` on your project. It will get all the necessary files for you.

Secondly, edit your `app/AppKernel.php` and add:

```PHP
class AppKernel extends Kernel
{
    public function registerBundles()
    {
        $bundles = array(
            ...
            new Flob\Bundle\FoundationBundle\FlobFoundationBundle(),
            ...
        );
    }
}
```

You now need to add the libraries to your project.

The easy way to do it (but there are other ways to do so):

```HTML
<html>
    <head>
        ...
        <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/foundation/5.5.0/css/normalize.min.css" type="text/css" />
        <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/foundation/5.5.0/css/foundation.min.css" type="text/css" />
        <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/font-awesome/4.2.0/css/font-awesome.min.css" type="text/css" />
        <link rel="stylesheet" href="{{ asset('bundles/flobfoundation/css/foundationtosymfony.css') }}" type="text/css" />
        <script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/modernizr/2.8.3/modernizr.min.js"></script>
        ...
    </head>
    <body>
        ...
        <script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
        <script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/foundation/5.5.0/js/foundation.min.js"></script>
        <script type="text/javascript" src="{{ asset('bundles/flobfoundation/js/foundationtosymfony.js') }}"></script>
    </body>
</html>
```

Your project is ready!

## Configuration

**This bundle does not theme any elements by default**, in case you want to use Foundation on a specific form or bundle.

To automatically theme forms or other elements by default, go into the `app/config/config.yml` and add at the end:

```YAML
flob_foundation:
    theme: { form: true, knp_menu: true, knp_paginator: true, pagerfanta: true }
```

If you want to do specific HTML markup that extends templates of this bundle:
* create your template in your bundle
* add `{% extends 'FlobFoundationBundle:Form:foundation_form_div_layout.html.twig' %}` at the top (the form template, for example)
* edit the `app/config/config.yml`:

```YAML
flob_foundation:
    theme: { form: true, knp_menu: true, knp_paginator: true, pagerfanta: true }
    template:
        form: 'YourBundle:YourFolder:formtemplate.html.twig'
        breadcrumb: 'YourBundle:YourFolder:breadcrumbtemplate.html.twig'
        knp_menu: 'YourBundle:YourFolder:menutemplate.html.twig'
        knp_paginator: 'YourBundle:YourFolder:paginatortemplate.html.twig'
        pagerfanta: 'YourPagerFantaTemplate'
```

## Usage

### Theme

However instead of setting it in the configuration, you can theme specific elements using one of these methods:

```Twig
{# Form #}
{% form_theme yourform 'FlobFoundationBundle:Form:foundation_form_div_layout.html.twig' %}

{# Menu #}
{{ knp_menu_render('yourmenu', {'template' : 'FlobFoundationBundle:Menu:foundation_knp_menu.html.twig'}) }}

{# Pagination with KNP paginator #}
{{ knp_pagination_render(yourpagination, 'FlobFoundationBundle:Pagination:foundation_sliding.html.twig') }}

{# Pagination with PagerFanta (through WhiteOctober bundle) #}
{{ pagerfanta(paginationFanta, 'foundation') }}
```

### Breadcrumb

If you want a breadcrumb generated from a KNP Menu :

* Make sure that your menu builder get the current URI by adding this :

```Php
public function createMyMenu(Request $request)
{
    $menu = $this->factory->createItem('Home', array('route' => 'homepage')));

    // ... Add entries here ...

    $menu->setCurrentUri($request->getBaseUrl().$request->getPathInfo());

    return $menu;
}
```

* Then add this code in your template :

```Twig
{{ flob_foundation_breadcrumb_render('yourknpmenu') }}
```

If you want a specific template :

```Twig
{{ flob_foundation_breadcrumb_render('yourknpmenu', {'template' : 'YourBundle:YourFolder:breadcrumbtemplate.html.twig') }}
```

### Slider (form field type)

You can now use the slider in your forms.

The slider extend the [number field type](http://symfony.com/doc/current/reference/forms/types/number.html), so it has the same options.
The additional options are :
* start :
type: integer / default: 0
This specifies the starting point number.
* end :
type: integer / default: 100
This specifies the highest number in the range.
* step :
type: integer / default: 1
This specifies the cursor's incremental skip.
* vertical :
type: boolean / default: false
If true, displays the slider vertically instead of horizontally.

This is an example of the field :
```Php
$builder->add('My slider', 'slider', array('label' => 'Slider', 'start' => 10, 'end' => 20, 'step' => 2));
```

### Switch (form field type)

You can now use the switch in your forms.

The switch extend the [choice field type](http://symfony.com/doc/current/reference/forms/types/number.html), so it have the same options. But you can't set the option "expanded" to false (cannot be a select).

This is an example of the field :
```Php
$builder->add('switch_radio', 'switch', array('label' => 'Switch (as radio)', 'choices' => array(1 => 'Choice 1', 2 => 'Choice 2', 3 => 'Obi wan kenobi'), 'multiple' => false));
```

### Button Group (form field type)

You can now use [button groups](http://foundation.zurb.com/docs/components/button_groups.html) in your forms.

Button groups are button, grouped together by Foundation. This way they are rendered on the same line, instead of all on a different row.

This is an example of the field :
```Php
$builder->add(
    'buttons',
    'button_group',
    array(
        'label' => 'Buttons group',
        'buttons' => array(
            'back' => array(
                'type'    => 'button',
                'options' => array(
                    'label' => 'Cancel',
                    'attr' => array(
                        'class' => 'secondary',
                    ),
                ),
            ),
            'save' => array(
                'type'    => 'submit',
                'options' => array(
                    'label' => 'Submit',
                ),
            ),
        ),
        'attr' => array(
            'class' => 'right',
        ),
    )
);
```

In the `buttons` array, you define the buttons that need to be rendered. All the buttons should be of FormType `ButtonType`.
For the `ButtonType`, you cannot specify behavior in [Symfony](http://symfony.com/doc/current/reference/forms/types/button.html).
You can change the type to `reset` , to render a button with `type="reset"`, (a `ResetType`) but cannot add links.

However, you have various options:
* add an `onClick` tag to the `attr` array
* add a class `link`, a tag `data-url` to the `attr` array and let JavaScript handle it

### Button Bar (form field type)

A button bar is a group of button groups, perfect for situations where you want groups of actions that are all related to a similar element or page.

This is an (long :D) example of the field :
```Php
$builder->add(
    'button_bar',
    'button_bar',
    array(
        'button_groups' => array(
            'button_group_first' => array(
                'label' => 'Buttons group',
                'buttons' => array(
                    'one' => array(
                        'type'    => 'submit',
                        'options' => array(
                            'label' => 'one',
                        ),
                    ),
                    'two' => array(
                        'type'    => 'button',
                        'options' => array(
                            'label' => 'two',
                            'attr' => array(
                                'class' => 'success',
                            ),
                        ),
                    ),
                    'three' => array(
                        'type'    => 'button',
                        'options' => array(
                            'label' => 'three',
                            'attr' => array(
                                'class' => 'alert',
                            ),
                        ),
                    ),
                ),
                'attr' => array(
                    'class' => 'round',
                ),
            ),
            'button_group_second' => array(
                'label' => 'Buttons group',
                'buttons' => array(
                    'four' => array(
                        'type'    => 'button',
                        'options' => array(
                            'label' => 'four',
                            'attr' => array(
                                'class' => 'disabled',
                            ),
                        ),
                    ),
                    'five' => array(
                        'type'    => 'button',
                        'options' => array(
                            'label' => 'five',
                            'attr' => array(
                                'class' => 'secondary',
                            ),
                        ),
                    ),
                    'six' => array(
                        'type'    => 'button',
                        'options' => array(
                            'label' => 'six',
                            'attr' => array(
                                'class' => 'secondary',
                            ),
                        ),
                    ),
                ),
                'attr' => array(
                    'class' => 'radius',
                ),
            ),
        ),
    );
```

## Feedback

**Please provide feedback!**
We want to make this bundle useful in as many projects as possible.

Maintained by [Florian Belhomme](http://florianbelhomme.com).

## License

- This bundle is licensed under the [MIT License](http://opensource.org/licenses/MIT)
