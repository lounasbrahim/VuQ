<template>
    <div>
        <b-nav tabs>
            <b-nav-item style="title" disabled><strong>Quizzz</strong></b-nav-item>
            <b-nav-item style="counter" disabled>Score: {{numCorrect}}/{{numTotal}}  </b-nav-item>
            <b-nav-item style="counter" disabled>Question: {{questIndex + 1}}/{{questions.length}}  </b-nav-item>
        </b-nav>
    </div>
</template>
<script>
export default {
    name:"Header",
    props: [
        "numCorrect", 
        "numTotal",
        "questions",
        "questIndex",
    ]
  
}
</script>
<style scoped>
#app > div:nth-child(2) > ul > li:nth-child(1) > a{
    font-size:30px;
    color:white;
    text-shadow: 1px 1px 1px rgba(0,0,0,0.3);
}
#app > div:nth-child(2) > ul > li:nth-child(2) > a,#app > div:nth-child(2) > ul > li:nth-child(3) > a {
    font-size:18px;
    color:white;
    text-shadow: 1px 1px 1px rgba(0,0,0,0.3);
}
#app > div:nth-child(2) > ul{
    display: flex;
    align-items: center;
}
</style>