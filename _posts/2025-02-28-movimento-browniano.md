---
author: danilomolero
comments: true
date: 2025-02-28
layout: post
title: Movimento Browniano, Simulações Visuais e o Medo da Mudança
---

##Voltei.
Muita coisa mudou.
Comigo e com o mundo.
E IA está envolvida nesse processo (meu e do mundo).
A mudança é a constante, mas que medo!

Gosto de simular aleatoriedade e usar programação para isso.
###Movimento Browniano.

Quem me falou disso foi um grande professor anos atrás.
Como será que ele está agora?

Sim, também adoro simulações visuais, quem não gosta?

Aleatoriedade, irregularidade.

É como se contivéssemos um gênio numa garrafa.
Mesmo que seja um geninho e uma garrafinha, já tá ótimo.
Estava estudando sobre movimento browniano e fiz isso:

python
Copy
Edit
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation

tamanho_quadro = 10
n_passos = 500
tamanho_passo = 0.5

posicao = np.array([tamanho_quadro / 2, tamanho_quadro / 2])
posicoes_x = [posicao[0]]
posicoes_y = [posicao[1]]

def movimento_aleatorio():
    global posicao
    direcao = np.random.choice(['cima', 'baixo', 'esquerda', 'direita'])
    if direcao == 'cima':
        posicao[1] += tamanho_passo
    elif direcao == 'baixo':
        posicao[1] -= tamanho_passo
    elif direcao == 'esquerda':
        posicao[0] -= tamanho_passo
    elif direcao == 'direita':
        posicao[0] += tamanho_passo

    posicao[0] = np.clip(posicao[0], 0, tamanho_quadro)
    posicao[1] = np.clip(posicao[1], 0, tamanho_quadro)

    posicoes_x.append(posicao[0])
    posicoes_y.append(posicao[1])

fig, ax = plt.subplots()
ax.set_xlim(0, tamanho_quadro)
ax.set_ylim(0, tamanho_quadro)
ponto, = ax.plot([], [], 'bo')
linha, = ax.plot([], [], 'r-', alpha=0.6)

def init():
    ponto.set_data([], [])
    linha.set_data([], [])
    return ponto, linha

def animate(i):
    movimento_aleatorio()
    ponto.set_data([posicoes_x[-1]], [posicoes_y[-1]])
    linha.set_data(posicoes_x, posicoes_y)
    return ponto, linha

ani = animation.FuncAnimation(fig, animate, frames=n_passos, init_func=init,
                              interval=50, blit=True)

plt.title("Simulação de Movimento Browniano")
plt.xlabel("Eixo X")
plt.ylabel("Eixo Y")
plt.grid(True)
plt.show()

É divertido corrigir os próprios erros.
Seja na programação ou não.
