up:: [[Open Graph Protocol]]
tags:: #source/article #note/reference 
Source:: [[freecodecamp.org]]

[![freeCodeCamp.org](https://cdn.freecodecamp.org/platform/universal/fcc_primary.svg)](https://www.freecodecamp.org/news)
# What is Open Graph and how can I use it for my website?


It can take a lot of time to build content and maintain a website. How can we make sure our content stands out when getting shared on social feeds around the internet?

- [What is Open Graph?](#what-is-open-graph)
- [Why do I need it?](#why-do-i-need-it)
- [What happens if I don’t have it?](#what-happens-if-i-don-t-have-it)
- [Starting with the basics of open graph](#starting-with-the-basics-of-open-graph)
- [Website open graph type](#website-open-graph-type)
- [Some other open graph tags that are worth adding](#some-other-open-graph-tags-that-are-worth-adding)
- [Twitter and other social media networks using open graph](#twitter-and-other-social-media-networks-using-open-graph)
- [Images in open graph](#images-in-open-graph)
- [Testing your open graph tags](#testing-your-open-graph-tags)
- [Can I get an example?](#can-i-get-an-example)

## What is Open Graph?

[Open Graph](https://ogp.me/) is an internet protocol that was originally created by [Facebook](http://fbdevwiki.com/wiki/Open_Graph_protocol) to standardize the use of metadata within a webpage to represent the content of a page.

Within it, you can provide details as simple as the title of a page or as specific as the duration of a video. These pieces all fit together to form a representation of each individual page of the internet.

## Why do I need it?

Content on the internet is typically created with at least one goal in mind -- to share it with others. This might not necessarily matter if you’re just sending it to one friend, but if you want to share it or want it to be shared on any social network or app that utilizes rich previews, you’ll want that preview to be as effective as possible.

This will help encourage people to check out your content and inevitably click through to your content.

## What happens if I don’t have it?

Most social networks by default will try to make their best effort in creating a preview of your content. This more often than not doesn’t go so well.

Take for instance my website [colbyfayock.com](https://colbyfayock.com).

![simple-twitter-card](https://www.freecodecamp.org/news/content/images/2020/03/simple-twitter-card.jpg)

Example of a simple Twitter Card

It correctly grabs the title of my page and the description, but it's not the most enticing looking tweet in a feed.

Contrast that to the preview of a single post and we see a different story.

![large-image-twitter-card](https://www.freecodecamp.org/news/content/images/2020/03/large-image-twitter-card.jpg)

Example of a Twitter Card with a large image

So what happens if you don’t have open graph tags? Nothing bad will happen, but you won’t be taking advantage of some of the features that help make your content stand out next to the loads of other content getting posted on the internet.

## Starting with the basics of open graph

The four basic open graph tags that are required for each page are `og:title`, `og:type`, `og:image`, and `og:url`. These tags should be unique for each page you serve, meaning your homepage’s tags should all be different from your blog post article’s page.

![open-graph-twitter-card](https://www.freecodecamp.org/news/content/images/2020/03/open-graph-twitter-card.jpg)

Anatomy of a Twitter Card using Open Graph tags

While it should be pretty straightforward, here’s a breakdown of what each of the tags mean:

- `og:title`: The title of your page. This is typically the same as your webpage's `<title>` tag unless you’d like to present it differently.
- `og:type`: The “type” of website you have. I’ll explain more in the next section, though a generic “type” is “website”.
- `og:image`: This should be a link to an image that you’d like to represent your content. It should be a high resolution image that the social networks will use in their feeds.
- `og:url`: This should be the URL of the current page.

When placing a tag on your website, you should place it in the `<head>` along with any other metadata. The tag used will be a `<meta>` tag and should look like this pattern:

```
<meta property=“[NAME]” content=“[VALUE]” />
```

So if I were to create a set four basic open graph tags for my website, [colbyfayock.com](https://colbyfayock.com), it might look like:

```html
<meta property="og:title" content="Colby Fayock - A UX Designer &amp; Front-end Developer Blog" />
<meta property="og:type" content="website" />
<meta property="og:image" content="/static/website-social-card-44070c4a901df708aa1563ac4bbe595a.jpg" />
<meta property="og:url" content="https://www.colbyfayock.com" />
```

## Website open graph type

The open graph protocol has a few variations of the “type” of website it supports. This includes types like website, article, or video.

When setting up your open graph tags, you’ll want to have an idea of which type will make more sense for your website. If your page is focused on a single video, it probably makes sense to use the type “video”. If it’s a general website with no specific vertical, you would probably just want to use the type “website”.

Similar to the others, this is unique for each page. So if your homepage is "website,” you could always have another page of type “video”.

So if I were to create an open graph type for my website, it might look like the following on my homepage:

```html
<!-- colbyfayock.com -->
<meta property=“og:type” content=“profile” />
```

When navigating to a blog post, it would look like:

```html
<!-- https://www.colbyfayock.com/2020/03/anyone-can-map-inspiration-and-an-introduction-to-the-world-of-mapping/ -->
<meta property=“og:type” content=“article” />
```

You can find the most common open graph website types on the open graph webpage: [https://ogp.me/#types](https://ogp.me/#types)

## Some other open graph tags that are worth adding

Though you’ll generally be okay with the basics, here are a few more that would be worth adding:

- `og:description`: A description of your page. Similarly to `og:title`, this may be the same as your website’s `<meta type=“description”>` tag, unless you’d like to present it differently.
- `og:locale`: If you want to localize your tags, it would probably make sense to include locale. The format is `language_TERRITORY`, where the default is `en_US`.
- `og:site_name`: The name of the overall website your content is on. If you're on a blog post page, you might have a `title` using that blog post’s title, where the `site_name` would be the name of your blog.
- `og:video`: Have a video that supports your content? Here’s a chance to include it. Add a link to your video using this tag.

These tags will be added in the same pattern as before:

```html
<meta property=“[NAME]” content=“[VALUE]” />
```

## Twitter and other social media networks using open graph

Most of the social networks adhere to the basics of open graph standards, but a few of them also include their own extension to help customize the look and feel within their ecosystem.

Twitter for instance, allows you to specify `twitter:card`, which is the type of “card” you can use when they show your website. At this time, their card types include:

- summary
- summary_large_image
- app
- player

This will help you choose how your links are used in their feed. If you choose `summary_large_image` for instance, Twitter will show your links with big high resolution images as long as you’re providing it in the in the `og:image` tag.

Here are some quick references to the documentation of how to use open graph tags with some of the social media sites:

- Twitter: [https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started)
- Facebook: [https://developers.facebook.com/docs/sharing/webmasters/](https://developers.facebook.com/docs/sharing/webmasters/)
- Pinterest: [https://developers.pinterest.com/docs/rich-pins/overview/](https://developers.pinterest.com/docs/rich-pins/overview/)?
- LinkedIn: [https://www.linkedin.com/help/linkedin/answer/46687/making-your-website-shareable-on-linkedin?lang=en](https://www.linkedin.com/help/linkedin/answer/46687/making-your-website-shareable-on-linkedin?lang=en)

## Images in open graph

While adding your image as `og:image` should often be enough, sometimes it can be challenging to get your image to show up correctly. If you seem to be running into trouble, the open graph standard includes a few image tags such as `og:image:url` vs `og:image:secure_url` as well as the `og:image:width` and `og:image:height`.

Try to make sure you’re following all of the [notes and examples in the open graph documentation](https://ogp.me/#structured). Additionally, some of the social networks have image requirements. [Twitter for instance requires](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/overview/summary-card-with-large-image) a ratio of 2:1 with a minimum size of 300x157 and a maximum size of 4096x4096 that’s under 5MB and of JPG, PNG, WEBP or GIF format.

If you’re stuck, test your tags using the social media network’s tools to see if you can find the issue.

## Testing your open graph tags

Luckily, our favorite social networks also provide tools to help us debug our tags. Once you make sure that your tags are actually showing up in the source code of your website, you’ll be able to preview how your website will look in the feed.

- Twitter: [https://cards-dev.twitter.com/validator](https://cards-dev.twitter.com/validator)
- Facebook: [https://developers.facebook.com/tools/debug/](https://developers.facebook.com/tools/debug/)
- Pinterest: [https://developers.pinterest.com/tools/url-debugger/](https://developers.pinterest.com/tools/url-debugger/)

## Digging further into open graph tags

While most of these should cover a basic website, there are a few more tags that might help you and your business’s discoverability throughout social networks.

If you’re interested in diving in more, [the documentation](https://ogp.me/) does a great job at providing a list of all of the available tags for you to use.

[https://ogp.me/](https://ogp.me/)

## Can I get an example?

If you’re simply looking for an example to get started, here’s what you should end up with when setting up your tags for [a blog post](https://www.colbyfayock.com/2020/03/anyone-can-map-inspiration-and-an-introduction-to-the-world-of-mapping/):

```html
<meta property="og:site_name" content="Colby Fayock" />
<meta property=“og:title” content=“Anyone Can Map! Inspiration and an introduction to the world of mapping - Colby Fayock" />
<meta property="og:description" content="Chef Gusteau was a visionary who created food experiences for the world to enjoy. How can we take his lessons and apply them to the world of…" />
<meta property="og:url" content="https://www.colbyfayock.com/2020/03/anyone-can-map-inspiration-and-an-introduction-to-the-world-of-mapping/" />
<meta property="og:type" content="article" />
<meta property="article:publisher" content="https://www.colbyfayock.com" />
<meta property="article:section" content="Coding" />
<meta property="article:tag" content="Coding" />
<meta property="og:image" content="https://res.cloudinary.com/fay/image/upload/w_1280,h_640,c_fill,q_auto,f_auto/w_860,c_fit,co_rgb:232129,g_west,x_80,y_-60,l_text:Source%20Sans%20Pro_70_line_spacing_-10_semibold:Anyone%20Can%20Map!%20Inspiration%20and%20an%20introduction%20to%20the%20world%20of%20mapping/blog-social-card-1.1" />
<meta property="og:image:secure_url" content="https://res.cloudinary.com/fay/image/upload/w_1280,h_640,c_fill,q_auto,f_auto/w_860,c_fit,co_rgb:232129,g_west,x_80,y_-60,l_text:Source%20Sans%20Pro_70_line_spacing_-10_semibold:Anyone%20Can%20Map!%20Inspiration%20and%20an%20introduction%20to%20the%20world%20of%20mapping/blog-social-card-1.1" />
<meta property="og:image:width" content="1280" />
<meta property="og:image:height" content="640" />
<meta property="twitter:card" content="summary_large_image" />
<meta property="twitter:image" content="https://res.cloudinary.com/fay/image/upload/w_1280,h_640,c_fill,q_auto,f_auto/w_860,c_fit,co_rgb:232129,g_west,x_80,y_-60,l_text:Source%20Sans%20Pro_70_line_spacing_-10_semibold:Anyone%20Can%20Map!%20Inspiration%20and%20an%20introduction%20to%20the%20world%20of%20mapping/blog-social-card-1.1" />
<meta property="twitter:site" content="@colbyfayock" />
```

