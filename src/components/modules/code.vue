<template>
        <button type="button" :disabled="disabled">{{time | change}}</button>

</template>

<script>
	let flag = false
    export default {
        data () {
            return {
                time : '获取验证码',
            	disabled: false,
            }
        },
        props : {
            start : {
                type : Boolean
            }
        },
        watch : {
            start (value,oldvalue) {
                if(value == true){
                    this.countDown()
                }
            }
        },
        methods: {
            countDown () {
            	this.disabled = !this.disabled
                this.time = 10;
                let time = setInterval(()=>{
                    this.time --
                    if(this.time == 0){

                   		this.disabled = false
                        this.$emit('countDown')
                        this.time = '获取验证码'
                        flag = true
                        clearInterval(time)
                    }
                },1000)
            }
        },
        filters : {
            change (value) {
               if(!value) return ""
               if(!isNaN(value)){

                   // if(flag == true){
                       // return `重新发送${value}S`
                   // }
                   return value+'S'
               }else{
                   return value
               }
            }
        }

    }
</script>