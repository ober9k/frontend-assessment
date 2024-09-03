<script lang="ts">
import HeaderComponent from "@/components/HeaderComponent.vue";
import SectionsComponent from "@/components/SectionsComponent.vue";
import type { Section } from "@/types/section";
import { defineComponent } from "vue";

async function getSections() {
    const response = await fetch("./assets/data.json");
    return response.json();
}

function prepareSections(sections: Array<Section>): Array<Section> {
    let id = 1; /* add some IDs for tracking */

    return sections.map((section: Section) => {
        section.id = id++;
        return section;
    });
}

const sections = prepareSections(await getSections());

export default defineComponent({
    components: { HeaderComponent, SectionsComponent },
    data() {
        return {
            sections: sections
        }
    }
});

</script>

<template>
    <HeaderComponent/>
    <section>
        <SectionsComponent :sections="sections"/>
    </section>
</template>
