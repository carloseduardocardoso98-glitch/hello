o respeitoeco é um programa que tentar descobrir se a pessoa possuir capacidade de entender importância do meio ambiente. um programa foi feito utilizando IA, mas foi possível aprender sobre alguns import como FreeSimpleGUI, time e random



import FreeSimpleGUI as sg
import time
import random

# ---- Parte 1: Splash Screen ----
def splash_screen():
    sg.change_look_and_feel('GreenTan')
    layout = [[sg.Text("Aprenda e salve o planeta jogando!",
                       size=(40, 2), font=("Helvetica", 18),
                       text_color='black', justification='center')]]
    window = sg.Window("RespeitoEco", layout, no_titlebar=True,
                       keep_on_top=True, alpha_channel=0.95, grab_anywhere=True)
    start_time = time.time()
    while True:
        event, values = window.Read(timeout=100)
        if time.time() - start_time > 2.5:
            break
    window.Close()

# ---- Parte 7: Banco de 40 perguntas ----
perguntas = [
    {"pergunta": "Por que o meio ambiente é essencial para a vida na Terra?",
     "opcoes": {"A": "Porque fornece apenas energia elétrica",
                "B": "Porque garante os recursos naturais necessários para a vida",
                "C": "Porque é um conceito filosófico sem impacto real",
                "D": "Porque apenas os animais dependem dele"},
     "resposta_correta": "B"},
    {"pergunta": "O que é biodiversidade?",
     "opcoes": {"A": "Variedade de espécies em um ecossistema",
                "B": "Quantidade de água nos rios",
                "C": "A poluição do ar",
                "D": "A temperatura média da Terra"},
     "resposta_correta": "A"},
    {"pergunta": "Qual das seguintes ações contribui para a preservação do meio ambiente?",
     "opcoes": {"A": "Jogar lixo nas ruas",
                "B": "Plantar árvores nativas",
                "C": "Queimar florestas para abrir áreas de pasto",
                "D": "Desperdiçar água"},
     "resposta_correta": "B"},
    {"pergunta": "O desmatamento causa principalmente:",
     "opcoes": {"A": "Redução da biodiversidade e mudanças climáticas",
                "B": "Aumento da biodiversidade",
                "C": "Redução do calor global",
                "D": "Aumento de rios e lagos"},
     "resposta_correta": "A"},
    {"pergunta": "Por que a poluição do ar é um problema ambiental?",
     "opcoes": {"A": "Porque não afeta os seres humanos",
                "B": "Porque causa problemas de saúde e mudanças climáticas",
                "C": "Porque aumenta a biodiversidade",
                "D": "Porque enriquece o solo"},
     "resposta_correta": "B"},
    {"pergunta": "O efeito estufa é causado principalmente pelo:",
     "opcoes": {"A": "Excesso de oxigênio",
                "B": "Acúmulo de gases como CO₂ e metano na atmosfera",
                "C": "Crescimento da população de plantas",
                "D": "Aumento da biodiversidade"},
     "resposta_correta": "B"},
    {"pergunta": "Reciclar materiais é importante porque:",
     "opcoes": {"A": "Aumenta a quantidade de lixo nos aterros",
                "B": "Reduz a necessidade de exploração de novos recursos",
                "C": "Gera poluição atmosférica",
                "D": "Diminui a qualidade da água"},
     "resposta_correta": "B"},
    {"pergunta": "Qual é o principal objetivo de um parque nacional?",
     "opcoes": {"A": "Construir prédios turísticos",
                "B": "Proteger espécies e ecossistemas naturais",
                "C": "Desmatar áreas para agricultura",
                "D": "Explorar recursos minerais"},
     "resposta_correta": "B"},
    {"pergunta": "O que significa sustentabilidade?",
     "opcoes": {"A": "Usar recursos naturais sem comprometer o futuro",
                "B": "Consumir todos os recursos disponíveis imediatamente",
                "C": "Evitar o uso de tecnologia",
                "D": "Não plantar árvores"},
     "resposta_correta": "A"},
    {"pergunta": "A água é considerada um recurso ambiental vital porque:",
     "opcoes": {"A": "É infinita e não precisa ser preservada",
                "B": "Sustenta a vida de todos os seres vivos e ecossistemas",
                "C": "É apenas um recurso industrial",
                "D": "Não é necessária para os animais terrestres"},
     "resposta_correta": "B"},
    # ... continue até a pergunta 40 seguindo o mesmo padrão
]

# ---- Seleção de 10 questões aleatórias ----
def selecionar_questoes():
    return random.sample(perguntas, 10)

