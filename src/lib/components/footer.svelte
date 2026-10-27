<script lang="ts">

    import { onMount } from "svelte";
    import { animate } from "animejs";
    import { letterSlideIn, maskSlideIn } from "$lib/animations";
    import { loadPagePromise } from "$lib/store";
    import { onScrolledIntoView } from "$lib/utils";
    import { dataState } from "$lib/state.svelte";

    let footerContainerElement: HTMLElement = $state()!;
    let logoElement: HTMLElement = $state()!; 
    let creditsElement: HTMLElement = $state()!; 
    let statusElement: HTMLElement = $state()!; 
    let fullEmailLinkElement: HTMLElement = $state()!;

    let stayConnectedElement: HTMLElement = $state()!;
    let stayConnectedLineElement: HTMLElement = $state()!;

    let signaturePath1: SVGPathElement = $state()!; 
    let signaturePath2: SVGPathElement = $state()!; 
    let signaturePath3: SVGPathElement = $state()!;
    let signaturePath4: SVGPathElement = $state()!; 

    const currentYear = new Date().getFullYear();
    
    function introAnimations() {

        // Scroll activated animations powered by anime instead of svelte transitions
        const logoAnimate = maskSlideIn(logoElement, {});
        const fullEmailLinkAnimate = letterSlideIn(fullEmailLinkElement, { delay: 6, initDelay: 150 });
        const creditsAnimate = maskSlideIn(creditsElement, { delay: 150 });
        const statusAnimate = letterSlideIn(statusElement, { delay: 6, initDelay: 100 });

        if (stayConnectedElement) {
            stayConnectedElement.style.opacity = "0";
            stayConnectedElement.style.transform = "translate3d(-30px, 0px, 0px)";
        }

        if (stayConnectedLineElement) {
            stayConnectedLineElement.style.transform = "scaleX(0)";
            stayConnectedLineElement.style.transformOrigin = "left center";
        }

        // Intersection observer to run animations when footer is in scroll view
        onScrolledIntoView(footerContainerElement, () => {
            if (stayConnectedElement) {
                animate(stayConnectedElement, {
                    opacity: [0, 1],
                    translateX: ["-30px", "0px"],
                    duration: 900,
                    delay: 50,
                    easing: "cubicBezier(0.165, 0.84, 0.44, 1)"
                });
            }

            if (stayConnectedLineElement) {
                animate(stayConnectedLineElement, {
                    scaleX: [0, 1],
                    duration: 1100,
                    delay: 200,
                    easing: "cubicBezier(0.165, 0.84, 0.44, 1)"
                });
            }

            logoAnimate.anime();
            creditsAnimate.anime();
            fullEmailLinkAnimate.anime();
            statusAnimate.anime();

            // Signature SVG animation
            let animation = [{ strokeDashoffset: '0' }];

            // Signature animation using svg strokeDashOffset
            signaturePath1.animate(animation, {
                duration: 1000,
                delay: 0,
                easing: 'cubic-bezier(.72,.3,.25,1)',
                fill: 'forwards' 
            });
            signaturePath2.animate(animation, {
                duration: 300,
                delay: 1000,
                easing: 'cubic-bezier(.47,.41,.26,1)',
                fill: 'forwards' 
            });
            signaturePath3.animate(animation, {
                duration: 200,
                delay: 1300,
                easing: 'cubic-bezier(.47,.41,.26,1)',
                fill: 'forwards' 
            });
            signaturePath4.animate(animation, {
                duration: 1000,
                delay: 1500,
                easing: 'cubic-bezier(.47,.41,.26,1)',
                fill: 'forwards' 
            });
        });
    }

    onMount(async () => {
        await loadPagePromise;
        introAnimations();
    });

</script>



