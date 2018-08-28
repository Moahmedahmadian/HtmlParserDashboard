<template>
 <div class="container">
      <div class="py-5 text-center">
        <img class="d-block mx-auto mb-4" src="https://getbootstrap.com/docs/4.1/assets/brand/bootstrap-solid.svg" alt="" width="72" height="72">
        <h2>Html Parser</h2>
        <p class="lead"></p>
        Please Choose html file by click on button below
      </div>

      <div class="row">
   <div class="col-md-5 order-md-2 mb-5" v-if="!htmlParsed" >
          <h4 class="d-flex justify-content-between align-items-center mb-3">
            <span class="text-muted">{{alert}}</span>
          </h4>
        </div>

        <div class="col-md-5 order-md-2 mb-5" v-if="htmlParsed" >
          <h4 class="d-flex justify-content-between align-items-center mb-3">
            <span class="text-muted">Html Parsed Result</span>
            <span class="badge badge-secondary badge-pill">{{parsedTags.length}}</span>
          </h4>
          <ul class="list-group mb-3">
            <li class="list-group-item d-flex justify-content-between lh-condensed" v-for="item in parsedTags">
              <div>
                <h6 class="my-0">{{item.tagName}}</h6>
              </div>
              <span class="text-muted">{{item.rawText}}</span>
            </li>
          </ul>
  
        </div>
        <div class="col-md-7 order-md-1">
          <h4 class="mb-3">Html File</h4>
          <form class="needs-validation"  method="post" enctype="multipart/form-data">
            <div class="row">
              <div class="col-md-10 mb-10">
                <input type="File" name="file" id="file" accept="*.html" class="form-control" placeholder="" value="" required>
                <div class="invalid-feedback">
                  Please Enter File 
                </div>
              </div>
                <div class="col-md-2">

                </div>
            </div>

            <hr class="mb-4">
            <button class="btn btn-primary btn-lg btn-block" type="button" v-on:click="sendToServer()">{{buttonText}}</button>
          </form>
        </div>
      </div>

      <footer class="my-5 pt-5 text-muted text-center text-small">
        <p class="mb-1">&copy; 2018-2019 Mohamed Ahmadian</p>
        <ul class="list-inline">
     
          <li class="list-inline-item"><a target="_blank" href="https://jobinja.ir/user/MV-7526766">Resume</a></li>
        </ul>
      </footer>
    </div>

</template>

<script>
export default {
  name: 'HtmlParser',
  props: {
    msg: String
  } ,
  data : function(){
      return {
          alert:"  ",
          htmlParsed : false ,
          parsedTags:[],
          buttonText:"Send File to Server"
      } 
  },
  methods :{
      sendToServer : function(){
          if(document.getElementById('file').files.length==0)
          {
              this.buttonText="Please Choose File ";
              return;
          }
          this.buttonText="Send File to Server";
          this.htmlParsed =false;
          this.alert="Please Wait until parsing finish ... !";
        //   var file = document.getElementById('file').files[0];
        //   alert(file.name);
        var data = new FormData()
        data.append('file', document.getElementById('file').files[0])
          fetch('http://localhost:2000/upload', { // Your POST endpoint
                method: 'POST',
                body: data // This is your file object ,
                
            })
           .then(response => response.json())
           .then(res=>{
                this.parsedTags = res;
           }).catch((error)=>{
               alert(error);
           });


          setTimeout(()=>{
              this.htmlParsed =true;
          } , 1000)
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