# ---- Parte 4: Tela de perguntas ----
def jogo_perguntas(questoes):
    sg.change_look_and_feel('GreenTan')
    total_questoes = len(questoes)
    indice = 0
    respostas_do_aluno = [None] * total_questoes

    while True:
        q = questoes[indice]
        layout = [
            [sg.Text(f"Questão {indice + 1}/{total_questoes}", font=("Helvetica", 16), text_color='black')],
            [sg.Text(q['pergunta'], font=("Helvetica", 14), text_color='black', size=(50, 4))],
            [sg.Button(f"A) {q['opcoes']['A']}", size=(50, 1))],
            [sg.Button(f"B) {q['opcoes']['B']}", size=(50, 1))],
            [sg.Button(f"C) {q['opcoes']['C']}", size=(50, 1))],
            [sg.Button(f"D) {q['opcoes']['D']}", size=(50, 1))],
        ]

        window = sg.Window("RespeitoEco - Perguntas", layout)
        event, values = window.Read()
        window.Close()

        if event == sg.WIN_CLOSED:
            break
        elif event.startswith(("A", "B", "C", "D")):
            letra = event[0]
            respostas_do_aluno[indice] = letra
            if letra == q['resposta_correta']:
                sg.Popup("Acertou!", title="Feedback", text_color='black', background_color='green')
            else:
                sg.Popup(f"Errou! Resposta correta: {q['resposta_correta']}", title="Feedback", text_color='black', background_color='green')

            if indice < total_questoes - 1:
                indice += 1
            else:
                break

    return respostas_do_aluno

# ---- Parte 5: Tela de pontuação final ----
def tela_pontuacao_final(questoes, respostas_do_aluno):
    sg.change_look_and_feel('GreenTan')
    total_acertos = sum(1 for i, q in enumerate(questoes) if respostas_do_aluno[i] == q['resposta_correta'])
    total_questoes = len(questoes)
    total_erros = total_questoes - total_acertos
    mensagem = ("Você precisa entender melhor o meio ambiente, por favor, tente novamente."
                if total_acertos <= 5 else "Você entende bem o meio ambiente, parabéns.")
    layout = [
        [sg.Text("Pontuação Final", font=("Helvetica", 20), text_color='black')],
        [sg.Text(f"Acertos: {total_acertos} / {total_questoes}", font=("Helvetica", 16), text_color='black')],
        [sg.Text(f"Erros: {total_erros} / {total_questoes}", font=("Helvetica", 16), text_color='black')],
        [sg.Text(mensagem, font=("Helvetica", 14), text_color='black', size=(50, 3))],
        [sg.Button("Fazer novamente", size=(20, 2)), sg.Button("Sair do jogo", size=(20, 2))]
    ]
    window = sg.Window("RespeitoEco - Resultado Final", layout)
    while True:
        event, values = window.Read()
        if event == sg.WIN_CLOSED or event == "Sair do jogo":
            window.Close()
            return False
        elif event == "Fazer novamente":
            window.Close()
            return True

# ---- Parte 2: Menu Principal ----
def menu_principal():
    sg.change_look_and_feel('GreenTan')
    layout = [
        [sg.Text("RespeitoEco - Menu Principal", font=("Helvetica", 20), text_color='black', justification='center')],
        [sg.Button("Iniciar o jogo", size=(20, 2))],
        [sg.Button("Explicações do jogo", size=(20, 2))],
        [sg.Button("Sair", size=(20, 2))]
    ]
    window = sg.Window("RespeitoEco - Menu Principal", layout)
    while True:
        event, values = window.Read()
        if event == sg.WIN_CLOSED or event == "Sair":
            window.Close()
            break
        elif event == "Iniciar o jogo":
            window.Close()
            while True:
                questoes = selecionar_questoes()
                respostas = jogo_perguntas(questoes)
                if not tela_pontuacao_final(questoes, respostas):
                    break
            menu_principal()
        elif event == "Explicações do jogo":
            window.Close()
            explicacoes_jogo()

# ---- Tela de explicações ----
def explicacoes_jogo():
    layout = [
        [sg.Text("O objetivo do jogo é efetivar uma aprendizagem benéfica sobre a importância do meio ambiente.\n"
                 "As questões são focadas apenas para conscientizar e contêm 10 questões de múltipla escolha para realizar.",
                 font=("Helvetica", 14), text_color='black', justification='center')],
        [sg.Button("Voltar ao Menu Principal", size=(25, 2))]
    ]
    window = sg.Window("Explicações do Jogo", layout)
    while True:
        event, values = window.Read()
        if event == sg.WIN_CLOSED or event == "Voltar ao Menu Principal":
            window.Close()
            menu_principal()
            break

# ---- Execução do App ----
if __name__ == "__main__":
    splash_screen()
    menu_principal()
