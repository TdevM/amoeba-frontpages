<template>
  <div class="row no-gutters align-items-center justify-content-between mb-5">
    <div class="flex-1 pr-4">
      <div class="row no-gutters justiify-content-between align-items-center">
        <div class="s-50x50 round border border-2 b-white">
          <img :src="attempt.user.photo">
        </div>
        <div class="flex-1 pl-3 font-sm bold d-lg-block d-md-none d-sm-block d-none">
          {{userName}}
        </div>
        <div class="col-12 mt-2 font-sm bold d-lg-none d-md-block d-sm-none d-block">
          {{userName}}
        </div>
      </div>
    </div>
    <div class="border b-white dashed-border py-2 px-3 br-5 font-sm">
      {{attempt.meta.coupon.percentage}}% Discount
    </div>
  </div>  
</template>
<script>
export default {
  props: {
    attempt: {
      type: Object,
      required: true
    }
  },
  computed: {
    userName() {
      return this.attempt.user.firstname + this.attempt.user.lastname
    }
  }
}
</script>
