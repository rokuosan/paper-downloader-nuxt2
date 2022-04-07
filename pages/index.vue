<template>
  <div>
    <PaperDownloader />
    <Footer />
  </div>
</template>

<script>
import PaperDownloader from "~/components/PaperDownloader.vue";
import Footer from "~/components/Footer.vue";
export default {
    name: "IndexPage",
    components: { PaperDownloader, Footer }
}
</script>
