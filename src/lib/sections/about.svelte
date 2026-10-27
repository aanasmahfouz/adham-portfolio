<script lang="ts">

	import { onMount } from "svelte";
	import { loadPagePromise } from "$lib/store";
	import { letterSlideIn, maskSlideIn } from "$lib/animations";
	import { loadImage, onScrolledIntoView } from "$lib/utils";
    import { scrollAnchorState, viewPortState } from "$lib/state.svelte";

	let section1Element: HTMLElement;
	let section2Element: HTMLElement;
	let profilePicContainer: HTMLElement;

	// Promise which when resolved will trigger svelte animations
	let sectionOneResolve: (value?: any) => void;
	let sectionOnePromise = new Promise((resolve) => sectionOneResolve = resolve);
	let sectionTwoResolve: (value?: any) => void;
	let sectionTwoPromise = new Promise((resolve) => sectionTwoResolve = resolve);

	onMount(async () => {
		// Wait for page to load
		await loadPagePromise;
		// Set navbar about link's y location to top of aboutContainer
		scrollAnchorState.about = section1Element;

		viewPortState.slickscrollInstance.addOffset({
			element: profilePicContainer!,
			speedY: 0.8
		});

		onScrolledIntoView(section1Element, () => sectionOneResolve(true));
		onScrolledIntoView(section2Element, () => sectionTwoResolve(true));
	});

	function titleIn(node: HTMLElement) {
		const titleAnimation = letterSlideIn(node, { delay: 15 });
		titleAnimation.anime();
	}

	// Add parallax scrolling offsets to slickScroll
	function addSlickScrollOffset(node: HTMLElement) {
		viewPortState.slickscrollInstance.addOffset({
			element: node,
			speedY: 0.8
		});
	}

</script>

