# cifrador-cesar

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cifrador</title>
    <link rel="stylesheet" href="index.css">
    <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>🔐</text></svg>">
    <script src="index.js" type="text/javascript" defer></script>
</head>
<body>
    <main>
    <h1 class="titulo">Código secreto</h1>
     <form id ="cifrador" autocomplete="off">
       <input id="input-original" spellcheck="false" placeholder="Ingresa Texto" type="text" name="text">
     </form>
     <div id="resultado"></div>
     <div class="rango">
        <input id="rango" type="range" value="10" min="1" max="20" oninput="this.nexElementSibling.value = this.value">
        <output>10</output>
    </div>
     </main> 
</body>
</html>
</html>

@import url('https://fonts.googleapis.com/css2?family=Source+Code+Pro:wght@500&display=swap');

:root {
    --color-terminal: #292b36;
    --borde-redondo: 4px;
}

* {
    font-family: 'Source Code Pro', monospace;
}

body {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #5c4b7a;
    color: white;
}

input, button {
    border: none;
    outline: none;
}

main {
    padding: 25px;
    box-shadow: 8px 8px 35px 0px rgb(0 0 0 / 75%);
    border-radius: var(--borde-redondo);
}

h1 {
    margin: 0;
}

#input-original, #resultado {
    background-color: var(--color-terminal);
    width: -webkit-fill-available;
}

#input-original {
    margin: 10px 0 0;
    padding: 10px;
    color: white;
    border-top-left-radius: var(--borde-redondo);
    border-top-right-radius: var(--borde-redondo);
}

#resultado {
    height: 20px;
    color: #5af78d;
    padding: 0 10px 10px;
    border-bottom-left-radius: var(--borde-redondo);
    border-bottom-right-radius: var(--borde-redondo);
}

.rango {
    margin-top: 10px;
    display: grid;
    grid-template-columns: 1fr 30px;
    text-align: center;
    align-items: center;
}

input[type='range'] {
    -webkit-appearance: none;
    border-radius: var(--borde-redondo);
    background:var(--color-terminal);
    margin: 0;
    padding: 0 5px;
    height: 20px;
}

input[type='range']::-webkit-slider-thumb {
    cursor: pointer;
    -webkit-appearance: none;
    background:white;
    border-radius: 50%;
    height:10px;
    width:10px;
}

const alfabeto = ["A","B","C","D","E","F","G","H","I","J","K","L","M","N","Ñ","O","P","Q","R","S","T","U","V","W","X","Y","Z"];
const inputOriginal = document.getElementById('input-original');
const cifrador = document.getElementById('cifrador');
const resultado = document.getElementById('resultado');
const rango = document.getElementById('rango');

const shifMessage = () => {
    const wordArray = [...inputOriginal.value.toUpperCase()];
    printChar(0, wordArray);
}

const printChar = (currentLetterIndex, wordArray) => {
    if(wordArray.length === currentLetterIndex) return;
    inputOriginal.value = inputOriginal.value.substring(1)
    const spanChar = document.createElement("span");
    resultado.appendChild(spanChar);
    animateChar(spanChar)
        .then( () => {
            const charSinCodificar = wordArray[currentLetterIndex];
            spanChar.innerHTML = alfabeto.includes(charSinCodificar) ? 
                alfabeto[(alfabeto.indexOf(charSinCodificar) + parseInt(rango.value)) % alfabeto.length] : 
                charSinCodificar
            printChar(currentLetterIndex + 1, wordArray);
        });
}

const animateChar = spanChar => {
    let cambiosDeLetra = 0;
    return new Promise(resolve => {
        const intervalo = setInterval(() => {
            spanChar.innerHTML = alfabeto[Math.floor(Math.random() * alfabeto.length)];
            cambiosDeLetra++;
            if(cambiosDeLetra === 3) {
                clearInterval(intervalo);
                resolve();
            }
        }, 50);
    });
}

const submit = e => {
    e.preventDefault();
    resultado.innerHTML = '';
    shifMessage()
}

cifrador.onsubmit = submit;


