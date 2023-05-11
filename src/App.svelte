<script lang='ts'>
    let videoSource = null;
    let loading = false;
    let camera = false;
    const getVideoCam = async () => {
        try {
        loading = true;
        const stream = await navigator.mediaDevices.getUserMedia({
            video: {width: 320, height: 320}
        });
        videoSource.srcObject = stream;
        videoSource.play();
        loading = false;
        camera = true;
        } catch (error) {
        console.log(error);
        }
    };

    getVideoCam();

    function reinitialised(){
        //Fonction pour réinitialiser tous les placements et les dimensions

        document.getElementById("ennemyWin").style.display = "none";
        document.getElementById("playerWin").style.display = "none";

        document.getElementById("player").style.width = "70%";
        document.getElementById("player").style.height = "70%";
        document.getElementById("ennemy").style.width = "70%";
        document.getElementById("ennemy").style.height = "70%";

        document.getElementById("utilityButton").style.display = "none";
        document.getElementById("utilityText").style.display = "inline";

        document.getElementById("player").style.display = "none";
        document.getElementById("video").style.display = "inline";
    }

    function winnerFunction(result){
        // Fonction de mise en valeur du gagnant
        let loser;
        let winner;
        // Je determine mes variables en fonction du perdant ou du gagnant
        if (result == 1){
            loser = "ennemy"
            winner = "player"
        }
        else{
            loser = "player"
            winner = "ennemy"
        }
        // Je prend la capture d'écran et je l'applique sur le canvas
        var canvasPlayerPicture = <HTMLCanvasElement> document.getElementById('player');
        var ctx = canvasPlayerPicture.getContext('2d');
        ctx.drawImage(videoSource, 0, 0);
        // Je fais apparaitre le texte de vistoire du gagnant
        document.getElementById(winner+"Win").style.display = "inline";
        // Je modifie la taille du perdant pour mettre en valeur le gagnant
        document.getElementById(loser).style.width = "50%";
        document.getElementById(loser).style.height = "50%";
        // Je fais apparaitre l'image a la place de la vidéo
        document.getElementById("video").style.display = "none";
        document.getElementById("player").style.display = "inline";
    }

    function endOfGame(){
        // Fonction pour determiner le gagnant et le mettre en valeur

        // Je fais réapparaitre le bouton pour rejouer
        document.getElementById("utilityButton").style.display = "inline";
        document.getElementById("utilityText").style.display = "none";
        document.getElementById("utilityButton").innerHTML = "Rejouer";
        // Je détermine le gagnant aléatoirement
        let winnerResult = Math.floor(Math.random() * 2);
        // Je lance la fonction pour capturer la caméra de l'utilisateur
        winnerFunction(winnerResult);
    }

    function fonctionGame(){
        // Finction verifiant que la caméra est bien allumée et qui attend les 10 seconds
        if (camera){
            reinitialised();
            for(let i = 0; i <= 10; i++){ 
                setTimeout(() => {document.getElementById("utilityText").innerHTML = (10-i).toString()}, i*1000);
            }
            setTimeout(endOfGame, 11000);
        }
    }
</script>
<canvas id="redBackground"/>
<canvas id="blueBackground"/>
<header>
    <h1>Staring Contest</h1>
</header>
<main>
    <div class="wrapper" id="wrapperPrincipal">
        <div class="boxTextWinEnnemy">
            <p class="winnerText" id="ennemyWin">
                Winner !
            </p>
        </div>
        <div class="utilityBox" id="utilityDivText">
            <p id="utilityText">
                10
            </p>
            <button id="utilityButton" on:click = {fonctionGame}>
                Start !
            </button>
        </div>
        <div class="boxTextWinPlayer">
            <p class="winnerText" id="playerWin">
                Winner !
            </p>
        </div>
        <div class="ennemyDisplay">
            <img src = "../images/BG_stare.png" alt = "Me, looking at you." id="ennemy">
        </div>
        <div class="playerDisplay">
            <video id = "video" bind:this={videoSource} width=50 height=50 autoplay muted></video>
            <canvas id = "player" height="320" width="320">Your browser doesn't support  </canvas>
        </div>
    </div>
