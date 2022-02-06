<html>
<head>
<meta name = "viewport" content="width=device-width, initial-scale=1.0"> 
<style>

hr.dashed {
  border-top: 3px dashed #bbb;
}
.column1 {
  float: left;
  width: 40%;
  height:95%
}
.column2{
  float: left;
  width: 30%;
  height:95%
}
.column3 {
  float: left;
  width: 30%;
  height:95%
}

/* Stops the float property from affecting content after the columns */
.columns:after {
  content: "";
  display: table;
  clear: both;
}

</style>
<script type = "text/javascript">
var cantry=1
var ws;

var url2="ws://dbtrades.westeurope.azurecontainer.io:10012";

var url1="http://dbtrades.westeurope.azurecontainer.io:10100/"
//Var url2="ws://64c6-106-214-87-93.ngrok.io";

//Var url1="http://ec56-106-214-87-93.ngrok.io/";

function init1(){

const Http = new XMLHttpRequest();
const url=url1+"checkstoplossthread";
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){ 

}
if (this.readyState==4&& this.status!=200){
 //window.alert(this.response);
};
}
}

function connectws(){
  ws = new WebSocket(url2);

  // Set event handlers.

  ws.onopen = function(e) {
	document.getElementById("console").value = document.getElementById("console").value+"\n"+ "Connected Successfully";

  };
  
  ws.onmessage = function(e) {
	// e.data contains received string.
	//window.alert("onmessage: " + e.data);
	const marray=e.data.split(":");
	//window.alert(marray)
	if (marray[0]=="verify") { document.getElementById("console").value = document.getElementById("console").value+"\n"+marray[1];}
	if (marray[0]=="noverify") { document.getElementById("console").value = document.getElementById("console").value+"\n"+marray[1];}
	if (marray[0]=="deposit") { document.getElementById("console").value = document.getElementById("console").value+"\n"+ "Deposit Amount is:"+marray[1];}
	if (marray[0]=="buyprice") { document.getElementById("console").value = document.getElementById("console").value+"\n"+ "Bought at:"+marray[1];}
	if (marray[0]=="sellprice") { document.getElementById("console").value = document.getElementById("console").value+"\n"+ "Sold at:"+marray[1];}
	if (marray[0]=="error") { document.getElementById("console").value = document.getElementById("console").value+"\n"+ "error at:"+marray[1];}
	if (marray[0]=="rejected") { document.getElementById("console").value = document.getElementById("console").value+"\n"+ "Rejected at:"+marray[1];}
	if (marray[0]=="buypic") { imgElem.setAttribute('src', "data:image/png;base64," + marray[1]);document.getElementById("downloadbuy").href ="data:image/png;base64," + marray[1]; }
	if (marray[0]=="sellpic") { imgElem1.setAttribute('src', "data:image/png;base64," + marray[1]);document.getElementById("downloadsell").href ="data:image/png;base64," + marray[1];}
	if (marray[0]=="targetupd") { document.getElementById("console").value = document.getElementById("console").value+"\n"+ "New Target Profit:"+marray[1];}
	if (marray[0]=="lossupd") { document.getElementById("console").value = document.getElementById("console").value+"\n"+ "New target loss:"+marray[1];}
	if (marray[0]=="common") { document.getElementById("console").value = document.getElementById("console").value+"\n"+marray[1];}
	if (marray[0]=="initloss") { document.getElementById("stoploss1").value = marray[1];}
	if (marray[0]=="donothing") { }
	if (marray[0]=="liveprice") { document.getElementById("liveprice").innerHTML = marray[1];}
	if (marray[0]=="block") { document.getElementById("getready").disabled = true ;
							  document.getElementById("execute").disabled = true ;
							  document.getElementById("additem").disabled = true ;
							  document.getElementById("cancel").disabled = true ;}
	if (marray[0]=="unblock") { document.getElementById("getready").disabled = false ;
						  document.getElementById("execute").disabled = false ;
						  document.getElementById("additem").disabled = false ;
						  document.getElementById("cancel").disabled = false ;}
//	else { var baseStr64=e.data;	
//	imgElem.setAttribute('src', "data:image/jpg;base64," + baseStr64);

//	}
	
  };
  
  ws.onclose = function() {
	document.getElementById("console").value = document.getElementById("console").value+"\n"+ "Connection Closed";
	if (cantry==1){
	setTimeout(function() {
      connectws();
    }, 1000); }
  };

  ws.onerror = function(e) {
	cantry=0
	document.getElementById("console").value = document.getElementById("console").value+"\n"+ "Connection Error";

  };
}
function init() {
	//window.alert("starting init");
	
  // Connect to Web Socket

	connectws();
	setTimeout(function() {
      init1();
    }, 2000);
	setTimeout(function() {
      additemsinit();
    }, 1000);
	
}