<div id="content-container" class="about" bind:this={section1Element}>
	{#await sectionOnePromise then _}
		<div class="content-wrapper">
			<h1 class="title" use:titleIn>
				Hey I'm <br>Adham
			</h1>
			<div in:maskSlideIn={{ duration: 1200, reverse: true, delay: 150 }}>
				<p class="paragraph">
					I'm a software developer and Computer Science student at Al-Ryada University, Egypt. I specialize in designing and engineering scalable, user-friendly applications and modern web experiences.<br><br>With practical experience spanning front-end and back-end development, I craft responsive solutions tailored to real-world needs. Think we can make something great together? Let's talk over email.
				</p>
			</div>
			<div class="social-button-wrapper">
				<div in:maskSlideIn={{ delay: 400, reverse: true }}>
					<span class="button"><a href="mailto:adhamhossam29@gmail.com" target="_blank" class="clickable sublink link">Email Me</a></span>
				</div>
				<div in:maskSlideIn={{ delay: 700, reverse: true }}>
					<span class="button"><a href="https://github.com/adhamhossam" target="_blank" class="clickable sublink link">Github</a></span>
				</div>
			</div>
		</div>
		<div class="profile-image" use:addSlickScrollOffset>
			{#await loadImage("assets/imgs/profile-photo.jfif") then src}
				<img src="{src}" in:maskSlideIn={{ duration: 1200,
					delay: 100,
					reverse: true,
					maskStyles: [
						{ property: "width", value: "100%"},
						{ property: "height", value: "100%"}
					]
				}} alt="Adham's Profile" class="profile-pic">
			{/await}
		</div>
	{/await}
</div>

<div class="horizontal-flex" bind:this={section2Element}>
	{#await sectionTwoPromise then _}
		<ul class="list first">
			<li class="list-title">
				<div in:letterSlideIn={{ initDelay: 400 }}>
					technical expertise
				</div>
			</li>
			<li>
				<div in:letterSlideIn={{ initDelay: 550 }}>
					Web & Front-end
				</div>
				<div 
					class="flex-item" 
					in:maskSlideIn={{ delay: 600 }}>
					<img src="assets/imgs/svg-icons/react.svg" alt="React">
					<img src="assets/imgs/svg-icons/svelte.svg" alt="JavaScript">
				</div>
			</li>
			<li>
				<div in:letterSlideIn={{ initDelay: 650 }}>
					Back-end & Systems
				</div>
				<div class="flex-item" in:maskSlideIn={{ delay: 700 }}>
					<img src="assets/imgs/svg-icons/nodejs.svg" alt="node js">
					<img src="assets/imgs/svg-icons/php.svg" alt="Backend">
				</div>
			</li>
			<li>
				<div in:letterSlideIn={{ initDelay: 750 }}>
					Programming & OOP
				</div>
				<div class="flex-item" in:maskSlideIn={{ delay: 800 }}>
					<span style="font-size: 1.4vh; font-weight: bold; letter-spacing: 0.2vh; color: #aaa;">C++ / JAVA</span>
				</div>
			</li>
			<li>
				<div in:letterSlideIn={{ initDelay: 850 }}>
					Tools & Workflow
				</div>
				<div class="flex-item" in:maskSlideIn={{ delay: 900 }}>
					<span style="font-size: 1.4vh; font-weight: bold; letter-spacing: 0.2vh; color: #aaa;">GIT / GITHUB &bull; VS CODE</span>
				</div>
			</li>
		</ul>
		<ul class="list">
			<li class="list-title">
				<div in:letterSlideIn={{ initDelay: 400 }}>
					certifications
				</div>
			</li>
			<li>
				<div in:letterSlideIn={{ initDelay: 550 }}>
					Prodigy Infotech — Full Stack Intern
				</div>
			</li>
			<li>
				<div in:letterSlideIn={{ initDelay: 650 }}>
					Code Alpha — Web Development Intern
				</div>
			</li>
			<li>
				<div in:letterSlideIn={{ initDelay: 750 }}>
					IEEE — Front-End Intern
				</div>
			</li>
			<li>
				<div in:letterSlideIn={{ initDelay: 850 }}>
					Al-Ryada Univ — Bachelor of CS
				</div>
			</li>
		</ul>
	{/await}
</div>


<style lang="sass">

@use "../consts.sass" as consts

@include consts.textStyles()

#content-container.about
	display: flex
	flex-direction: row
	justify-content: space-between
	overflow: hidden
	padding: 0 5vw
	margin-top: 40vh
	position: relative
	padding-bottom: 5vh

	.profile-image
		width: 55%
		overflow: hidden
		margin-top: -40vh
		position: relative

		img
			height: 80%
			width: 90%
			border-radius: 0.5vh
			object-fit: cover

	.content-wrapper
		box-sizing: border-box
		width: 50%
		height: 100%
		margin: 0 2vw
		padding-right: 4vw
		display: flex
		flex-direction: column
		justify-content: center
		margin-top: 5vh
		box-sizing: border-box
		z-index: 2

		@media only screen and (max-width: 950px)
			&
				width: 80%

				h1
					font-size: 25vw !important

		h1
			font-size: 20vh
			font-weight: 400
			text-transform: none !important

		.paragraph
			margin-top: 10vh
			margin-left: 13vw
			position: relative
			width: 60%
			line-height: 1.5rem

			@media only screen and (max-width: 750px)
				&
					width: 100%
					margin-left: 5vw

			&::before
				content: ""
				position: absolute
				height: 1px
				width: 10vw
				right: 115%
				top: 15%
				background-color: white
				

		.social-button-wrapper
			font-size: 3vh
			margin-left: 13vw
			margin-top: 4vh
			display: inline-block

			& :global(*:not(:last-child))
				margin-right: 2vw

			@media only screen and (max-width: 750px)
				&
					margin-left: 5vw


	@media only screen and (max-width: 950px)
		.profile-image
			display: none

.horizontal-flex
	display: flex
	flex-direction: row
	justify-content: space-between
	padding: 0 13vw
	margin-top: 12vh
	width: 100%
	box-sizing: border-box

	@media only screen and (max-width: 1080px)
		flex-direction: column
		padding: 0 8vw

	.list
		list-style-type: none
		text-align: left

		@media only screen and (max-width: 1080px)
			margin-top: 10vh

		li.list-title
			letter-spacing: 0.6vh
			font-size: 1.3vh
			font-weight: bold

		li
			font-family: consts.$font
			text-transform: uppercase
			font-size: 2vh
			letter-spacing: 0.5vh
			padding: 2vh 0
			border-bottom: 1px solid #444
			display: flex
			flex-wrap: wrap
			flex-direction: row
			justify-content: space-between
			align-items: center
			column-gap: 10vw
			row-gap: 3vh

			img
				height: 2.3vh

</style>