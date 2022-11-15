//lines to copy:
//9 - 14 (html)
//22, 25 - 34, 45 - 71 (Vue / JS)
//79 - 84 (CSS)

<template>
    <div id="window">

        <div v-scroll="handleScroll" class="box1">
            <h1>{{ title }}</h1>
            <p>
                With this snippet. When the chosen element reaches the center of the screen. It will
            </p>
        </div>

    </div>
</template>

<script>

//Inside of the CLI. you will need to import Vue into your vue file to be ale to create this custom directive
import Vue from 'vue';

//directive that uses the JavaScript scroll event listener.
Vue.directive('scroll', {
    inserted: function (el, binding) {
        let f = function (evt) {
            if (binding.value(evt, el)) {
                window.removeEventListener('scroll', f)
            }
        }
        window.addEventListener('scroll', f)
    }
})

export default {
    name: 'HelloWorld',

    data() {
        return {
            title: 'Visible When Scrolled to Center',
        }
    },

    methods: {
        handleScroll: function (evt, el) {

            //value of the elements distance from the center of the y value of the screen.
            let dist_to_center = (el.getBoundingClientRect().bottom - (el.offsetHeight / 2) - (window.innerHeight / 2));

            //lerp values should be between 0 and 1. 0 being the center of the window.
            //enter_lerp should always be less than or equal to the exit_lerp
            const enter_lerp = 0;
            const exit_lerp = 0.5;

            if (dist_to_center < ((window.innerHeight / 2) * enter_lerp)) {
                el.setAttribute(
                    //attributes style changes when triggered are here    
                    'style',
                    'opacity: 1; transform: translate3d(0, 0, 0)'
                )
            }
            else if (dist_to_center > (window.innerHeight / 2) * exit_lerp) {
                el.setAttribute(
                    //attributes style changes when not triggered are here    
                    'style',
                    'opacity: 0; transform: translate3d(0, 20px, 0)'
                )
            }
        }
    },

}
</script>
  
<style scoped lang="scss">
//for this snippet to work. all effected elements must have an ID or class attribute assigned to them.
//transition all is needed to allow animation between the two states.
.box1 {
    opacity: 0;
    transition: 1.5s all;
    background-color: red;
    height: 20vh;
}
</style>