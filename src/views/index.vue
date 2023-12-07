<script>
import { onMounted, onUnmounted, ref } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import SplitText from 'gsap/SplitText';
import LocomotiveScroll from 'locomotive-scroll';
import Scene from '../components/BlobScene.vue';

gsap.registerPlugin(SplitText);
gsap.registerPlugin(ScrollTrigger);


export default {
	components: {
		Scene
	},
	setup() {
		const main = ref();
		const scroller = ref(null);
		let ctx;

		onMounted(() => {
			let loco;
			let textSplit = new SplitText('.text-split', {type: "lines, words"});
			let introSplit = new SplitText('.intro-title', {type: "lines, words"});
			let wavyText = textSplit.words;
			let introText = introSplit.words;
			let workItem = gsap.utils.toArray(".work__item");
			let introTl = gsap.timeline({paused: true, delay: 2.5});
			const overlay = document.querySelectorAll('.overlay');
			const overlayPath = document.querySelectorAll('.overlay__path');

			console.log(Scene);

			//marQuee();

			ctx = gsap.context((self) => {

				function initScroll() {

					loco = new LocomotiveScroll({
						el: document.querySelector('[data-scroll-container]'),
						smooth: true,
						getDirection: true
					});

					loco.on("scroll", ScrollTrigger.update);

					ScrollTrigger.scrollerProxy('[data-scroll-container]', {

						scrollTop(value) {
						return arguments.length ? loco.scrollTo(value, 0, 0) : loco.scroll.instance.scroll.y;
						}, 
						getBoundingClientRect() {
							return {top: 0, left: 0, width: window.innerWidth, height: window.innerHeight};
						},
						pinType: document.querySelector('[data-scroll-container]').style.transform ? "transform" : "fixed"

					});
					ScrollTrigger.addEventListener('refresh', () => loco.update());
					ScrollTrigger.refresh();
				}

				initScroll();

				introText.forEach(word => {

					introTl.from(word, {
						opacity: 0,
						y: 150,
						duration: 0.5,
						delay: 0.2,
						stagger: 0.05,
						ease: "power3"
					})

				});

				introTl.from('#gl-stuff', {
					autoAlpha: 0,
					duration: 2,
					ease: 'power3.out'
				});

				wavyText.forEach(word => {

					gsap.from(word, {
						opacity: 0,
						y: 150,
						stagger: 0.5,
						ease: "power3inOut",
						scrollTrigger: {
							trigger: word,
							start: "top 70%",
							scroller: ".scroller",
						}
					})
				});

				workItem.forEach(item => {

					let line = item.querySelectorAll('.line');
					let client = item.querySelectorAll('.work__item a');
					let workSplit = new SplitText(client, {type: "lines, words"});
					let workText = workSplit.lines;

					let workTl = gsap.timeline({
						clearProps: true,
						stagger: 0.5,
						ease: "power3inOut",
						scrollTrigger: {
							trigger: item,
							start: "top 60%",
							scroller: ".scroller"
						}
					});

					workTl.to(line, {
						scaleX: 1.0,
						duration: 1.0
					})

					workTl.from(workText, {
						opacity: 0,
						y: 150
					}, '-= 0.5')
				});

				gsap.timeline({delay: 0.5})
					.set(overlayPath, { 
						attr: { d: 'M 0 100 V 0 Q 50 0 100 0 V 100 z' }
					})
					.to(overlayPath, { 
						duration: 0.3,
						ease: 'power2.in',
						attr: { d: 'M 0 100 V 50 Q 50 100 100 50 V 100 z' }
					})
					.to(overlayPath, { 
						duration: 0.8,
						ease: 'power4',
						attr: { d: 'M 0 100 V 100 Q 50 100 100 100 V 100 z' }
					})
					.to(overlay, {
						autoAlpha: 0,
						ease: 'power4'
					})
					.to('#gl-stuff, .scroller', {
						autoAlpha: 1,
						duration: 2.0,
						ease: "power3inOut"
					})

					introTl.play();
				
			}, main.value); // <- Scope!

			
		});

		onUnmounted(() => {
			ctx.revert(); // <- Easy Cleanup!
		});

		return { main };

	},
	methods: {
		
	}
};
</script>

