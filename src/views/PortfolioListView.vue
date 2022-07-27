<script setup>
import { butterCMS } from "@/utils/ButterCMS";
import { useApiError } from "@/utils/hooks";
import { useRoute } from "vue-router";
import { inject, nextTick, onMounted, ref, watch } from "vue";
import PortfolioTile from "@/components/PortfolioSections/PortfolioTile.vue";

const { setError } = useApiError();
const portfolioProjects = ref(null);
const route = useRoute();
const { handleMounted } = inject("layout")

onMounted(async () => {
    try {
        const pages = await butterCMS?.page.list("portfolio_page");
        portfolioProjects.value = pages?.data.data;
        await nextTick()
        handleMounted()
    } catch(e) {
        setError(e);
    }
});
</script>
<template>
    <section id="portfolio" class="blog-section">
        <div class="container">
            <div class="row justify-content-center">
                <div class="col-lg-6 col-md-10">
                    <div class="section-title text-center">
                        <h2>All Portfolio Projects</h2>
                        <p>
                        Check out my super awesome portfolio!
                        </p>
                    </div>
                </div>
            </div>
                <div class="row justify-content-center">
                    <portfolio-tile
                        v-for="(project, index) in portfolioProjects"
                        :key="index"
                        v-bind="project"
                    />
            </div>
        </div>
  </section>
</template>