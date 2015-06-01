#How to install zamtools-navigation.

# Installation #

Add zamtools-navigation to your project directory or put it somewhere on the PYTHONPATH.

In your settings.py file, add zamtools-navigation to the INSTALLED\_APPS.

```
INSTALLED_APPS = (
   'zamtools-navigation',
)
```

Add the top\_level and current context processors to your TEMPLATE\_CONTEXT\_PROCESSORS.

```
TEMPLATE_CONTEXT_PROCESSORS = (
    'zamtools-navigation.context_processors.top_level',
    'zamtools-navigation.context_processors.current',
)
```

Synchronize your database.

```
> python manage.py syncdb
```

# Usage #

Log in to the Admin panel and create some locations.

It is usually a good idea to surround the `base_url` and `target_url` with slashes (eg: `/about/`) to prevent any ambiguity when urls are similar (eg: `/about` and `/blog/about-last-night`).

Locations that don't specify a parent are considered to be "top level" locations.

**NOTE:** It is **not** recommended that you create a root location, this will break top level functionality since that root would be the only top level location.

By default, the order the locations are displayed in are the order they were added.  However, order can be more tightly controlled by using the `order` field.  Locations are sorted in ascending order, with lower values being listed first.

Hidden locations are ignored by all Location model methods (`top_level`, `root`, `children`) and context processors (`top_level`, `current`).  The purpose of hidden locations is to act as structure in the location hierarchy, but avoid being tracked as the current location.

Examples of hidden locations are `/login/` and `/logout/` pages that belong to an `/accounts/` parent.  The `login` and `logout` locations can be set to hidden, causing the current location detection to fall back on the `accounts` parent.

# Context Variables #

The zamtools-navigation context processors add `top_locations` and `current_location` variables to the context.

`top_locations` is a list of all locations without parents that aren't hidden.

`current_location` is the location that most closely matches the current url.  If no match is found, its value is `None`.

# Testing #

Tests can be performed using the standard Django test command.

```
> python manage.py test zamtools-navigation
```