<template>
    <div id="home" ref="main">
		<Scene />
        <main class="scroller" ref="scroller" data-scroll-container data-scroll-section-inview >
			<section data-scroll-section class="intro">
				<div class="intro__inner">
					<h1 class="intro-title">
						Code<br>
						<span class="hollow-text">Sweat<span class="small">&</span></span><br>
						Tears
					</h1>
				</div>
			</section>
			<section data-scroll-section class="hello">
				<div class="hello__inner">
					<h3 class="hello__text text-split anim-text">
						<span>Hi there!</span>
						Welcome to my portfolio.<br>
						I'm Mehdy, a frontend and creative developer based in Berlin.<br> I like creating neat interface with cool animations and clean code. Currently discovering the magic world of threejs and vue. Enjoy (~‾▿‾)~
					</h3>
				</div>
			</section>
			<section data-scroll-section class="work">
				<div class="work__inner">
					<h3 class="title-lg is-bold text-split">WORK</h3>
					<div class="work__items">
						<div class="work__item">
							<a href="https://www.dixa.com" target="_blank">
								Dixa
							</a>
							<span class="line"></span>
						</div>
						<div class="work__item">
							<a href="https://web.archive.org/web/20230410111542/https://28marketplace.co.uk/" target="_blank">
								28 Market Place
							</a>
							<span class="line"></span>
						</div>
						<div class="work__item">
							<a href="https://www.canvasoffices.co.uk" target="_blank">
								Canvas Offices
							</a>
							<span class="line"></span>
						</div>
						<div class="work__item">
							<a href="https://web.archive.org/web/20190507120134/https://kutir.co.uk/" target="_blank" >
								Kutir
							</a>
							<span class="line"></span>
						</div>
						<div class="work__item">
							<a href="https://lahkoh.co.uk/" target="_blank">
								Lah Koh
							</a>
							<span class="line"></span>
						</div>
						<div class="work__item">
							<a href="https://web.archive.org/web/20211027092818/https://sohobrewing.com/" target="_blank">
								Soho Brewing
							</a>
							<span class="line"></span>
						</div>
						<div class="work__item">
							<a href="https://web.archive.org/web/20180811165212/http://pydarplace.co.uk/" target="_blank">
								Pydar Place
							</a>
							<span class="line"></span>
						</div>
					</div>
				</div>
			</section>
			<section data-scroll-section class="techno">
				<h3 class="lead text-center is-bold text-split">Things I use:</h3>
				<div class="marquee">
					<div class="marquee__inner first">
						<span>GSAP</span>
						<span>COFFEE</span>
						<span>THREEJS</span>
						<span>ES6</span>
					</div>
					<div class="marquee__inner second">
						<span>WORDPRESS</span>
						<span>CLUBMATE</span>
						<span>BEDROCK</span>
						<span>WEBPACK</span>
					</div>
					<div class="marquee__inner first">
						<span>GULP</span>
						<span>COMPOSER</span>
						<span>PIZZA</span>
						<span>DOCKER</span>
					</div>
					<div class="marquee__inner second">
						<span>VUE</span>
						<span>LARAVEL</span>
						<span>SASS</span>
						<span>FIGMA</span>
					</div>
				</div>
				
			</section>
			<section data-scroll-section class="more">
				<div class="more__inner">
					<a class="title-md is-bold text-center contact" href="mailto:contact@mehdy-elm.com" target="_blank">contact</a>
					<div class="socials">
						<a href="https://www.linkedin.com/in/mehdyelm/" target="_blank">linkedin</a>
						<a href="https://github.com/M3MS" target="_blank">github</a>
						<a href="https://www.instagram.com/_m3ms_/" target="_blank">instagram</a>
					</div>
				</div>
			</section>
		</main>
		<svg class="overlay" width="100%" height="100%" viewBox="0 0 100 100" preserveAspectRatio="none">
			<path class="overlay__path" vector-effect="non-scaling-stroke" d="M 0 100 V 100 Q 50 100 100 100 V 100 z" />
		</svg>
    </div>

</template>
