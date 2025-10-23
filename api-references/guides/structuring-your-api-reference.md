---
description: >-
  Learn how to structure your API reference across multiple pages with icons and
  descriptions.
---

# Structuring your API reference

GitBook does more than just render your OpenAPI spec. It lets you customize your API reference for better clarity, navigation, and branding.

### Split operations across multiple pages

To keep your documentation organized, GitBook can split your API operations into separate pages. Each page is generated from a tag in your OpenAPI spec. To group operations into a page, assign the same tag to each operation:

<pre class="language-yaml" data-title="openapi.yaml"><code class="lang-yaml">paths:
  /pet:
    put:
<strong>      tags:
</strong><strong>        - pet
</strong>      summary: Update an existing pet.
      description: Update an existing pet by Id.
      operationId: updatePet
</code></pre>

### Reorder pages in your table of contents

The order of pages in GitBook matches the order of tags in your OpenAPI tags array:

<pre class="language-yaml" data-title="openapi.yaml"><code class="lang-yaml">tags:
<strong>  - name: pet
</strong><strong>  - name: store
</strong><strong>  - name: user
</strong></code></pre>

### Nest pages into groups

To build multi-level navigation, use `x-parent` (or `parent`) in tags to define hierarchy:

<pre class="language-yaml" data-title="openapi.yaml"><code class="lang-yaml">tags:
  - name: everything
  - name: pet
<strong>    x-parent: everything
</strong>  - name: store
<strong>    x-parent: everything
</strong></code></pre>

The above example will create a table of contents that looks like:

```
Everything
├── Pet
└── Store
```

If a page has no description, GitBook will automatically show a card-based layout for its sub-pages.

### Customize page titles, icons, and descriptions

You can enhance pages with titles, icons, and descriptions using custom extensions in the tags section. All [Font Awesome icons](https://fontawesome.com/search) are supported via `x-page-icon`.

{% code title="openapi.yaml" %}
```yaml
tags:
  - name: pet
    # Page title displayed in table of contents and page
    -x-page-title: Pet
    # Icon shown in table of contents and next to page title
    -x-page-icon: dog
    # Description shown just above the title
    -x-page-description: Pets are amazing!
    # Content of the page
    description: Everything about your Pets
```
{% endcode %}

### Build rich descriptions with GitBook Blocks

Tag description fields support GitBook markdown, including [advanced blocks](../../creating-content/blocks/) like tabs:

{% code title="openapi.yaml" %}
```
  {% tabs %}
  {% tab title="Dog" %}
  Here are the dogs
  {% endtab %}

  {% tab title="Cat" %}
  Here are the cats
  {% endtab %}

  {% tab title="Rabbit" %}
  Here are the rabbits
  {% endtab %}
  {% endtabs %}
```
{% endcode %}

### Document a webhook endpoint

GitBook also supports documenting webhook endpoints when using OpenAPI 3.1.

You can define your webhooks directly in your OpenAPI file using the `webhooks` field, which works similarly to `paths` for regular API endpoints:

{% code title="openapi.yaml" %}
```yaml
---
openapi: 3.1.0 # Webhooks are available starting from OpenAPI 3.1

webhooks:
  newPet:
    post:
      summary: New pet event
      description: Information about a new pet in the system
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Pet"
      responses:
        "200":
          description: Return a 200 status to indicate that the data was received successfully
```
{% endcode %}
