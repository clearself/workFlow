<template>
    <div class="w100 pic-wrapper event " style="color: #fff;">
    	<bpmn-modeler
    	  ref="refNode"
    	  :xml="xml"
    	  :is-view="false"
    	/>
    </div>
</template>

<script>
import bpmnModeler from '../package/index'
import { xmlStr } from './xmlData.js'
export default {
    name: 'home',
    components: {
        bpmnModeler,
    },
    data() {
        return {
			xml: '', // 后端查询到的xml
        }
    },
	mounted(){
		 this.xml = xmlStr;
	},
}
</script>

<style lang="scss" scoped>
.event{
	height:100%;
	background-color: rgba(17,29,43,1);
}
</style>
