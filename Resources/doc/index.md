Installation
------------

### Download HshnAngularBundle using composer

```bash
$ php composer.phar require hshn/angular-bundle
```

### Enable the bundle

```php
<?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new Hshn\AngularBundle\HshnAngularBundle(),
    );
}
```

### Configure the HshnAngularBundle

```yaml
# app/config/config.yml
hshn_angular:
    template_cache:
        modules:
            # "app" will be angular module name
            app:
                # When specify true, "app" module will be created.
                create: true

                # Specify a directory that angular templates are contained.
                # It will be a base directory of this module and a template url will be relative path from the base directory.
                targets:
                    - @YourBundle/Resources/public/js
    assetic: ~
```

```yaml
# app/config/parametters.yml
parameters:
    assetic.asset_factory.class: Hshn\AngularBundle\Factory\AssetFactory
```

### Using AngularTemplateCache

In Twig:

If you configured as the sample above, the template cache of `app` module is available as `@ng_template_cache_app`.

```twig
{% javascripts
  '@ng_template_cache_app'%}
  <script src="{{ asset_url }}"></script>
{% endjavascripts %}
```

In Javascript:

If your directory structure is like this

```
@YourBundle/Resource/public/js
├── bar
│   └── a.html
└── foo
    └── b.html
```

then you can use two angular templates named `bar/a.html` and `foo/b.html` in `app` module.

```js
angular
    .module('bar', [
        'app'
    ])
    .directive('bar', function () {
        return {
            templateUrl: 'bar/a.html', // loads the template via template cache
            link: function () {}
        }
    })
```

In tests:

If you want to load the template cache without using assetic, run command `hshn:angular:template-cache:dump` and the template cache will be generated to the file system.

