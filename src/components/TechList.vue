<template>
    <nav class="right">
        <ul class="tech">
            <li v-for="topic in topics" >
                <img class="tech_img" :src=topic.thumbnail >
                <h3>{{topic.name}}</h3>
                <p>{{topic.name}} is ...</p>
            </li>
        </ul>
    </nav>
</template>
<script>
export default {
    name : "TechList"
    ,created: function () {
        this.$http.get(`/api/topics/`)
            .then(response => {
                let { data } = response
                data.forEach(topic => {
                    this.$http.get(`/api/topics/thumbnail/${topic.thumbnail}`)
                        .then(response => {
                            topic.thumbnail = `data:image/png;base64,${response.data}`
                        })
                })
                this.topics = data;
            })
    }
    ,data: function(){
        return{
            topics: {}
      }  
    }
}
</script>
<style>

.right{
    background:inherit;
    overflow:auto;
    width:300px;
    padding-top:10px;
    border-left: 1px solid rgba(200,200,200,0.5);
    box-sizing : border-box;
    height:94vh;
}
.right>ul.tech{
    background:inherit;
    overflow:auto;
    padding:0px 15px 10px 15px;

}
ul.tech>li:hover{
    background:rgba(200,200,200,0.5);
    border-bottom : 1px solid rgba(200,200,200,0.5);
    cursor:pointer;
}
ul.tech>li{
    background:inherit;
    list-style: none;
    padding : 12px 0 12px 0;
    margin : 0 0 9px 0;
    border-bottom : 1px solid #eee;
    border-radius : 5px;
}
ul.tech>li:last-child{
    border-bottom : none;
}
li>.tech_img{
    float:left;
    width:50px;
    height:50px;
    margin: 0 10px 0 10px;
}
</style>
