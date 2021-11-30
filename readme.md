# SCSS Banner Carousel

This SCSS mixin allows for fading and sliding carousels without needing any JS!

Copy/Paste the file contents in your site's SCSS, and call the mixin similarly to:
```
// parameters($class, $imgH, $imgW, $h:null, $min:null, $max:null, $m-x:1.5, $imgName, $suffix:false, $count, $pos:50% 50%, $size:cover, $repeat:no-repeat, $webp:false)
@include bannerCarousel('banner--carousel', 250px, 600px, 3vw, 200px, 300px, 1.5, 'banner-carousel.jpg', true, 3, 50% 50%, cover, no-repeat, false);
```
