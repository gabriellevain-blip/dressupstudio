<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DressUp Studio</title>

<style>

body{
    margin:0;
    font-family:Arial,sans-serif;
    background:#eaeaea;
}

.toolbar{
    background:#2ecc71;
    padding:10px;
    display:flex;
    gap:8px;
    flex-wrap:wrap;
}

button{
    padding:10px;
    border:none;
    border-radius:8px;
    cursor:pointer;
    font-weight:bold;
}

#workspace{
    position:absolute;
    left:0;
    top:60px;
    width:calc(100% - 220px);
    height:calc(100vh - 60px);
    background:#d8d8d8;
    overflow:hidden;
}

#photo{
    position:absolute;
    left:50%;
    top:50%;
    transform:translate(-50%, -50%);
    max-width:90%;
    max-height:90%;
    pointer-events:none;
}

#wardrobe{
    position:fixed;
    right:0;
    top:60px;
    width:220px;
    height:calc(100vh - 60px);
    background:white;
    border-left:2px solid #ccc;
    overflow-y:auto;
}

#wardrobe h2{
    text-align:center;
}

.wardrobe-item{
    width:180px;
    margin:10px auto;
    display:block;
    cursor:pointer;
    border:2px solid transparent;
}

.wardrobe-item:hover{
    border-color:#2ecc71;
}

.selection{
    outline:3px solid lime;
}

</style>
</head>
<body>

<div class="toolbar">

<button onclick="photoInput.click()">
📷 Photo
</button>

<button onclick="habitInput.click()">
👕 Importer vêtement
</button>

<button onclick="largeurPlus()">
↔ +
</button>

<button onclick="largeurMoins()">
↔ -
</button>

<button onclick="hauteurPlus()">
↕ +
</button>

<button onclick="hauteurMoins()">
↕ -
</button>

<button onclick="rotationGauche()">
↺
</button>

<button onclick="rotationDroite()">
↻
</button>

<button onclick="supprimerVetement()">
🗑
</button>

</div>

<div id="workspace">
    <img id="photo">
</div>

<div id="wardrobe">
    <h2>👕 Garde-robe</h2>
    <div id="wardrobeItems"></div>
</div>

<input type="file" id="photoInput" accept="image/*" hidden>
<input type="file" id="habitInput" accept="image/png" hidden>

<script>

const workspace = document.getElementById("workspace");
const photo = document.getElementById("photo");

let selection = null;

document.getElementById("photoInput")
.addEventListener("change",e=>{

const file=e.target.files[0];

if(!file)return;

photo.src=URL.createObjectURL(file);

});

document.getElementById("habitInput")
.addEventListener("change",e=>{

const file=e.target.files[0];

if(!file)return;

const reader=new FileReader();

reader.onload=function(event){

const image64=event.target.result;

let vetements=
JSON.parse(localStorage.getItem("vetements")||"[]");

vetements.push(image64);

localStorage.setItem(
"vetements",
JSON.stringify(vetements)
);

afficherGardeRobe();

};

reader.readAsDataURL(file);

});

function afficherGardeRobe(){

const zone =
document.getElementById("wardrobeItems");

zone.innerHTML="";

const vetements=
JSON.parse(
localStorage.getItem("vetements")||"[]"
);

vetements.forEach(src=>{

const mini=document.createElement("img");

mini.src=src;

mini.className="wardrobe-item";

mini.onclick=function(){

creerVetement(src);

};

zone.appendChild(mini);

});

}

function creerVetement(src){

const img=document.createElement("img");

img.src=src;

img.style.position="absolute";
img.style.left="400px";
img.style.top="200px";
img.style.width="200px";
img.style.height="200px";
img.style.cursor="move";

img.dataset.rotation="0";

workspace.appendChild(img);

rendreManipulable(img);

}

function rendreManipulable(img){

img.addEventListener("click",function(e){

e.stopPropagation();

if(selection){
selection.classList.remove("selection");
}

selection=img;

selection.classList.add("selection");

});

let drag=false;
let startX=0;
let startY=0;

img.addEventListener("mousedown",function(e){

drag=true;

startX=e.clientX;
startY=e.clientY;

});

document.addEventListener("mousemove",function(e){

if(!drag)return;

let dx=e.clientX-startX;
let dy=e.clientY-startY;

img.style.left=
(img.offsetLeft+dx)+"px";

img.style.top=
(img.offsetTop+dy)+"px";

startX=e.clientX;
startY=e.clientY;

});

document.addEventListener("mouseup",function(){

drag=false;

});

}

workspace.addEventListener("click",()=>{

if(selection){

selection.classList.remove("selection");

selection=null;

}

});

function largeurPlus(){

if(!selection)return;

selection.style.width=
(selection.offsetWidth+20)+"px";

}

function largeurMoins(){

if(!selection)return;

selection.style.width=
Math.max(
40,
selection.offsetWidth-20
)+"px";

}

function hauteurPlus(){

if(!selection)return;

selection.style.height=
(selection.offsetHeight+20)+"px";

}

function hauteurMoins(){

if(!selection)return;

selection.style.height=
Math.max(
40,
selection.offsetHeight-20
)+"px";

}

function rotationDroite(){

if(!selection)return;

let angle=
parseInt(
selection.dataset.rotation
);

angle+=15;

selection.dataset.rotation=angle;

selection.style.transform=
`rotate(${angle}deg)`;

}

function rotationGauche(){

if(!selection)return;

let angle=
parseInt(
selection.dataset.rotation
);

angle-=15;

selection.dataset.rotation=angle;

selection.style.transform=
`rotate(${angle}deg)`;

}

function supprimerVetement(){

if(!selection)return;

selection.remove();

selection=null;

}

document.addEventListener("keydown",e=>{

if(e.key==="Delete" && selection){

selection.remove();

selection=null;

}

});

afficherGardeRobe();

</script>

</body>
</html>
