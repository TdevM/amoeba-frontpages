<template>
  <div>
    <div class="border-card p-0 position-relative">
      <div class="p-30p bg-black-grey font-md white position-sticky tl z-pos">
        <h5 class="bold">Course Content</h5>
        <!-- <div class="divider-h mt-4"></div> -->
      </div>

     
      <SectionAccordion v-for="sectionId in sectionIds" :key="sectionId" :section-id="sectionId" />
      
    </div>
  </div>
</template>

<script>
import SectionAccordion from '~/components/AboutCourse/CourseContentCard/SectionAccordion.vue'
export default {
  name: 'CourseContentCard',

  components: {
    SectionAccordion
  },
  props: {
    sectionIds: {
      type: Array,
      default: () => [],
      validator: (values) => values.every((val) => !isNaN(+val))
    }
  }
}
</script>

<style>
.border-bottom-list > div:not(:last-child):after {
  content: '';
  display: block;
  background-color: #f3f3f3;
  height: 1px;
  width: 100%;
  margin: 1.5rem 0;
}
</style>

<style scoped>
.border-card {
  max-height: 1111px;
  overflow: auto;
}

.border-bottom-list {
  max-height: 500px;
  overflow: auto;
}
.border-bottom-list > *:first-child {
  margin-top: 1.5rem;
}
.p-30p {
  padding: 30px;
}
</style>