function inputevent1(){
//	alert(document.getElementById("target1").value);
}
	
function additems1(){
const Http = new XMLHttpRequest();
const url=url1+"additems1/"+document.getElementById("additems1").value;
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){
	strr=this.response;

	//window.alert(this.response) ;
	tokenselect=document.getElementById("addselect");
	const choiced=strr.split(":");

	for (let i = 0; i < choiced.length; i++) {

	  var opt = document.createElement('option');
		opt.value = choiced[i];
		opt.innerHTML = choiced[i];
		tokenselect.appendChild(opt);
	}

}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}

function additemsinit(){
const Http = new XMLHttpRequest();
const url=url1+"inittok";
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){
	strr=this.response;

	//window.alert(this.response) ;
	tokenselect=document.getElementById("token");
	tokenselect.length=0;
	const choiced=strr.split(":");

	for (let i = 0; i < choiced.length; i++) {

	  var opt = document.createElement('option');
		opt.value = choiced[i];
		opt.innerHTML = choiced[i];
		tokenselect.appendChild(opt);
	}

}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}
function additems2(){
const Http = new XMLHttpRequest();
const url=url1+"additems2/"+document.getElementById("addselect").value;
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){ 
   tokenselect=document.getElementById("token");
   valtoadd=document.getElementById("addselect").value
   valtoadd1=valtoadd.split("-")[0]
   var opt1 = document.createElement('option');
   opt1.value = valtoadd1;
   opt1.innerHTML = valtoadd1;
   tokenselect.appendChild(opt1);
   }
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}

function verifyfn(){
//window.alert("starting ws");

//const url=url1+"startws";
//Http.open("GET",url);
//Http.send();
//Http.onreadystatechange=function(){
//if (this.readyState==4&& this.status==200){ ;}
//if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
//};
//window.alert("init");

//window.alert("vrify");
//const Http = new XMLHttpRequest();
const Http = new XMLHttpRequest();
const url01=url1+"verify/"+document.getElementById("Uname").value;
Http.open("GET",url01);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){ 
	resp=this.response;
	//window.alert(this.response);
	if (resp.includes('success')){ init();}
	if (resp.includes('fail')){ window.alert("Failed Quote");}
	if (resp.includes('duplicate')){ window.alert("Someone already logged in ");}
}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}

function cancelorder(){
const Http = new XMLHttpRequest();
const url=url1+"cancelorder";
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){ ;}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}

function updstoploss(){
const Http = new XMLHttpRequest();
const url=url1+"updstoploss/"+document.getElementById("stoploss1").value;
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){ ;}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}
function updtarget(){
const Http = new XMLHttpRequest();
const url=url1+"updtarget/"+document.getElementById("target1").value;
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){ ;}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}

function p1(){
const Http = new XMLHttpRequest();
const url=url1+"initial"
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){ ;}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}

function getstat(){
const Http = new XMLHttpRequest();
const url=url1+"getstat"
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){ ;}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}


function exitnow(){
const Http = new XMLHttpRequest();
const url=url1+"exitordernow"
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){;}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}

