<script lang="ts">

	import { animate, stagger } from "animejs";
	import { onMount } from "svelte";
	import { loadPagePromise } from "$lib/store";
	import { loadImage } from "$lib/utils";
    import { scrollAnchorState, viewPortState } from "$lib/state.svelte";

	// DOM Node Binds for animations
	let homeContainerElement: HTMLElement = $state()!; // Container
	let backgroundContainerElement: HTMLElement = $state()!;
	let backgroundImageElement: HTMLElement = $state()!; // Offsets

	// Elements for animations
	let titleWord1Element: HTMLElement = $state()!; 
	let titleWord2Element: HTMLElement = $state()!; 
	let shortDetailsElement: HTMLElement = $state()!; 
	let callToActionElement: HTMLElement = $state()!;

	// SVG Paths
	let signaturePath1: SVGPathElement = $state()!; 
	let signaturePath2: SVGPathElement = $state()!;
	let signaturePath3: SVGPathElement = $state()!; 
	let signaturePath4: SVGPathElement = $state()!;

	onMount(async () => {
		await loadPagePromise;
		// Set navbar home link's y location to top of homeContainer
		scrollAnchorState.home = homeContainerElement;

		// Add parallax scrolling offsets to slickScroll
		viewPortState.slickscrollInstance!.addOffset({
			element: backgroundContainerElement,
			speedY: 0.8
		});

		introAnimations();
	})


	// Page load animations
	function introAnimations() {

		const animation = [{ strokeDashoffset: '0' }];

		// Signature animation using svg stroke DashOffset
		signaturePath1.animate(animation, {
			duration: 1000,
			delay: 500,
			easing: 'cubic-bezier(.72,.3,.25,1)',
			fill: 'forwards' 
		});
		signaturePath2.animate(animation, {
			duration: 300,
			delay: 1500,
			easing: 'cubic-bezier(.47,.41,.26,1)',
			fill: 'forwards' 
		});
		signaturePath3.animate(animation, {
			duration: 200,
			delay: 1800,
			easing: 'cubic-bezier(.47,.41,.26,1)',
			fill: 'forwards' 
		});
		signaturePath4.animate(animation, {
			duration: 1000,
			delay: 2000,
			easing: 'cubic-bezier(.47,.41,.26,1)',
			fill: 'forwards' 
		});


		// Animate background image
		Object.assign(backgroundContainerElement.style, {
			height: "0",
			transform: "scale(1.3)",
		});
		backgroundImageElement.style.transform = "translateY(80%) scale(1.4)";

		animate(backgroundContainerElement, {
			height: "100%",
			scale: 1,
			easing: "cubicBezier(0.165, 0.84, 0.44, 1)",
			duration: 1500,
			delay: 500,
			complete: () => {
				backgroundContainerElement.style.boxShadow = "3px 9px 18px rgba(0, 0, 0, 0.2)";
			}
		});

		animate(backgroundImageElement, {
			translateY: "0",
			scale: 1,
			easing: "cubicBezier(0.165, 0.84, 0.44, 1)",
			duration: 1500,
			delay: 500
		});


		// Animate title elements
		const titleElements = [titleWord1Element, titleWord2Element, shortDetailsElement, callToActionElement];
		titleElements.forEach(e => {
			e.style.transform = "translateY(130%) rotate(10deg)";
		})
		animate(titleElements, {
			rotate: "0",
			translateY: "0%",
			easing: "cubicBezier(0.165, 0.84, 0.44, 1)",
			duration: 900,
			delay: stagger(80, {start: 500})
		});
	}

</script>



