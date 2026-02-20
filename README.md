<!DOCTYPE html>
<html lang="ku">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Perfect 3D Dice</title>

<style>
body{
    margin:0;
    height:100vh;
    background:radial-gradient(circle,#222,#000);
    display:flex;
    justify-content:center;
    align-items:center;
    flex-direction:column;
    color:white;
    font-family:Arial;
    perspective:1200px;
}

.scene{
    display:flex;
    gap:100px;
}

.cube{
    width:120px;
    height:120px;
    position:relative;
    transform-style:preserve-3d;
    transition:transform 1.4s cubic-bezier(.17,.67,.83,.67);
}

.face{
    position:absolute;
    width:120px;
    height:120px;
    background:linear-gradient(145deg,#fff,#ddd);
    border-radius:20px;
    display:grid;
    grid-template-columns:repeat(3,1fr);
    grid-template-rows:repeat(3,1fr);
    padding:15px;
    box-shadow:
        inset -5px -5px 10px rgba(0,0,0,0.2),
        inset 5px 5px 10px rgba(255,255,255,0.6),
        0 30px 60px rgba(0,0,0,0.7);
}

.dot{
    width:18px;
    height:18px;
    background:black;
    border-radius:50%;
    align-self:center;
    justify-self:center;
    opacity:0;
}

/* face positions */
.front{ transform:rotateY(0deg) translateZ(60px); }
.back{ transform:rotateY(180deg) translateZ(60px); }
.right{ transform:rotateY(90deg) translateZ(60px); }
.left{ transform:rotateY(-90deg) translateZ(60px); }
.top{ transform:rotateX(90deg) translateZ(60px); }
.bottom{ transform:rotateX(-90deg) translateZ(60px); }

#total{
    margin-top:40px;
    font-size:30px;
}

.fire{
    position:absolute;
    font-size:70px;
    animation:boom 1s ease forwards;
}

@keyframes boom{
    0%{ transform:scale(0.5); opacity:1; }
    100%{ transform:scale(2.5); opacity:0; }
}
</style>
</head>

<body>

<div class="scene">
    <div class="cube" id="cube1"></div>
    <div class="cube" id="cube2"></div>
</div>

<div id="total">کۆ: 2</div>
<p>دەست لە شاشە بدە 🎲</p>

<script>
const patterns={
1:[4],
2:[0,8],
3:[0,4,8],
4:[0,2,6,8],
5:[0,2,4,6,8],
6:[0,2,3,5,6,8]
};

function createFace(num){
    const face=document.createElement("div");
    face.className="face";
    for(let i=0;i<9;i++){
        let dot=document.createElement("div");
        dot.className="dot";
        if(patterns[num].includes(i)) dot.style.opacity=1;
        face.appendChild(dot);
    }
    return face;
}

function createCube(cube){
    cube.innerHTML="";
    cube.appendChild(createFace(1)).classList.add("front");
    cube.appendChild(createFace(6)).classList.add("back");
    cube.appendChild(createFace(3)).classList.add("right");
    cube.appendChild(createFace(4)).classList.add("left");
    cube.appendChild(createFace(5)).classList.add("top");
    cube.appendChild(createFace(2)).classList.add("bottom");
}

const cube1=document.getElementById("cube1");
const cube2=document.getElementById("cube2");
const totalDiv=document.getElementById("total");

createCube(cube1);
createCube(cube2);

function showFire(){
    const fire=document.createElement("div");
    fire.className="fire";
    fire.innerHTML="🔥🔥";
    fire.style.top="40%";
    fire.style.left="45%";
    document.body.appendChild(fire);
    setTimeout(()=>fire.remove(),1000);
}

let spinX1=0, spinY1=0;
let spinX2=0, spinY2=0;

function roll(){

    let n1=Math.floor(Math.random()*6)+1;
    let n2=Math.floor(Math.random()*6)+1;

    const rot={
        1:[0,0],
        2:[90,0],
        3:[0,-90],
        4:[0,90],
        5:[-90,0],
        6:[0,180]
    };

    spinX1 += 720;
    spinY1 += 720;
    spinX2 += 720;
    spinY2 += 720;

    cube1.style.transform =
        `rotateX(${spinX1 + rot[n1][0]}deg)
         rotateY(${spinY1 + rot[n1][1]}deg)`;

    cube2.style.transform =
        `rotateX(${spinX2 + rot[n2][0]}deg)
         rotateY(${spinY2 + rot[n2][1]}deg)`;

    totalDiv.textContent="کۆ: "+(n1+n2);

    if(n1===n2){
        showFire();
    }
}

document.body.addEventListener("click",roll);
document.body.addEventListener("touchstart",roll);
</script>

</body>
</html>
