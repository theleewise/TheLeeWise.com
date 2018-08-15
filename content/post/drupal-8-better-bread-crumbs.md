---
title: "Better Bread Crumbs in Drupal 8"
subtitle: "hey hey"
date: 2018-04-22T23:11:59-05:00
draft: false
image: "assets/imgs/posts-main/pexels-photo-160107.jpeg"
tags: ["Drupal","Tutorial"]
description: "Include the current page node in the bread crumbs but only if it is a page"
---

In Drupal 8, the default breadcrumbs will include a full path to the current page but won't include the current page itself. Now I found a solution online that was easy enough but I had a few issues to complicate it a little.

1. I really only want this for nodes that are pages and not say events or news. Mostly because these can often times have much longer titles and would break the design to have this in the breadcrumbs
2. I want to be able to add the necessary accessability code for this current item & any additional style classes.

To begin, we'll add a preprocess function. It will check if the current node is the `page` content type. It will also add a new property `active` to the item and then added to the bread crumb array.

`mytheme.theme`
{{< highlight php >}}
<?php
function THEME_preprocess_breadcrumb(&$variables){
	if(($node = \Drupal::routeMatch()->getParameter('node')) && $variables['breadcrumb']){
		if($node->getType() === 'page'){
			// Adding the title of the page in the breadcrumb
			$variables['breadcrumb'][] = array(
				'text' => $node->getTitle(),
				'url' => null,
        'active' => true
			);
		}
	}
}
?>
{{< /highlight >}}

Finally we add a few lines to the breadcrumb twig template. I needed a class and `aria-current="page"` on the link for the current page. This is done with a simple conditional against the new property we passed `active`.

`breadcrumb.html.twig`
{{< highlight twig >}}
{% if breadcrumb %}
  <nav role="navigation" aria-labelledby="system-breadcrumb">
    <h2 id="system-breadcrumb" class="visually-hidden">{{ 'Breadcrumb'|t }}</h2>
    <ol class="breadcrumbs__list">
    {% for item in breadcrumb %}
      <li>
        {% if item.url %}
          <a class="{{ (item.active ? 'active') }}" href="{{ item.url }}"
            {% if item.active %}aria-current="page"{% endif %}
          >{{ item.text }}</a>
        {% else %}
          <span>{{ item.text }}</span>

        {% endif %}
      </li>
    {% endfor %}
    </ol>
  </nav>
{% endif %}
{{< /highlight >}}

And that's all there is to it.