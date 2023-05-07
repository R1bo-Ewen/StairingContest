<script lang='ts'>
    let videoSource = null;
    let loading = false;
    const obtenerVideoCamara = async () => {
        try {
        loading = true;
        const stream = await navigator.mediaDevices.getUserMedia({
            video: true,
        });
        videoSource.srcObject = stream;
        videoSource.play();
        loading = false;
        } catch (error) {
        console.log(error);
        }
    };

    obtenerVideoCamara();

    /*Je lance la fonction qui pour capturer la caméra de l'utilisateur*/
    function endOfGame(){
        document.getElementById("buttonBoxId").style.display = "block";
        document.getElementById("utilityDivText").style.display = "none";
        document.getElementById("utilityButton").innerHTML = "Rejouer";
        let winner = Math.floor(Math.random() * 2);
        if (winner == 1){
            document.getElementById("enemy").style.height = "84%";
            document.getElementById("enemy").style.width = "84%";
            document.getElementById("video").style.height = "110%";
            document.getElementById("video").style.width = "111%";
            document.getElementById("playerWin").style.display = "block";
            document.getElementById("video").style.display = "none";
            document.getElementById("endPicture").style.display = "block";
        }
        else{
            document.getElementById("video").style.height = "84%";
            document.getElementById("video").style.width = "84%";
            document.getElementById("enemy").style.height = "90%";
            document.getElementById("enemy").style.width = "90%";
            document.getElementById("enemyWin").style.display = "block";
            document.getElementById("video").style.display = "none";
            document.getElementById("endPicture").style.display = "block";
        }
    }

    function fonctionGame(){
        document.getElementById("video").style.height = "100%";
        document.getElementById("video").style.width = "101%";
        document.getElementById("enemy").style.height = "100%";
        document.getElementById("enemy").style.width = "84%";
        document.getElementById("enemyWin").style.display = "none";
        document.getElementById("playerWin").style.display = "none";
        document.getElementById("buttonBoxId").style.display = "none";
        document.getElementById("utilityDivText").style.display = "block";
        document.getElementById("endPicture").style.display = "none";
        document.getElementById("video").style.display = "block";
        for(let i = 0; i <= 10; i++){
            setTimeout(() => {document.getElementById("utilityText").innerHTML = (10-i).toString()}, i*1000);
        }
        setTimeout(endOfGame, 11000);
    }
</script>
    

<header>
    <h1>Staring Contest</h1>
</header>
<main>
    <div class="wrapper">
        <div class="box1">
            <p class="winnerText" id="enemyWin">
                Winner !
            </p>
        </div>
        <div class="box3">
            <p class="winnerText" id="playerWin">
                Winner !
            </p>
        </div>
        <div class="box4">
            <img src = "../images/BG_stare.png" alt = "Me, looking at you." id="enemy">
        </div>
        <div class="counter" id="utilityDivText">
            <p id="utilityText">
                10
            </p>
        </div>
        <div class="buttonBox" id="buttonBoxId">
            <button id="utilityButton" on:click = {fonctionGame}>
                Start !
            </button>
        </div>
        <div class="box6">
            <video id = "video" bind:this={videoSource} autoplay muted></video>
            <img src = "" alt="Picture of you winning." id="endPicture">
        </div>
    </div>
</main>
<style>

    header{
        margin-top: auto;
    }

    .winnerText{
        font-size: 200%;
        display: none;
    }

    .wrapper{
        align-items : center;
        display: grid;
        grid-template-columns: 45% 10% 45%;
        grid-template-rows: 20% 80%;
    }

    .buttonBox {
        grid-column: 2;
        grid-row-start: 2;
        position: relative;
    }

    .counter {
        grid-column: 2;
        grid-row-start: 2;
        position: relative;
        display: none;
    }

    .box4{
        grid-column: 1;
        grid-row-start: 2;
        position: relative;
    }

    .box3{
        grid-column: 3;
        grid-row-start: 1;
        position: relative;
    }

    .box6{
        grid-column: 3;
        grid-row-start: 2;
        position: relative;
    }

    video{
        height: 100%; 
        width: 101%;
    }

    img{
        height: 100%; 
        width: 84%;
    }
    #endPicture{
        display: none;
    }
</style>