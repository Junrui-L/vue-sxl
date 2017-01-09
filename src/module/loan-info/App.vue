<template>
	<vhead></vhead>
	<loan-info></loan-info>
	<vfoot></vfoot>
</template>

<script>
import vhead from '../../components/common/Head'
import LoanInfo from '../../components/LoanInfo'
import vfoot from '../../components/common/Foot'
export default {
components: {
// 可以以key-value的形式注册组件, 此时挂载点的名字就是key
// 否则挂载点和组件名字一致, 即vhead
vhead,
vfoot,
LoanInfo
}
}
</script>

<style scoped>

</style>
