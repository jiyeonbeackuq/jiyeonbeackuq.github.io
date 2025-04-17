---
layout: post
title:  "Post With A Code Snippet"
date:   2014-12-13
---

# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6


## Unordered List
* List Item
* Longer List Item
  * Nested List Item
  * Nested Item
* List Item

## Ordered List
1. List Item
2. Longer List Item
    1. Nested OL Item
    2. Another Nested Item
3. List Item

## Definition List
<dl>
  <dt>Coffee</dt>
  <dd>Black hot drink</dd>
  <dt>Milk</dt>
  <dd>White cold drink</dd>
</dl>

## Table

| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |
| Header      | Title       |
| Paragraph   | Text        |



<figure>
	<img src="/assets/img/touring.jpg" alt=""> 
	<figcaption>Fig1. - This is an example figcaption</figcaption>
</figure>

{%- highlight html -%}
<figure>
	<img src="/assets/img/touring.jpg" alt=""> 
	<figcaption>Fig1. - This is an example figcaption</figcaption>
</figure>
{%- endhighlight -%}


<p class="intro"><span class="dropcap">Y</span>ou'll find this post in your `_posts` directory - edit this post and re-build (or run with the `-w` switch) to see your changes! To add new posts, simply add a file in the `_posts` directory that follows the convention: YYYY-MM-DD-name-of-post.ext.</p>

Jekyll also offers powerful support for code snippets:

{%- highlight ruby -%}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{%- endhighlight -%}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh].

[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com
