# SCSS Banner Carousel

This SCSS mixin allows for fading and sliding carousels without needing any JS! It is currently using the `sass:meta`, `:math`, and `:list` Built-In Modules.

## Markup structure
```
<section id="banner">
	<div class="banner banner-home">
		<div class="slides"></div>
		<p>Any other content you'd like on top of the carousel</p>
	</div>
</section>
```

## How to use

### 1. Include the mixin in your project files and reference it in your main `.scss` file.
`@import "./path to file/bannerCarousel.scss";`

##### Or if you're using SASS's meta `@use "sass:meta";`
`@include meta.load-css("./path to file/bannerCarousel.scss");`

### 2. Reference the mixin
The parameters used for this can be a variable you define to use globally, or can just be passed in the mixin directly to use case by case. 

The parameters are in the following order:
```
$imgName: 	Image Name ( required for each instance )
			This filename needs to be the name of the class you will apply to your carousel.
				e.g., images for the class .banner-home will be named: 
				'banner-home-1.jpg', 'banner-home-2.jpg', 'banner-home-3.jpg', etc.
$h:		Image's ideal height ( required )
$min:		Image's minimum allowed height ( required )
$max:		Image's maximum allowed height ( required )
$m-x:		A ratio value used for mobile screens' fixed height based on the desktop's minimum height
$suffix:	A suffix value to put on the images (e.g., "@0.5x" would result in "Image Name@0.5x.jpg")
$count:		The number of slides in your carousel. ( must be at least 1 )
$fade:		Determines the animation style to be fading or sliding. ( true = fading, false = sliding )
$pos:		Background position of slide images
$size:		Background size of slide images
$repeat:	Background image reapeat of slide images
$webp:		Set to true if you're using webp
$tiny:		This allows you to use this mixin in addition to the [TinySlider library](https://github.com/ganlanyuan/tiny-slider)
```

`@include bannerCarousel('banner-home.jpg', 3vw, 200px, 300px, 1.5, true, 3, true, 50% 100%, cover, no-repeat, true, false);`

### 3. Change animation variables
In the `bannerCarousel.scss` be sure to change the animation variables to your preferences:

```
$mobile: false; // a variable to determine how to compile the code for desktop or mobile sites
$carousel-fade: true; // true: Fade or false: slide
$animation-speed: 1s; // speed between slides
$animation-duration: 5s; // duration of slide on screen
```
\
\
\
\
\
Created by: Brett Byron  
Date Released: 2022
