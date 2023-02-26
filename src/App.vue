<template>
  <v-app>
     <v-card>
        <v-app-bar dark color="pink">
          <v-app-bar-nav-icon></v-app-bar-nav-icon>
          <v-toolbar-title>To-Do 리스트</v-toolbar-title>
        </v-app-bar>

      <v-main>
          <v-container>
             <v-row my-5>
                <v-col cols="8"  offset="1"  >
                    <!-- 실행되자마자 입력 포커스를 가지도록 autofocus 설절 -->
                    <v-text-field   label="할 일" required autofocus v-model="sTodoTitle"  >   
                    </v-text-field>       
                </v-col>
                <v-col cols="2">
                  <v-btn  fab class="mx-2"  dark   color="pink"  @click="fnsubmitTodo()" >
                    <v-icon dark>mdi-plus</v-icon>
                  </v-btn>
                </v-col>
             </v-row>


             <v-row>
                <v-col cols="12" >

                  <v-list two-line  v-for="(item, i) in items"  :key="i">
            
                        <v-card flat color="grey lighten-3"  v-if="!item.b_edit">
                            <v-list-item-group>
                              <v-list-item class="py-2">
                                <template >
                                  <v-list-item-action>
                                    <v-checkbox v-model="item.b_completed"  @change="fnCheckboxChange(item.key)"></v-checkbox>
                                  </v-list-item-action>

                                  <v-list-item-content>
                                    <v-list-item-title v-text="item.todo_title" :class="{'style_completed':item.b_completed}"></v-list-item-title>
                                    <v-list-item-subtitle class="mt-2">

                                        <v-icon class="pointer" @click="fnSeEditTodo(item.key)">create</v-icon>
                                        <v-icon class="pointer" @click="fnRemoveTodo(item.key)">delete</v-icon>

                                    </v-list-item-subtitle>
                                  </v-list-item-content>
                                </template>
                              </v-list-item>
                            </v-list-item-group>  
                      </v-card>


                      <v-card v-else dark>
                            <v-list-item-group>
                              <v-list-item class="py-2">
                                
                                <template v-slot:default="{ active }">
                                  <v-list-item-action>
                                    <v-checkbox v-model="item.b_completed"  @change="fnCheckboxChange(item.key)"></v-checkbox>
                                  </v-list-item-action>

                                  <v-list-item-content >

                                    <v-col cols="11"   >
                                        <v-text-field v-model="item.todo_title"   color="blue darken-2"  autofocus clearable   
                                          required
                                        ></v-text-field>
                                  
                                    </v-col>
                                    <v-col cols="1"   >
                                      <v-list-item-subtitle class="mt-2">
                                        <v-icon class="pointer" @click="fnSaveEditTodo(item.key)">save</v-icon>
                                        <v-icon class="pointer" @click="fnCancelEdit(item.key)">cancel</v-icon>
                                      </v-list-item-subtitle>
                                    </v-col>

                                  </v-list-item-content>



                                </template>
                              </v-list-item>
                            </v-list-item-group>  
                      </v-card>




                  </v-list>
                </v-col>
                
             </v-row>



          </v-container>
      </v-main>
  </v-card> 
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data () {
    return {
       sTodoTitle:"",
       selectedItem: 1,

       items: [
        {key:1, todo_title: 'Real-Time' , b_edit:false, b_completed:false},
        {key:2, todo_title: 'Audience' , b_edit:false , b_completed:false},
        {key:3, todo_title: 'Conversions' , b_edit:false, b_completed:false},
        {key:4, todo_title: 'Conversions' , b_edit:true, b_completed:false},
      ],
      checkbox: true,
      settings: [], 

      }
     
  },
  methods: {
    //할일 추가 폼 버튼 클릭시
    fnsubmitTodo(){
      if(!this.sTodoTitle){
        window.alert("할일을 입력해 주세요.");
        return;
      }
      const data= {
            key:new Date().getTime(), 
            todo_title:this.sTodoTitle ,
            b_edit:false, 
            b_completed:false
          }
      this.items.push(data);
      this.sTodoTitle="";
    },
    

    //수정 버튼 클릭시
    fnSeEditTodo(key){
      this.items.map(item=>{
          if(item.key===key){
            item.b_edit=true
          }
      });
    },

    //수정후 저저장 버튼 클릭시
    fnSaveEditTodo(key){
      
      this.items.map(item=>{
          if(item.key===key){
            item.b_edit=false
          }
      });
    },


    //취소버튼 클릭시
    fnCancelEdit(key){
      this.items.map(item=>{
          if(item.key===key){
            item.b_edit=false
          }
      });
    },

    //삭제 버튼 클릭시
    fnRemoveTodo(key){
  
      const result=this.items.filter((item)=>(item.key!==key));   
      this.items=result;

    },


    fnCheckboxChange(key){
       
    }

  },
  computed: {
    
  },
};
</script>

<style>
  .pointer {
    /* 마우스포인터를 손모양으로 변경 */
    cursor: pointer;
  }

  .style_completed {
    /* 할 일의 제목을 취소선으로 변경 */
    text-decoration: line-through;
  }
</style>