</main>

<style>
/* @media screen and (min-width:0px){
    #blueBackground{
        position: absolute ;
        background: #535bf2;
        left: 0;
        top: 800px;
        height: 100%; 
        width: 100%;
        z-index:-1;
    }
    #redBackground{
        position: absolute ;
        background: #f25353;
        left:0;
        top:0;
        height: 50%; 
        width: 100%;
        z-index:-1;
    }

    header{
        margin-top: auto;
        font-size:25px;
    }

    p{
        font-size: 70px;
        display: none;
    }

    .wrapper{
        align-items : center;
        display: grid;
        grid-template-columns: 50% 50%;
        grid-template-rows: 45% 10% 45%;
    }

    .utilityBox {
        grid-column: 1/3;
        grid-row-start: 2;
        position: center;
        margin-top: 200px;
    }

    .ennemyDisplay{
        grid-column: 1;
        grid-row-start: 1;
        position: relative;
    }

    .boxTextWinPlayer{
        grid-column: 2;
        grid-row-start: 3;
        position: relative;
    }
    .boxTextWinEnnemy{
        grid-column: 2;
        grid-row-start: 1;
        position: relative;
    }

    button {
        border-radius: 8px;
        border: 5px solid transparent;
        padding: 0.6em 1.2em;
        font-size: 40px;
        font-weight: 500;
        font-family: inherit;
        background-color: #1a1a1a;
        cursor: pointer;
        transition: border-color 0.25s;
        }
    .playerDisplay{
        grid-column: 1;
        grid-row-start: 3;
        position: relative;
    }

    video{
        height: 70%; 
        width: 70%;
    }

    img{
        margin-top: 30px;
        height: 70%; 
        width: 70%;
    }

    #player{
        display: none;
        height: 70%; 
        width: 70%;
    }

    #utilityText{
        display: none;
    }
} */
@media screen and (min-width:1000px){
    #blueBackground{
        position: absolute ;
        background: #535bf2;
        left:50%;
        top:0;
        height: 100%; 
        width: 50%;
        z-index:-1;
    }
    #redBackground{
        position: absolute ;
        background: #f25353;
        left:0;
        top:0;
        height: 100%; 
        width: 50%;
        z-index:-1;
    }

    header{
        margin-top: auto;
        font-size:40px;
    }

    p{
        font-size: 90px;
        display: none;
    }

    .wrapper{
        align-items : center;
        display: grid;
        grid-template-columns: 45% 5% 5% 45%;
        grid-template-rows: 20% 80%;
    }

    .utilityBox {
        grid-column: 2/4;
        grid-row-start: 1;
        position: center;
    }

    .ennemyDisplay{
        grid-column: 1/3;
        grid-row-start: 2;
        position: relative;
    }

    .boxTextWinPlayer{
        grid-column: 4;
        grid-row-start: 1;
        position: relative;
    }

    button {
        border-radius: 8px;
        border: 5px solid transparent;
        padding: 0.6em 1.2em;
        font-size: 15px;
        font-weight: 500;
        font-family: inherit;
        background-color: #1a1a1a;
        cursor: pointer;
        transition: border-color 0.25s;
        }
    .playerDisplay{
        grid-column: 3/5;
        grid-row-start: 2;
        position: relative;
    }

    video{
        height: 70%; 
        width: 70%;
    }

    img{
        height: 70%; 
        width: 70%;
    }

    #player{
        display: none;
    }

    #utilityText{
        display: none;
    }
}

@media screen and (min-width:1190px){
    button {
        border-radius: 8px;
        border: 5px solid transparent;
        padding: 0.6em 1.2em;
        font-size: 20px;
        font-weight: 500;
        font-family: inherit;
        background-color: #1a1a1a;
        cursor: pointer;
        transition: border-color 0.25s;
    }
}
</style>