<div class="footer-wrapper" bind:this={footerContainerElement}>
    <!-- Stay Connected Header Bar -->
    <div class="stay-connected-wrapper">
        <span class="stay-connected-text" bind:this={stayConnectedElement}>Stay Connected</span>
        <div class="stay-connected-line" bind:this={stayConnectedLineElement}></div>
    </div>

    <!-- Main Footer Columns -->
    <div class="footer-content">
        <!-- Left side -->
        <div class="flex-wrapper">
            <div class="logo-wrapper">
                <div class="inline-flex" bind:this={logoElement}>
                    <img src="assets/imgs/logo.png" alt="Adham Hossam Logo" class="logo">
                </div>
            </div>

            <div class="status-wrapper">
                {#if dataState.siteData}
                    {#if dataState.siteData!.availablity_date === ""}
                        <p class="large-text" bind:this={statusElement}>
                            i am currently accepting freelance work, <br>you may reach me on my email.
                        </p>
                    {:else if dataState.siteData!.availablity_date === "TBD"}
                        <p class="large-text" bind:this={statusElement}>
                            I am currently not available for freelance work, <br> but you may reach me on my email for any inquiries.
                        </p>
                    {:else}
                        <p class="large-text" bind:this={statusElement}>
                            i am available for freelance work after <br> {dataState.siteData.availablity_date}.
                        </p>
                    {/if}
                {/if}
                <a class="button large-text" bind:this={fullEmailLinkElement} href="mailto:adhamhossam29@gmail.com" target="_blank">adhamhossam29@gmail.com</a>
            </div>
            
            <div class="credits-wrapper" bind:this={creditsElement}>
                <p class="year">© Anas Mahfouz - {currentYear}</p>
                <p class="credits">
                    designed and developed for Adham Hossam<br>
                    
                    <a class="clickable button no-decor" href="https://github.com/adhamhossam" target="_blank">
                        this website is open source on github
                    </a>
                </p>
            </div>
        </div>

        <!-- Right side -->
        <div class="flex-wrapper decor">
            <!-- Adham Hossam SVG Signature -->
            <svg id="signature" class="name-signature" x="0px" y="0px" viewBox="0 0 220 140" style="stroke: rgb(79, 78, 85);">
                <g>
                    <path
                        bind:this={signaturePath1}
                        class="path-1"
                        style="fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:round;stroke-opacity:1;"
                        d="M 32 98 C 22 75 25 35 48 20 C 65 9 82 22 84 52 C 86 78 72 108 52 106 C 36 104 38 78 58 66 C 78 54 98 56 108 58"/>
                    <path
                        bind:this={signaturePath2}
                        class="path-2"
                        style="fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:round;stroke-opacity:1;"
                        d="M 104 68 C 96 55 106 42 116 52 C 122 58 123 68 122 76 C 121 50 123 24 130 18 C 135 14 138 25 136 48 C 134 70 133 80 138 78"/>
                    <path
                        bind:this={signaturePath3}
                        class="path-3"
                        style="fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:round;stroke-opacity:1;"
                        d="M 138 78 C 141 55 146 36 150 40 C 153 45 148 68 152 74 C 156 80 162 62 166 65 C 170 68 168 76 172 74 C 176 72 181 60 185 64 C 188 67 186 74 190 70 C 194 66 199 62 204 65 C 208 68 207 75 212 72"/>
                    <path
                        bind:this={signaturePath4}
                        class="path-4"
                        style="fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:round;stroke-opacity:1;"
                        d="M 40 120 C 75 116 130 114 185 104 C 205 100 200 112 175 120 C 145 130 110 132 85 130"/>
                </g>
            </svg>
        </div>
    </div>
</div>



<style lang="sass">

@use "../consts.sass" as consts

@include consts.textStyles()

.footer-wrapper
    width: 100vw
    background-color: #111111ff
    display: flex
    flex-direction: column
    padding: 10vh 13vw 15vh 13vw
    margin-top: 25vh
    box-sizing: border-box

    .stay-connected-wrapper
        width: 100%
        display: flex
        flex-direction: row
        align-items: center
        gap: 2.5vw
        margin-bottom: 9vh
        overflow: visible

        .stay-connected-text
            font-family: consts.$fontPriestacy
            font-size: clamp(2.3rem, 3.8vw, 3.8rem)
            color: #ffffff
            white-space: nowrap
            user-select: none
            line-height: 1.5
            letter-spacing: 0.04em
            display: inline-block
            padding: 0.5rem 0
            overflow: visible

        .stay-connected-line
            flex: 1
            height: 1px
            background-color: rgba(255, 255, 255, 0.35)
            transform-origin: left center

        @media only screen and (max-width: 950px)
            gap: 4vw
            margin-bottom: 6vh

            .stay-connected-text
                font-size: clamp(1.8rem, 6vw, 2.6rem)
                line-height: 1.4
                padding: 0.4rem 0

    .footer-content
        width: 100%
        display: flex
        flex-direction: row
        justify-content: space-between

        @media only screen and (max-width: 950px)
            flex-direction: column-reverse

            .flex-wrapper:not(:first-child)
                margin-bottom: 15vh

    @media only screen and (max-width: 950px)
        .flex-wrapper.decor
            display: none !important

    .inline-flex
        flex-grow: 1
        display: flex
        flex-direction: row
        align-items: center


    .logo-wrapper
        margin-bottom: 5vh

        .logo
            display: inline-block
            height: 6vh
            width: auto
            object-fit: contain

    .status-wrapper
        .button.large-text
            margin-top: 2vh

    .credits-wrapper
        margin-top: 5vh
        color: rgba(255,255,255,0.3)

        p.year
            margin-bottom: 1vh
            font-family: consts.$font
            font-size: 1.8vh
            font-weight: normal
            color: rgba(255,255,255,0.3)

        .credits
            font-size: 1.5vh
            line-height: 125%
            white-space: nowrap
            color: rgba(255,255,255,0.3)

            .button
                color: rgba(255,255,255,0.3)

    .large-text
        font-size: 2.5vh

        @media only screen and (max-width: 950px)
            br
                display: none

    .flex-wrapper.decor
        display: flex
        flex-direction: column
        justify-content: center

        .name-signature
            width: 20vh

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

</style>