<script setup>
import { ref, computed } from "vue";
const perguntas = ref([
  {
    pergunta: "Meu nome é?",
    resposta: 0,
    opcoes: [
      "Luiz Nakaoshi Junior",
      "Abigail Thorest Seixas",
      "Nicholas Santos Nascimento",
      "Um Nome Aí",
    ],
    escolhido: null,
  },

  {
    pergunta: "Pelo Sobrenome você diz que eu sou descendente de:",
    resposta: 1,
    opcoes: ["Tailandes", "Japonês", "Coreano", "Chinês"],
    escolhido: null,
  },

  {
    pergunta: "Eu gosto de trabalhar com:",
    resposta: 2,
    opcoes: ["Back-End", "Fullstack", "Front-End", "DevOps"],
    escolhido: null,
  },
  {
    pergunta: "Ferramenta Favorita é:",
    resposta: 3,
    opcoes: ["React", "Angular", "Ember", "Vue", "JQuery"],
    escolhido: null,
  },

  {
    pergunta: "Você Tinha a Obrigação de saber?",
    resposta: 2,
    opcoes: ["Não", "Lógico", "Sim", "Só fiz pra perder tempo mesmo"],
    escolhido: null,
  },
]);

const questionarioCompletado = ref(false);
const perguntaAtual = ref(0);
const pontuacao = computed(() => {
  let valor = 0;
  perguntas.value.map((p) => {
    if (p.escolhido == p.resposta) {
      valor++;
    }
  });
  return valor;
});
const pegaPerguntaAtual = computed(() => {
  let questão = perguntas.value[perguntaAtual.value];
  questão.index = perguntaAtual.value;
  return questão;
});

const SetResposta = (e) => {
  perguntas.value[perguntaAtual.value].escolhido = e.target.value;
  e.target.value = null;
};

const ProximaPergunta = () => {
  if (perguntaAtual.value < perguntas.value.length - 1) {
    perguntaAtual.value++;
    return;
  }

  questionarioCompletado.value = true;
};
</script>

<template>
  <main class="app">
    <h1>Questionario</h1>

    <section class="questionario" v-if="!questionarioCompletado">
      <div class="questionario__pergunta">
        <span class="pergunta">{{ pegaPerguntaAtual.pergunta }}</span>
        <span class="pontuacao">
          pontuação {{ pontuacao }} / {{ perguntas.length }}</span
        >
      </div>

      <div class="opcoes">
        <label
          v-for="(opcao, index) in pegaPerguntaAtual.opcoes"
          :for="'opcao' + index"
          :key="index"
          :class="`opcao ${
						pegaPerguntaAtual.escolhido == index 
							? index == pegaPerguntaAtual.resposta 
								? 'correto' 
								: 'incorreto'
							: ''
					} ${
						pegaPerguntaAtual.escolhido != null &&
						index != pegaPerguntaAtual.escolhido
							? 'desabilitado'
							: ''
					}`">
        <input 
						type="radio" 
						:id="'opcao' + index" 
						:name="pegaPerguntaAtual.index" 
						:value="index" 
						v-model="pegaPerguntaAtual.escolhido" 
						:disabled="pegaPerguntaAtual.escolhido"
						@change="SetResposta" 
					/>

          <span>{{ opcao }}</span>
        </label>
      </div>

      <button @click="ProximaPergunta" :disabled="!pegaPerguntaAtual.escolhido">
        {{
          pegaPerguntaAtual.index == perguntas.length - 1
            ? "Acabou"
            : pegaPerguntaAtual.escolhido == null
            ? "escolha uma opção"
            : "Proxima Pergunta"
        }}
      </button>
    </section>

    <section v-else>
        <h2>Você Terminou o questionario!!!</h2>
        <p>Sua Pontuação é {{pontuacao}} / {{perguntas.length}}</p>
        <h3>caso tenha acertado entre em contato comigo!</h3>
        <h4>juniornakaoshi@outlook.com</h4>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: 0;
  font-family: "Montserrat", sans-serif;
}
body {
  background-color: #222831;
  color: #fafafa;
}
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  height: 100vh;
}
h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}
.questionario {
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
}
.questionario__pergunta{
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}
.questionario__pergunta .question {
  color: #8f8f8f;
  font-size: 1.25rem;
}
.questionario__pergunta .score {
  color: #fff;
  font-size: 1.25rem;
}
.opcoes {
  margin-bottom: 1rem;
}
.opcao {
  padding: 1rem;
  display: block;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}
.opcao:hover {
  background-color: #2d213f;
}
.opcao .correct {
  background-color: #2cce7d;
}
.opcao .wrong {
  background-color: #ff5a5f;
}
.opcao:last-of-type {
  margin-bottom: 0;
}
.opcao .disabled {
  opacity: 0.5;
}
.opcao input {
  display: none;
}
button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
  border-radius: 0.5rem;
}
button:disabled {
  opacity: 0.5;
}
h2{
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
h3{
  font-size:1.5rem;
  margin-bottom:1.75rem;
  text-align:center;
}
h4{
  font-size:1.25rem;
  margin-bottom:1.45rem;
  text-align:center;
}
p {
  color: #8f8f8f;
  font-size: 1.5rem;
  margin-bottom:1.35rem;
  text-align: center;
}
</style>
