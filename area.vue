<template>
	<div class="container">
		<div class="row">
			<div class="col">
			<select v-model="f.p" @change="selpro">
				<option :value="i" v-for="(v,i) in pro">{{v.name}}</option>
			</select>
			</div>

			<div class="col">
			<select v-model="f.c" @change="selcity">
				<option :value="i" v-for="(v,i) in city">{{v.name}}</option>
			</select>
			</div>

			<!--<div class="col">
			<select v-model="f.cc" v-show="county.length>0" @change="result">
				<option :value="i" v-for="(v,i) in county">{{v.name}}</option>
			</select>
			</div>-->

		</div>

	</div>
</template>

<style scoped>
.row{display: flex;}
.col{flex-grow:1;}
select{width:98%;padding:.7 0.625rem;border:1px solid #ccc; font-size: 1.4rem;}
</style>

<script>
import data from "./data";

export default {
	data:function(){
		return {
           data:data,
           pro:"",
           city:"",
           county:"",
           f:{
	           p:0,
	           c:0,
	           cc:0,
       		}
		}
	},
	// watch: {
	// 	f: function (val) {
	// 		console.log(val)
	// 	}
	// },
	props: ['pid','cid'],
	mounted:function(){
		this.$nextTick(() => {
			this.pro=this.data;
			this.f.p = this.pid ? this.pid : 0
			this.f.c = this.cid ? this.cid : 0
			console.log(this.f.p);
			this.city=this.pro[this.f.p]['child'];
			//this.county=this.city[5]['child'];
			this.result();
		})
	},
	methods:{
		returnId: function () {
			return {'pid':this.pid, 'cid': this.cid};
		},
		selpro:function(){
			this.city=this.pro[this.f.p]['child'];
			this.county=this.city[0]['child'];
			this.f.c=0;
			this.f.cc=0;
			this.result();
		},
		selcity:function(){
			this.county=this.city[this.f.c]['child']?this.city[this.f.c]['child']:[];
			this.f.cc=0;
			this.result();
		},
		result:function(){
			var re={
				pro:{id:this.pro[this.f.p].id,name:this.pro[this.f.p].name,n:this.f.p},
				city:{id:this.city[this.f.c].id,name:this.city[this.f.c].name,n:this.f.c},
			};
			if(this.county.length>0){
             re.county={id:this.county[this.f.cc].id,name:this.county[this.f.cc].name,n:this.f.cc};
			}else{
				re.county={id:"",name:"",n:""};
			}
			 this.$emit("select",re);  //子组件向父组件传递数据
		}
	}
}	
</script>