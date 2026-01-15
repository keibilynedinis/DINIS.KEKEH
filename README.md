let readline = require("readline");

let rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

// Estado do hotel
let hotel = {};
let contadorHospedes = 0;
let MAX_VISUAL = 20;

// Inicializa o hotel com 5 hóspedes
for (let i = 1; i <= 5; i++) {
  contadorHospedes++;
  hotel[i] = "Hóspede " + contadorHospedes;
}

// Função para mostrar o hotel
function mostrarHotel() {
  console.log("\n=== Hotel Atual ===");
  let quartos = Object.keys(hotel)
    .map(Number)
    .sort(function(a, b){ return a - b; })
    .slice(0, MAX_VISUAL);

  for (let i = 0; i < quartos.length; i++) {
    let q = quartos[i];
    console.log("Quarto " + q + ": " + hotel[q]);
  }

  if (Object.keys(hotel).length > MAX_VISUAL) {
    console.log("... e mais hóspedes não mostrados");
  }
}

// Função para adicionar hóspedes
function adicionarHospedes(quantidade) {
  let novoHotel = {};

  if (quantidade === Infinity) {
    for (let quarto in hotel) {
      novoHotel[quarto * 2] = hotel[quarto];
    }
  } else {
    for (let quarto in hotel) {
      novoHotel[Number(quarto) + quantidade] = hotel[quarto];
    }

    for (let i = 1; i <= quantidade; i++) {
      contadorHospedes++;
      novoHotel[i] = "Hóspede " + contadorHospedes;
    }
  }

  hotel = novoHotel;
}

// Menu interativo
function menu() {
  mostrarHotel();

  console.log("\nEscolha a opção:");
  console.log("1 - Entrar 1 pessoa");
  console.log("2 - Entrar N pessoas");
  console.log("3 - Entrar infinitas pessoas");
  console.log("4 - Ver onde está um hóspede");
  console.log("5 - Sair\n");

  rl.question("> ", function(opcao){
    if (opcao === "1") {
      adicionarHospedes(1);
      menu();
    } else if (opcao === "2") {
      rl.question("Quantas pessoas entram? ", function(n){
        adicionarHospedes(Number(n));
        menu();
      });
    } else if (opcao === "3") {
      adicionarHospedes(Infinity);
      menu();
    } else if (opcao === "4") {
      rl.question("Número do hóspede original: ", function(n){
        let quartoEncontrado = null;
        for (let quarto in hotel) {
          if (hotel[quarto] === "Hóspede " + n) {
            quartoEncontrado = quarto;
            break;
          }
        }
        if (quartoEncontrado) {
          console.log("O hóspede " + n + " está agora no quarto " + quartoEncontrado);
        } else {
          console.log("Hóspede " + n + " não encontrado (provavelmente foi movido além do limite mostrado).");
        }
        menu();
      });
    } else if (opcao === "5") {
      console.log("Hotel fechado.");
      rl.close();
    } else {
      console.log("Opção inválida!");
      menu();
    }
  });
}

menu();
