<script>
import { onBeforeMount, onMounted, onUnmounted, ref } from 'vue';
import { Scene } from '../scenes/BlobSceneClass';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export default {

    setup() {
        const blob = ref(null);
        let sceneInstance = null;

        onMounted(() => {
            if (!blob.value) return;
            
            sceneInstance = new Scene({
                domElement: blob.value // Pass native DOM element
            });

            // GSAP animations setup
            const animMesh = sceneInstance.mesh;

            const g1Tl = gsap.timeline({
				delay: 4,
				scrollTrigger: {
					trigger: '.hello',
					start: 'top 60%',
					scroller: '.scroller',
					scrub: 2,
					immediateRender: false,
				},
			});
			const g2Tl = gsap.timeline({
				delay: 4,
				scrollTrigger: {
					trigger: '.techno',
					start: 'top 80%',
					scroller: '.scroller',
					scrub: 2,
					immediateRender: false,
				},
			});

            g1Tl.to(animMesh.rotation, { x: 0.5, y: -1 });
			g1Tl.to(animMesh.camera.position, { x: 3, z: 4.5 }, '-=1');

			g2Tl.to(animMesh.rotation, { x: -2.5, y: 1.5 });
			g2Tl.to(animMesh.scale, { x: 2.5, z: 3 }, '-=1');
			g2Tl.to(animMesh.camera.position, { x: 6, y: -8 }, '-=1');
        });

        onUnmounted(() => {
            if (sceneInstance) sceneInstance.cleanup();
        });

        return { blob };
    },
};
</script>
<template>
  <div id="gl-stuff" ref="blob"></div>
</template>