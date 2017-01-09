<template>
	<vhead></vhead>
	<payment-status></payment-status>
	<vfoot></vfoot>
</template>

<script>
import vhead from '../../components/common/Head'
import vindex from '../../components/PaymentStatus'
import vfoot from '../../components/common/Foot'
export default {
components: {
// 可以以key-value的形式注册组件, 此时挂载点的名字就是key
// 否则挂载点和组件名字一致, 即vhead
vhead,
vfoot,
PaymentStatus
}
}
</script>

<style scoped>

</style>
