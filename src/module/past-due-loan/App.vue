<template>
	<vhead></vhead>
	<pas-due-loan></pas-due-loan>
	<vfoot></vfoot>
</template>

<script>
import vhead from '../../components/common/Head'
import PasDueLoan from '../../components/PasDueLoan'
import vfoot from '../../components/common/Foot'
export default {
components: {
// 可以以key-value的形式注册组件, 此时挂载点的名字就是key
// 否则挂载点和组件名字一致, 即vhead
vhead,
vfoot,
PasDueLoan
}
}
</script>

<style scoped>

</style>
