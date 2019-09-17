<template>
    <div class="detail-checkbox-item">
        <el-checkbox-group v-model="checkedAllList">
            <el-checkbox :disabled="disable" :indeterminate="isIndeterminate" @change="handleCheckAllChange" label="all">
                <slot name="title" :value="defaultValue"><span>全选</span></slot>
            </el-checkbox>
        </el-checkbox-group>
        <el-checkbox-group :disabled="disable" v-model="checkedList" @change="handleCheckedCitiesChange">
            <el-checkbox style="width:20%;" v-for="(item,index) in defaultValue[defaultProps.children]" :label="item[defaultProps.key]" :key="index">{{item[defaultProps.label]}}</el-checkbox>
        </el-checkbox-group>
    </div>
</template>

<script>
export default {
  name: "detail-checkbox-group",
  mixins: [],
  props: {
    disable:{
        type:Boolean,
        default:()=>{
            return false
        }
    },
    value:{
       type:Object,
       required:true,
       default:()=> {
           return {}
       }, 
    },
    defaultProps:{
        type:Object,
        default:()=>{
            return {
                label:'label',
                key:'id',
                checked:'checked',
                children:'childList'
            }
        }
    }
  },
  data(){
      return {
        defaultValue:this.value,
        checkedList:[],
        checkedAllList:[],
      }
  },
  mounted(){
      
  },
  beforeUpdate(){
    
  },
  computed:{
      isIndeterminate(){
          if(this.checkedList.length > 0 && this.checkedList.length < this.value[this.defaultProps.children].length){
              return true 
          }else{
              return false
          }
      },
  },
  watch:{
      value:{
        immediate: true,
        handler:function(newVal,old){
            this.checkedList = newVal[this.defaultProps.children].filter(item=>item[this.defaultProps.checked]).map(item => item[this.defaultProps.key])
        }
      },
      'checkedList.length':{
        immediate: true,
        handler:function(newVal,old){
            if(newVal === this.value[this.defaultProps.children].length){
                this.checkedAllList = ['all']
            }else{
               this.checkedAllList = []
            }
        }
      }
  },
  methods: {
      handleCheckAllChange(val){
          if(val){
              this.checkedList = this.defaultValue[this.defaultProps.children].map(item => item[this.defaultProps.key])
          }else{
              this.checkedList = [];
          }
      },
      handleCheckedCitiesChange(val){
          
      },
      resetCheckList(){
          this.checkedList = this.value[this.defaultProps.children].filter(item=>item[this.defaultProps.checked]).map(item => item[this.defaultProps.key])
      }
  }
};
</script>

<style lang="scss" scoped>
    .detail-checkbox-item{
        line-height:36px;
    }
</style>