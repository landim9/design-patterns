# State - Design Patterns

O State é um padrão de projeto comportamental que permite que um objeto altere seu comportamento quando seu estado interno muda. Parece como se o objeto mudasse de classe.

Ele encapsula estados distintos em objetos separados e delega as responsabilidades associadas a cada estado a esses objetos. Isso torna o comportamento de um objeto dependente do seu estado atual e facilita a adição de novos estados sem alterar a lógica do objeto.

![Texto Alternativo](https://refactoring.guru/images/patterns/content/state/state-pt-br-2x.png)

## Funcionamento 

1.Quando uma operação é chamada no contexto, ele delega a chamada para o objeto estado atual.

2.O objeto estado atual executa a operação apropriada.

3.Se necessário, o objeto estado atual pode modificar o estado do contexto chamando um método no próprio contexto para alterar sua referência de estado.

4.O ciclo continua conforme novas operações são chamadas no contexto.

## Implementação em JavaScript

```
// Interface para definir os métodos de ação do State
class State {
    play() {}
    pause() {}
    stop() {}
}

// Classe concreta representando o estado "tocando"
class PlayingState extends State {
    play() {
        console.log("O player já está tocando uma música.");
    }

    pause() {
        console.log("Pausando a música.");
        return new PausedState();
    }

    stop() {
        console.log("Parando a música.");
        return new StoppedState();
    }
}

// Classe concreta representando o estado "pausado"
class PausedState extends State {
    play() {
        console.log("Retomando a música.");
        return new PlayingState();
    }

    pause() {
        console.log("A música já está pausada.");
    }

    stop() {
        console.log("Parando a música.");
        return new StoppedState();
    }
}

// Classe concreta representando o estado "parado"
class StoppedState extends State {
    play() {
        console.log("Iniciando a música.");
        return new PlayingState();
    }

    pause() {
        console.log("Não é possível pausar, a música está parada.");
    }

    stop() {
        console.log("A música já está parada.");
    }
}

// Classe do player de música que mantém o estado atual
class MusicPlayer {
    constructor() {
        this.state = new StoppedState(); // Inicializa com o estado "parado"
    }

    changeState(state) {
        this.state = state;
    }

    play() {
        this.state.play();
    }

    pause() {
        this.state = this.state.pause();
    }

    stop() {
        this.state = this.state.stop();
    }
}

// Uso do player de música
const player = new MusicPlayer();
player.play();  // Inicia a música
player.pause(); // Pausa a música
player.play();  // Retoma a música
player.stop();  // Para a música
player.stop();  // Tenta parar a música novamente

```

## Analogia com o mundo real

Os botões e interruptores de seu smartphone comportam-se de forma diferente dependendo do estado atual do dispositivo:

- Quando o telefone está desbloqueado, apertar os botões leva eles a executar várias funções.

- Quando o telefone está bloqueado, apertar qualquer botão leva a desbloquear a tela.

- Quando a carga da bateria está baixa, apertar qualquer botão mostra a tela de carregamento.