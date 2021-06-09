<template>
    <div class="monthAndYear">
        
        {{months[cmonth]}} {{cyear}}
    </div>
</template>
<script>
export default {
    data() {
        return {
         months: ["Jan", "Feb", "Mar","Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],

        }
    },props:['cmonth','cyear']
}
</script>
<style scoped>

</style>