function getready(){
const tok =document.getElementById("token").value
const ttype =document.getElementById("ttype").value
const buysell =document.getElementById("buysell").value
const quant =document.getElementById("quant").value
const Http = new XMLHttpRequest();
const url=url1+"getready/"+tok+"/"+ttype+"/"+buysell+"/"+quant
Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){

if (this.readyState==4&& this.status==200){ ;}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}

function execute(){
const price1 =document.getElementById("price1").value

const Http = new XMLHttpRequest();
const url=url1+"startnow/"+price1

Http.open("GET",url);
Http.send();
Http.onreadystatechange=function(){
if (this.readyState==4&& this.status==200){ ;}
if (this.readyState==4&& this.status!=200){ window.alert(this.response);}
};
}
</script>

<body>
<div style="height:5%;background-color:yellow;"> <h2 style="background-color:lightgrey;">Welcome to DB Trades</h1></div>

<div class="columns">
  <div class="column1" style="background-color:grey;">


 <label><b>Share the Quote     
 </b>    
</label> 
<input type="text" name="Uname" id="Uname" placeholder="quote">    
<br><br>    

<button onclick="verifyfn()">Verify Quote</button>    
<br> 
<hr class="dashed">
<br>


 <label style="display:block;width:50px;"><b>Select-Scrip    
 </b>    
</label>  
<select id="token">

</select>

<select id="ttype">
  <option value="EQ">EQ</option>
  <option value="FO">FO</option>
</select>
<select id="buysell">
  <option value="buy">buy</option>
  <option value="sell">sell</option>
</select>

<input  id="quant" placeholder="quantity" style="width: 60px;color:red"></input>
<button id ="getready" onclick="getready()">getready</button>
<br><br>

<hr class="dashed">

 <label style="display:block;width:50px;"><b>Price     
 </b>    
</label>   
<input id="price1" placeholder="price" style="width: 60px;color:red" ></input>
<button id="execute" onclick="execute()">ExecuteOrder</button>
<button id="cancel" onclick="cancelorder()">CancelOrder</button>
<hr class="dashed">
<br>

<label style="display:block;width:50px;"><b>Stoploss     
 </b>    
</label>   
<input type="number"  min="0" max="99999999" step="2" value="0" id="stoploss1"  style="width: 60px;color:red" ></input>
<button onclick="updstoploss()">Upd</button>
<br>
 <label style="display:block;width:50px;"><b>Target     
 </b>    
</label>   
<input id="target1" oninput=inputevent1() type="number"  min="0" max="99999999" step="2" value="0"  style="width: 60px;color:red" ></input>
<button onclick="updtarget()">Upd</button>
<br><br>

<br><br>	
<hr class="dashed">

<button id="exit" onclick="exitnow()">Exitorder(Market)</button>
<hr class="dashed">
<br>
<br>
<P> Add a Ticker</p>
<input id="additems1" ></input>
<button id="additem" onclick="additems1()">Check</button>
<select id="addselect"></select>
<button onclick="additems2()">Add</button>
<h1> Live Price </h>
<h1 id="liveprice"><h1>




  </div>

  <div class="column2" style="background-color:green;">
<textarea id="console" style="display:block;width:100%;color:#fff;height:100%;background-color:black"> </textarea>
  </div>


  <div class="column3" style="background-color:red;">
    <div style="background-color:green;height:50%;"> 
	   <div style="background-color:blue;height:10%;">  
	             Buy Screenshot
	           <a href="" id="downloadbuy" download="buy.jpg">download</a>
		

	   </div>
	   <div style="background-color:silver;height:90%;">  
		<img id="imgElem" style="width:100%;display:block;height:100%;background-color:grey" ></img>
	</div>	
    </div>
    <div style="background-color:orange;height:50%;">
	   <div style="background-color:blue;height:10%;">  
		 Sell Screenshot
		<a href="" id="downloadsell" download="sell.jpg">download</a>
	</div>
	   <div style="background-color:silver;height:90%;"> 
		<img id="imgElem1" style="width:100%;display:block;height:100%;background-color:grey" ></img>
	 </div>	
    </div>
		
  </div>



</div>

</body>
</html>


</head>
</html>


    
