---
author: danilomolero
comments: true
date: 2023-12-18
layout: post
title: Pêndulo Duplo
---
## Caos.
É uma palavra que a gente encontra sempre em muitos contextos diferentes, mas na física ela me foi apresentada por um professor através do problema do pêndulo duplo. 

<div style="text-align:center">
  <img src="/assets/doublependulumtenor.gif" width="300" height="300">
</div>

É um modelo simples até, mas que mostra uma frestinha para algo muito complexo.

Para entender, normalmente eu começo pelo:

1. Pêndulo Único:
   - Imagine um pêndulo simples, como um balanço no parque. É tipo uma bola presa a uma corda que oscila quando empurrada. O Movimento dele é simples e conhecido; depende da massa, da gravidade e do comprimento.

2. Introduzindo um Segundo Pêndulo:
   - Agora, adicione outro pêndulo idêntico, pendurado abaixo do primeiro. Você tem dois pêndulos, como um de dois andares.

3. Inicializando o Movimento:
   - Se você impulsionar o pêndulo inferior, ele afetará o movimento do superior devido à conexão. ELES ESTÂO CONECTADOS, ENTÂO ISSO MUDA TUDO. À todo momento um está afetando o outro, isso é o coração do problema.

4. Caos e Sensibilidade:
   - Aqui está outro ponto crucial: pequenas variações na forma como inicia os pêndulos resultam em movimentos completamente distintos. O pêndulo duplo é extremamente sensível às condições iniciais.

5. Movimento Caótico:
   - Com o tempo, o movimento se torna altamente complexo e imprevisível, sem um padrão definido (OU SEJA, NÃO É FÁCIL DESCREVER UMA FUNÇÃO MATEMÁTICA PARA O MOVIMENTO, NO MÁXIMO ALGUMAS PREVISÕES LIMITADAS)

#### E é por isso que esse problema é especial, sua imprevisibilidade vem do fato que as condicoes de movimento mudam a todo momento, e dependem da condição anterior, onde cada pendulo depende de parâmetros um do outro.


## Uma modelagem que fiz em python do pendulo duplo:

<div style="text-align:center">
  <img src="/assets/double_pendulum_animation.gif">
</div>

### Segue o Código:
```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from scipy.integrate import odeint
import matplotlib.animation as animation 

# Constantes
g = 9.81  # Gravidade
l1 = 1.25  # Comprimento do primeiro pêndulo
l2 = 1.0  # Comprimento do segundo pêndulo
m1 = 1.0  # Massa do primeiro pêndulo
m2 = 1.25  # Massa do segundo pêndulo

# Configurações de tempo
dt = 0.05  # Passo de tempo
t = np.arange(0, 40, dt)  # Array de tempo

# Condições iniciais (ligeiramente diferentes para induzir caos)
theta1 = np.pi / 2
theta2 = np.pi / 2
omega1 = 0.0
omega2 = 0.01  # Pequena diferença na velocidade angular inicial

def equacoes(y0, t, l1, l2, m1, m2):
    theta1, z1, theta2, z2 = y0
    c, s = np.cos(theta1 - theta2), np.sin(theta1 - theta2)

    theta1_ponto = z1
    z1_ponto = (m2 * g * np.sin(theta2) * c - m2 * s * (l1 * z1 ** 2 * c + l2 * z2 ** 2) -
              (m1 + m2) * g * np.sin(theta1)) / l1 / (m1 + m2 - m2 * c ** 2)

    theta2_ponto = z2
    z2_ponto = ((m1 + m2) * (l1 * z1 ** 2 * s - g * np.sin(theta2) + g * np.sin(theta1) * c) +
              m2 * l2 * z2 ** 2 * s * c) / l2 / (m1 + m2 - m2 * c ** 2)

    return theta1_ponto, z1_ponto, theta2_ponto, z2_ponto

# Condição inicial
y0 = [theta1, omega1, theta2, omega2]

# Integra as equações
solucao = odeint(equacoes, y0, t, args=(l1, l2, m1, m2))
theta1, theta2 = solucao[:, 0], solucao[:, 2]

# Converte para coordenadas cartesianas
x1 = l1 * np.sin(theta1)
y1 = -l1 * np.cos(theta1)
x2 = x1 + l2 * np.sin(theta2)
y2 = y1 - l2 * np.cos(theta2)

# Número de posições a manter para o histórico (10 segundos)
tamanho_historico = int(10 / dt)

# Configura o gráfico
fig, ax = plt.subplots()
ax.set_xlim(-2 * (l1 + l2), 2 * (l1 + l2))
ax.set_ylim(-2 * (l1 + l2), 2 * (l1 + l2))
linha, = ax.plot([], [], 'o-', lw=2)
rastro1, = ax.plot([], [], '-', lw=1, color='blue')
rastro2, = ax.plot([], [], '-', lw=1, color='red')

# Função de inicialização
def inicializacao():
    linha.set_data([], [])
    rastro1.set_data([], [])
    rastro2.set_data([], [])
    return linha, rastro1, rastro2

# Função de animação
def animacao(i):
    linha.set_data([0, x1[i], x2[i]], [0, y1[i], y2[i]])

    # Atualiza o rastro para o pêndulo 1
    inicio1 = max(0, i - tamanho_historico)
    rastro1.set_data(x1[inicio1:i], y1[inicio1:i])

    # Atualiza o rastro para o pêndulo 2
    inicio2 = max(0, i - tamanho_historico)
    rastro2.set_data(x2[inicio2:i], y2[inicio2:i])

    return linha, rastro1, rastro2


# cria a animacao
ani = FuncAnimation(fig, animacao, frames=len(t), init_func=inicializacao, blit=True, interval=25)

# especifica o nome do arquivo de animacao
gif_filename = "double_pendulum_animation.gif"
gif_writer = animation.PillowWriter(fps=30)  # Use the PillowWriter for GIF format

# salva GIF
ani.save(gif_filename, writer=gif_writer)

# mostra a animacao no final
plt.show()

```
