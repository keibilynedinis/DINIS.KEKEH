let andar = 0;
let comando = "";

while (true) {
  comando = prompt(
    "Você está no andar " + andar +
    "\nDigite:\n1 - Subir\n2 - Descer\n3 - Sair"
  );

  if (comando === "1") {
    andar = andar + 1;
  }

  if (comando === "2") {
    andar = andar - 1;
  }

  if (comando === "3") {
    break;
  }
}

alert("Elevador desligado no andar " + andar);
