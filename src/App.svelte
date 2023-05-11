<script lang='ts'>
    let videoSource = null;
    let loading = false;
    const getVideoCam = async () => {
        try {
        loading = true;
        const stream = await navigator.mediaDevices.getUserMedia({
            video: {width: 320, height: 320}
        });
        videoSource.srcObject = stream;
        videoSource.play();
        loading = false;
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
        let loser;
        let winner;
        if (result == 1){
            loser = "ennemy"
            winner = "player"
        }
        else{
            loser = "player"
            winner = "ennemy"
        }
        var canvasPlayerPicture = <HTMLCanvasElement> document.getElementById('player');
        var ctx = canvasPlayerPicture.getContext('2d');
        ctx.drawImage(videoSource, 0, 0);
        document.getElementById(winner+"Win").style.display = "inline";
        document.getElementById(loser).style.width = "55%";
        document.getElementById(loser).style.height = "55%";
        document.getElementById("video").style.display = "none";
        document.getElementById("player").style.display = "inline";
    }

    function endOfGame(){
        /*Je lance la fonction qui pour capturer la caméra de l'utilisateur*/
        document.getElementById("utilityButton").style.display = "inline";
        document.getElementById("utilityText").style.display = "none";
        document.getElementById("utilityButton").innerHTML = "Rejouer";
        let winnerResult = Math.floor(Math.random() * 2);
        winnerFunction(winnerResult);
    }

    function fonctionGame(){
        reinitialised();
        for(let i = 0; i <= 10; i++){
            setTimeout(() => {document.getElementById("utilityText").innerHTML = (10-i).toString()}, i*1000);
        }
        setTimeout(endOfGame, 11000);
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
    }

    p{
        font-size: 200%;
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

</style>