<div id="content-container" style="padding-top: 23vh" bind:this={homeContainerElement}>
	<div class="content-wrapper">
		<div class="flex">
			<div class="flex-wrapper first">

				<svg id="signature" class="h-signature" x="0px" y="0px" viewBox="0 0 220 140">
					<g>
						<path
							bind:this={signaturePath1}
							class="path-1"
							style="fill:none;stroke:#ffffff;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:round;stroke-opacity:1;"
							d="M 32 98 C 22 75 25 35 48 20 C 65 9 82 22 84 52 C 86 78 72 108 52 106 C 36 104 38 78 58 66 C 78 54 98 56 108 58"/>
						<path
							bind:this={signaturePath2}
							class="path-2"
							style="fill:none;stroke:#ffffff;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:round;stroke-opacity:1;"
							d="M 104 68 C 96 55 106 42 116 52 C 122 58 123 68 122 76 C 121 50 123 24 130 18 C 135 14 138 25 136 48 C 134 70 133 80 138 78"/>
						<path
							bind:this={signaturePath3}
							class="path-3"
							style="fill:none;stroke:#ffffff;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:round;stroke-opacity:1;"
							d="M 138 78 C 141 55 146 36 150 40 C 153 45 148 68 152 74 C 156 80 162 62 166 65 C 170 68 168 76 172 74 C 176 72 181 60 185 64 C 188 67 186 74 190 70 C 194 66 199 62 204 65 C 208 68 207 75 212 72"/>
						<path
							bind:this={signaturePath4}
							class="path-4"
							style="fill:none;stroke:#ffffff;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:round;stroke-opacity:1;"
							d="M 40 120 C 75 116 130 114 185 104 C 205 100 200 112 175 120 C 145 130 110 132 85 130"/>
					</g>
				</svg>

			</div>
			
			<div class="flex-wrapper second">
				<h1 class = "title">
					<div class="title-mask">
						<div class="word" bind:this={titleWord1Element}>Adham</div>
					</div><br> 
					<div class="title-mask">
						<div class="word" bind:this={titleWord2Element}>Hossam</div>
					</div>
				</h1>
				<div class="occupation mask">
					<p class = "paragraph" bind:this={shortDetailsElement}>
						software developer & cs student from monufia, egypt
					</p>
				</div>
				<div class="wrapper action-mask">
					<div class="action" bind:this={callToActionElement}>
						<div class="mask">
							{#await loadImage("assets/imgs/scroll_arrow.png") then src}
								<img src="{src}" alt="">
							{/await}
						</div>
						<div>
							scroll
						</div>
					</div>
				</div>
			</div>

			<div class="parallax-wrapper home-back" bind:this={backgroundContainerElement}>
				{#await loadImage("assets/imgs/home-back.jpg") then src}
					<img src="{src}" bind:this={backgroundImageElement} draggable="false" alt="Home Background" style="width:100%; height: 100%; object-fit: cover;">
				{/await}
			</div>
		</div>
	</div>
</div>



<style lang="sass">

@use "../consts" as consts
@include consts.textStyles()

#content-container
	height: 100vh
	width: 100vw
	padding: 12vh 7vw
	box-sizing: border-box
	position: relative

	.content-wrapper
		position: relative
		height: 100%
		box-sizing: border-box
		z-index: 2

	.flex
		z-index: 2
		width: 95%
		height: 100%
		display: flex
		flex-direction: row
		justify-content: space-between
		position: relative
		box-sizing: border-box

		.flex-wrapper
			position: relative
			height: 100%
			display: flex
			flex-direction: column
			justify-content: center

			&.second
				margin-right: 5vw 
				justify-content: flex-end

			h1
				font-weight: 400
				text-transform: none !important
				text-shadow: 0px 5px 10px rgba(0, 0, 0, 0.3)

				*
					text-transform: none !important

			.title-mask
				overflow: hidden
				display: inline-flex

			.mask
				overflow: hidden

			.h-signature
				width: 35vh
				margin-left: -6vh

			.occupation
				position: relative
				margin-top: 8vh

			.action-mask
				margin-top: 10vh
				margin-right: 7vw
				display: inline-flex
				overflow: hidden

				.action
					font-size: 2vh
					letter-spacing: 0.5vh
					font-family: consts.$font
					text-transform: uppercase
					color: white
					position: relative
					display: inline-flex
					flex-direction: row
					align-items: center

					.mask
						overflow: hidden
						height: 2vh

						img
							height: 2.3vh
							margin-right: 1.5vh
							animation: scrollArrowLoop 3s ease infinite

	.parallax-wrapper
		position: absolute
		left: 0
		z-index: -1
		width: 80%
		height: 100%
		margin-left: 5%
		border-radius: 1.5vh
		overflow: hidden
		box-sizing: border-box
		-webkit-touch-callout: none
		-webkit-user-select: none
		-moz-user-select: none
		-ms-user-select: none
		user-select: none
		transition: box-shadow 0.6s ease
		-webkit-transition: box-shadow 0.6s ease

		@media only screen and (max-width: 1250px)
			&
				opacity: 0.7

		@media only screen and (max-width: 750px)
			&
				opacity: 0.3

		img
			height: 100%
			width: 100%
			object-fit: cover
			border-radius: 1.5vh

@media only screen and (min-width: 1250px)
	.h-signature
		display: block

	.occupation
		width: 100%

	#content-container .flex *
		text-align: left

@media only screen and (max-width: 1250px)
	#content-container .flex *
		text-align: left

	.flex
		justify-content: center !important
		width: 100% !important

		.flex-wrapper 
			&.first
				display: none !important

			&.second
				justify-content: center !important
				margin: 0

	#content-container .flex .bottom
		text-align: left
		left: 5vw

	.parallax-wrapper
		width: 100% !important
		margin-left: 0 !important

@media only screen and (max-width: 750px)
	.occupation
		width: 70%


#signature
	.path-1
		stroke-dasharray: 320
		stroke-dashoffset: 320
	
	.path-2
		stroke-dasharray: 185
		stroke-dashoffset: 185

	.path-3
		stroke-dasharray: 165
		stroke-dashoffset: 165

	.path-4
		stroke-dasharray: 280
		stroke-dashoffset: 280


@keyframes scrollArrowLoop
	0%
		transform: translateY(-120%)
	
	30%
		transform: translateY(0%)
	
	70%
		transform: translateY(0%)
	
	100%
		transform: translateY(120%)

</style>