---
title: Use Plausible analytics with Jekyll Chirpy theme
date: 2025-02-15 14:00:00 +01:00
categories: [plausible]
tags: [plausible, analytics, jekyll, chirpy]
---

In this post, we'll see how to install [Plausible](https://plausible.io) analytics when using the Jekyll [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) theme.

## Prerequisites

- A Plausible analytics server (self-hosted aka. Community Edition (CE) or cloud-based)
- A Jekyll website with the Chirpy theme

## Installation

### Plausible

Add a new website to your Plausible instance. You can find the documentation [here](https://plausible.io/docs/add-website).

### Jekyll

1. Add a new file `plausible.html` under `_includes/analytics`. You may need to create this folder at the root.
   Its content is as follows:

{% raw %}

```html
<!-- Plausible -->
<script
  defer
  src="{{ site.analytics.plausible.domain }}/js/script.js"
  data-domain="{{ site.analytics.plausible.id }}"
></script>
```

{% endraw %}

2. Update your configuration file (`_config.yml`) with the following:

```yaml
analytics:
  plausible:
    id: <placeholder> # The domain name used in Plausible
    domain: <placeholder> # Your Plausible instance URL eg. https://plausible.example.com or https://plausible.io
```
