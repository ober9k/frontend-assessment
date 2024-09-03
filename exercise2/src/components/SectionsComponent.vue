<script lang="ts">
import type { Section } from "@/types/section";
import { defineComponent, type PropType } from "vue";

/**
 * Used a shared layout to switch between tabs and an accordion using CSS.
 * The nav/titles get shown/hidden based on the page width.
 */
export default defineComponent({
    props: {
        sections: {
            type: Array as PropType<Array<Section>>,
            default: () => [],
        },
    },
    data() {
        return {
            activeId: 0,
        }
    },
    methods: {
        handleClick(id: number) {
            this.activeId = (id !== this.activeId) ? id : 0; /* hide self */
        },
        isActive(id: number) {
            return id === this.activeId;
        }
    },
    beforeMount() {
        if (this.sections.length > 0) {
            this.activeId = this.sections[0].id; /* default to first section */
        }
    }
});

</script>

<template>
    <article class="section">
        <div class="section__nav">
            <div v-for="section in sections"
                 class="section__nav-item" :class="{ 'section__nav-item--active': isActive(section.id) }">
                <div class="section__nav-title">
                    <button @click="() => handleClick(section.id)"
                            class="btn btn--tab" :class="{ 'btn--active': isActive(section.id) }">
                        {{ section.title }}
                    </button>
                </div>
            </div>
        </div>
        <div class="section__content">
            <div v-for="section in sections"
                 class="section__content-item" :class="{ 'section__content-item--active': isActive(section.id) }">
                <div class="section__content-title">
                    <button @click="() => handleClick(section.id)"
                            class="btn btn--accordion" :class="{ 'btn--active': isActive(section.id) }">
                        {{ section.title }}
                    </button>
                </div>
                <div class="section__content-body"
                     v-html="section.content">
                </div>
            </div>
        </div>
    </article>
</template>
