# Jinja2 Cheat Sheet

## Table of content

* [Conditions & Loops](#conditions--loops)
  * [If, Else & Elif](#if-else--elif)
  * [Loops](#loops)
  * [Combine Loops & Conditions](#combine-loops--conditions)
* [Joining & Sorting](#joining--sorting)
  * [Joining](#joining)
  * [Sorting](#sorting)
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

## Conditions & Loops

### If, Else & Elif

In Jinja2 templates it is possible to use standard if-else statements like this:

```jinja2
{% if weather == 'rain' %}
WEATHER='sucks'
{% else %}
WEATHER='fine'
{% endif %}
```

If there are more than two conditions the condition can be expanded with `elif` like this:

```jinja2
{%if weather == 'rain' %}
WEATHER='wet'
{% elif weather == 'snow' %}
WEATHER='cold'
{% weather == 'sunny' %}
WEATHER='warm'
{% else %}
WEATHER='unknown'
{% endif %}
```

### Loops

If you have an array like this:

```yaml
apt_packages:
  - vim
  - nginx
  - mysql
```

And you want to loop over it, you can realize it with a for-loop like this:

```jinja2
{%for package in apt_packages %}
echo {{ package }}
{% endfor %}
```

### Combine Loops & Conditions

You can also combine conditions and loops like this:

```jinja2
{% if ansible_facts['os_family'] == 'Debian' %}
  {% for deb_package in apt_packages %}
    echo "Debian Package: {{ deb_package }}"
  {% else %}
    {% for other_package in other_packages %}
        echo "Other Package: {{ other_package }}"
  {% endfor %}
{% endif %}
```

## Joining & Sorting

### Joining

Sometimes you need to fetch multiple values from, for example `ansible_facts`. Usually they are not really joined together and are fetched
as an array. To be able to use these values all together it makes sense to `join` them together like this:

```jinja2
NETWORK="{{ ansible_facts.interfaces | join(' ') }}"
```

### Sorting

In case you want to fetch a list of values (for example network interfaces) and you want to avoid any "drift" (random re-sorting of the output
occasionally) it makes sense to sort the output.

The following code will cause a re-order of the interfaces from time to time:

```jinja2
NETWORK="{{ ansible_facts.interfaces | join(' ') }}"
```

If you `sort` the output, the output will always be forced to be the same:

```jinja2
NETWORK="{{ ansible_facts.interfaces | sort | join(' ') }}"
```

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

If you have two single variables like this:

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

## Using Raw so characters will not be interpreted by Jinja2

Sometimes you will find edge cases by jinja interpreting characters but you do not want it to. Let's say you have a string like this:

```text
FOO=${#BAR[@]}
```

Jinja2 will now interpret this part `{#` as the start of a comment and will complain that there is no ending for that comment.

To counter that you can put the whole line into a `raw` tag like this:

```jinja2
{% raw %}
FOO=${#BAR[@]}
{% endraw %}
```

Now the line will be placed like it should be placed.
