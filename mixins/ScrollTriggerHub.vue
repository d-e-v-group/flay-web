<script>
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/dist/ScrollTrigger'
gsap.registerPlugin(ScrollTrigger)

export default {
  data() {
    return {
      scrolltrigger_initiated: false,
      fade_up_els: [],
      slide_up_enter_els: [],
      slide_up_leave_els: [],
      animate_in_class_els: [],
      primary_header_el: [],
    }
  },
  activated() {
    // Restart header animations upon return to page
    if (this.scrolltrigger_initiated) {
      this.animate_in_class_els.forEach(el => {
        if (el.classList.contains('animate-in')) {
          el.classList.remove('animate-in')
          setTimeout(() => {
            el.classList.add('animate-in')
          }, 250)
        }
      })
    }

    this.scrollTriggerInit()
  },
  deactivated() {},
  methods: {
    scrollTriggerInit() {
      if (!this.scrolltrigger_initiated && process.client) {
        this.scrolltrigger_initiated = true

        // Fade In & Slide Up
        this.fade_up_els = document.querySelectorAll('[data-st-fade_up]')
        this.fade_up_els.forEach(this.add_fade_up_animation)

        // Slide Up Enter
        this.slide_up_enter_els = document.querySelectorAll('[data-st-slide_up_enter]')
        this.slide_up_enter_els.forEach(this.add_slide_up_enter_animation)

        // Slide Up Leave
        this.slide_up_leave_els = document.querySelectorAll('[data-st-slide_up_leave]')
        this.slide_up_leave_els.forEach(this.add_slide_up_leave_animation)

        // Animate In Class elements
        this.animate_in_class_els = document.querySelectorAll('[data-st-animate_in_class]')
        this.animate_in_class_els.forEach(this.toggle_animate_in_class)
      }
    },
    add_fade_up_animation(el, index) {
      const timeline = gsap
        .timeline({
          scrollTrigger: {
            trigger: el,
            start: 'top bottom',
            ease: 'power2',
            toggleActions: 'play none none none',
          },
        })
        .from(el, {
          y: 50,
          opacity: 0,
          duration: 2,
        })
    },
    add_slide_up_enter_animation(el, index) {
      const timeline = gsap
        .timeline({
          scrollTrigger: {
            trigger: el,
            start: 'top bottom',
            end: 'top center',
            scrub: 1,
          },
        })
        .from(el, {
          y: 100,
        })
    },
    add_slide_up_leave_animation(el, index) {
      // const timeline = gsap
      //   .timeline({
      //     scrollTrigger: {
      //       trigger: el,
      //       start: 'bottom 40%',
      //       end: 'bottom 0px',
      //       scrub: 1,
      //     },
      //   })
      //   .to(el, {
      //     y: -50,
      //     scale: 0.95,
      //     opacity: 0,
      //   })
    },
    toggle_animate_in_class(el, index) {
      const timeline = gsap.timeline({
        scrollTrigger: {
          trigger: el,
          start: 'top bottom',
          // toggleClass: { targets: el, className: 'animate-in' },
          onEnter: () => {
            el.classList.add('animate-in')
          },
        },
      })
    },
  },
}
</script>
