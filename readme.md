# SCSS Banner Carousel

This SCSS mixin allows for fading and sliding carousels without needing any JS!


```
// Markup structured like this:
//  <section id="banner">
//		<div class="banner banner-home">
//			<div class="slides"></div>
//			<p>Any other content you'd like on top of the carousel</p>
//		</div>
//	</section>
//
// Call like this:
//  // params($imgName, $h, $min, $max, $m, $suffix, $count, $fade, $pos, $size, $repeat, $webp, $tiny)
//	@include bannerCarousel('banner-home.jpg', 3vw, 200px, 300px, 1.5, true, 3, true, 50% 100%, cover, no-repeat, true, false);
//
// The name of your images needs to be the same as the name of the class for which you will apply this mixin.
//  The last variable is for if you still want to use a library like TinySlider for carousels, but still want to use this mixin.
// -----------------------------------------------------------------------------
$carousel-fade: true; // true: Fade or false: slide
$animation-speed: 1s; // speed between slides
$animation-duration: 5s; // duration of slide on screen

@mixin bannerCarousel($imgName, $h:$banner-h, $min:$banner-min, $max:$banner-max, $m-x:$banner-m-x, $suffix:$banner-suffix, $count:$banner-count, $fade: $carousel-fade, $pos:$banner-pos, $size:$banner-size, $repeat:$banner-repeat, $webp:$banner-webp, $tiny:$banner-tiny) {
	@if $h !=null and type-of($h) !=number and not str-index($h, 'vw') and not str-index($h, "%") and $h !='auto' {
		@error "The value of $h:#{$h} must be in pixels, vw, or %.";
	}

	@if $min !=null and type-of($min) !=number and not str-index($min, 'vh') and not str-index($min, "%") {
		@error "The value of $min:#{$min} must be in pixels, vh, or %.";
	}

	@if $max !=null and type-of($max) !=number and not str-index($max, 'vh') and not str-index($max, "%") {
		@error "The value of $max:#{$max} must be in pixels, vh, or %.";
	}

	@if $count==null or type-of($count) !=number or $count==0 {
		@error "The value of $count:#{$count} must be greater than 0";
	}

	// Get image name and use for class name
	$index: str-index($imgName, '.');
	$class: str-slice($imgName, 1, $index - 1);

	// Animation options
	$n: $count; // For "n" images
	$n1: $count+1;
	$maxCount: $n1;
	$s: $animation-speed; // s (speed)  = animation time between slides
	$d: $animation-duration; // d (duration) = duration for slide in view
	$t: ($s + $d) * $n; // t (time) = total time of all slides and animations
	$time: math.div($s, $t) * 100%; // time = time of animation (% of total time)
	$pause: math.div($d, $t) * 100%; // pause = time of paused animation (% of total time)

	// Slide images and positions
	$index: str-index($imgName, '.');
	$imgs: null;
	$imgList: ();
	$positionList: (0);
	$output: null;

	// @debug "Class: #{$class}";
	// $class: '';

	// Slideshow images for banner
	@if $count>1 {
		// Slide images
		@for $i from 1 through $count {

			@if $suffix and $mobile {
				@if $tiny {
					$output: str-insert($imgName, "-#{$i}#{$img-suffix}", $index);

					.slide-#{$i} {
						background-image: url($root-url+$output);

						@if $webp {
							.webp & {
								$slice: str-slice($imgName, 1, $index - 1);
								$output: $slice+"-#{$i}#{$img-suffix}.webp";
								background-image: url($root-url+$output);
							}
						}
					}
				} @else {
					$output: $root-url+str-insert($imgName, "-#{$i}#{$img-suffix}", $index);

					@if $imgs {
						$imgs: $imgs + ',url('+ $output + ')';
					} @else {
						$imgs: 'url('+ $output + ')';
					}

					$imgList: append($imgList, unquote('url('+ $output + ')'), comma);
					$positionList: append($positionList, unquote(($i * 100) + 'vw'));
				}
			}

			@else {
				@if $tiny {
					$output: str-insert($imgName, "-#{$i}", $index);

					.slide-#{$i} {
						background-image: url($root-url+$output);

						@if $webp {
							.webp & {
								// $index: str-index($imgName, '.');
								$slice: str-slice($imgName, 1, $index - 1);
								$output: $slice+"-#{$i}.webp";
								background-image: url($root-url+$output);
							}
						}
					}
				} @else {
					$output: $root-url+str-insert($imgName, "-#{$i}", $index);
					@if $imgs {
						$imgs: $imgs + ',url(' + $output + ')';
					} @else {
						$imgs: 'url(' + $output + ')';
					}

					$imgList: append($imgList, unquote('url(' + $output + ')'), comma);
					$positionList: append($positionList, unquote(($i * 100) + 'vw'));
				}
			}
		}

		&.#{$class} {

			.slides {
				position: absolute;
				height: 100%;
				top: 0;
				left: 0;
				margin: 0;
				padding: 0;
				list-style: none;
				overflow: hidden;
			}

			@if $tiny {
				.slides {
					width: 100%;
				}

				.slide {
					height: inherit;
					opacity: 0;

					&.tns-slide-active {
						opacity: 1;
					}
				}

				.tns-item {
					position: absolute;
					width: 100%;
					height: 100%;
					top: 0;
					left: 0;
					z-index: 0;
					background-position: $pos;
					background-size: $size;
					background-repeat: $repeat;
				}
			} @else {
				.slides {
					@if $fade {
						width: 100%;
						background-position: $pos;
						background-size: $size;
					} @else {
						width: ($count + 1) * 100vw;

						//- Infinite scroll image & position
						$imgList: append($imgList, unquote(nth($imgList, 1)));
						$positionList: append($positionList, unquote('#{(($count + 1) * 100)}vw'));

						background-position: join($positionList, (), comma);
						background-size: 100vw;
					}

					background-image: join($imgList, (), comma);
					background-color: $color-background-dark;
					background-repeat: $repeat;

					animation: carouselSlide $t infinite;
					@if $animation-duration == 0s {
						animation-timing-function: linear;
					}
				}

				/* Sliding Carousel Animation */
				@keyframes carouselSlide {
					@for $i from 1 through length($imgList) {
						$img: nth($imgList, $i);

						@if $i == 1 {
							0%, #{$pause} {
								@if $fade {
									background-image: $img;
								} @else {
									transform: translateX(0);
								}
							}
						} @else if $i < $maxCount {
							#{($pause + $time) * ($i - 1)}, #{(($pause + $time) * ($i - 1)) + $pause} {
								@if $fade {
									background-image: $img;
								} @else {
									transform: translateX((($i - 1) * -100vw));
								}
							}
						} @else {
							#{($pause + $time) * ($i - 1)}, 100% {
								@if $fade {
									background-image: $img;
								} @else {
									transform: translateX((($i - 1) * -100vw));
								}
							}
						}
					}
				}
			}

			@if $mobile {
				height: clamp(#{$min}, #{$h}, #{$max});

				@media screen and (orientation:landscape) {
					height: $h * $m-x;
				}
			}

			@else {
				height: clamp(#{$min}, #{$h}, #{$max});

				@media all and (-ms-high-contrast: none),
				(-ms-high-contrast: active) {
					height: $h;
					min-height: $min;
					max-height: $max;
				}
			}

			//End
		}

		// Single image for banner
	}

	@else if $count==1 {
		$index: str-index($imgName, '.');
		// $selector: str-slice($imgName, 1, ($index - 1));

		&.#{$class} {

			.banner-image {
				height: clamp(#{$min}, #{$h}, #{$max});

				@if $mobile {
					@media screen and (orientation:landscape) {
						height: $h * $m-x;
					}
				}

				@else {

					@media all and (-ms-high-contrast: none),
					(-ms-high-contrast: active) {
						height: $h;
						min-height: $min;
						max-height: $max;
					}
				}

				//End
				@if $suffix and $mobile {
					$output: str-insert($imgName, "#{$img-suffix}", $index);

					background-image: url($root-url+$output);

					@if $webp {
						.webp & {
							// $index: str-index($imgName, '.');
							$slice: str-slice($imgName, 1, $index - 1);
							$output: $slice + $img-suffix + '.webp';
							background-image: url($root-url+$output);
						}
					}
				}

				@else {
					background-image: url($root-url+$imgName);

					@if $webp {
						.webp & {
							// $index: str-index($imgName, '.');
							$slice: str-slice($imgName, 1, $index - 1);
							$output: $slice + '.webp';
							background-image: url($root-url+$output);
						}
					}
				}

				background-position: $pos;
				background-size: $size;
				background-repeat: $repeat;
			}
		}
	}

	@content;
}
```

Created by: Brett Byron
Date Released: 2022
