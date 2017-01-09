<template>
	<vhead></vhead>
	<schedule-payment></schedule-payment>
	<vfoot></vfoot>
</template>

<script>
import vhead from '../../components/common/Head'
import SchedulePayment from '../../components/SchedulePayment'
import vfoot from '../../components/common/Foot'
import flex from '../../static/js/flexible'
export default {
components: {
// 可以以key-value的形式注册组件, 此时挂载点的名字就是key
// 否则挂载点和组件名字一致, 即vhead
vhead,
ApplyLoan,
SchedulePayment
}
}
</script>

<style scoped>

</style>
