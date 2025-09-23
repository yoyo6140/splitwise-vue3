<template>
  <section>
    <div v-if="members.length === 0" class="text-muted">尚無成員</div>
    <div v-else>
      <div 
      v-for="m in members" 
      :key="m.id" 
      class="d-flex justify-content-between align-items-center mb-2 p-2 border rounded">
        <span>{{ m.name }}</span>
        <button @click="deleteMember(m.id)" class="btn btn-sm btn-danger">刪除</button>
      </div>
    </div>
    <div class=" d-flex mt-3 gap-2">
      <input type="text" v-model="newName" placeholder="輸入成員姓名"  class="form-control rounded-start me-2" />
      <button @click.prevent="addMember"  class="btn btn-primary rounded-end" style="white-space: nowrap;">新增成員</button>
    </div>
  </section>
</template>

<script>
export default {
    name:'MemberList',
    props: ['members'], //從app組件傳入的成員資料 綁定變化
    data() {
        return {
            //初始化數據
            newName:''
        }
    },
    methods: {
        //新增成員 加入條件 newMember物件化 emit回傳給父組件
        addMember () {
            if (!this.newName.trim()) {
                return alert('請輸入名字')
            }
            const newMember = {
                id:Date.now().toString(),
                name:this.newName.trim()
            }
            this.$emit('update-members',[...this.members, newMember])
            this.newName = ''
        },
        deleteMember(id) {
            const newMember  = this.members.filter(m => m.id !==id)
            this.$emit('update-members', newMember)
            this.$emit('delete-member-expenses', id)
        }
    }

}
</script>

<style>

</style>