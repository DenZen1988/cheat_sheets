# Jinja2 Cheat Sheet

## Table of content

* [Whitespaces](#whitespaces)
  * [Default Behavior](#default-behavior)
  * [Strip Whitespaces Before](#strip-whitespaces-before)
  * [Strip Whitespaces After](#strip-whitespaces-after)
  * [Preserve a Whitespace Before](#preserve-a-whitespace-before)
  * [Preserve a Whitespace After](#preserve-a-whitespace-after)
  * [Combine Stripping and Preserving](#combine-stripping-and-preserving)
* [Variables](#variables)
  * [Combine Variables](#combine-variables)
  * [Appending Variables](#appending-variables)

## Whitespaces

### Default behavior

The default behavior of jinja2 works like this:

* Single trailing newlines are stripped if present
* All other whitespaces (spaces, tabs, newlines, etc.) will be returned unchanged in the rendered result

### Strip whitespaces before

If you want to strip the whitespaces before the rendered result you can use a '-' within the condition:

```jinja2
{%- if foo == 'bar' %}
...
{% endif %}
```

### Strip whitespaces after

If you want to get rid of whitespaces at the end of a rendered result simply append the '-' at the end of the if clause:

```jinja2
{% if foo == 'bar' -%}
...
{% endif %}
```

### Strip whitespaces before AND after

You can also combine both of the above examples to get rid of whitespaces before and after a result:

```jinja2
{%- if foo == 'bar' -%}
...
{% endif %}
```

### Preserve a whitespace before

If you need to preserve a whitespace before the rendered result you can use a '+' in the if clause:

```jinja2
{%+ if foo == 'bar' %}
...
{% endif %}
```

### Preserve a whitespace after

If you need to preserve a whitespace after a rendered result you can append it at the end of the if clause:

```jinja2
{% if foo == 'bar' +%}
...
{% endif %}
```

### Combine stripping and preserving

Of course you can also combine the stripping and the preserving of whitespaces:

```jinja2
{%+ if foo == 'bar' -%}
...
{% endif %}
```

Or the other way around:

```jinja2
{%- if foo == 'bar' +%}
...
{% endif %}
```

## Variables

### Define Variables

To define variables within a jinja2 template you can use 'set' like this:

```jinja2
{% set foo = value_of_foo %}
```

And then use it somewhere else in the template:

```jinja2
{% for f in foo %}
...
{% endfor %}
```

### Combine Variables

You can also combine variables. Let's say you have some key-value variables like this:

```yaml
countries:
    north_america:
        - 'United States of America"
        - 'Canada'
    europe:
        - 'Germany'
        - 'Austria'
        - 'Belgium'
    asia:
        - 'China'
        - 'South Korea'
        - 'Japan'
    oceania:
        - 'New Zealand'
        - 'Australia'
```

Now you need to fetch all countries at once within a jinja2 template. Simply combine the variables like this:

```jinja2
{% for country in countries['north_america'] + countries['europe'] + countries['asia'] + countries['oceania'] %}
echo {{ country }}
{% endfor %}
```

Here is an example of combining them as a new variable first:

```jinja2
{% set countries_combined = countries['north_america'] + countries['europe'] + countries['asia'] + countries['oceania'] %}
{% for country in countries_combined %}
echo {{ country }}
{% endfor %}
```

### Appending Variables

If you have two single variables liek this:

```yaml
foo: 'value_of_foo'
bar: 'value_of_bar'
```

Then you can append them together like this:

```jinja2
{{ foo }} {{ bar }}
```

You can also append a single variable after a loop, just make sure to append it after the `{% endfor %}`:

```jinja2
{% set countries_combined = countries['north_america'] + countries['europe'] + countries['asia'] + countries['oceania'] %}
{% for country in countries_combined %}
echo {{ country }}
{% endfor %} {{ foo }}
```
