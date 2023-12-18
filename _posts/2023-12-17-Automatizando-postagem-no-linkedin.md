---
layout: post
title: "Postagem automatizada no linkedin"
categories: misc
---
Sobre o passo inicial em Python

É isso...

Queria muito aprender python de uma maneira que combinasse comigo e que não desanimasse, esse curso "Automate the Boring Stuff with Python
By Al Sweigart" já estava no meu radar desde que dei uma folheada nele há alguns anos atrás na faculdade, então pequei o curso na udemy e mandei ver.
O que me interessou foi poder manipular e ler arquivos no Windows, também automatizar navegação e aquisição de dados na web.
Tudo de uma forma iniciante, claro, mas com um número bom de desafios daqueles que te ensinam na marra.
Olhando agora, foi divertido e valeu a pena.

Escrevi um script que logou no meu Linkedin, pegou o certificado que tinha salvo no meu PC, escreveu a mensagem da postagem e postou.
Tá aqui a [postagem](https://www.linkedin.com/feed/update/urn:li:activity:7133694396247490560/).

Quem quiser ver o código, tá aí (eu substituí a senha por "x" pra evitar espertinhos):

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.action_chains import ActionChains

# Credenciais
email = "danilo.molero@outlook.com"
password = "xxxxxxxxx"

# Instância do navegador
driver = webdriver.Edge()

# Site do LinkedIn
driver.get("https://www.linkedin.com/login")

# Procura o campo de email
email_field = driver.find_element(By.ID, "username")
email_field.send_keys(email)

# Procura o campo de senha
password_field = driver.find_element(By.ID, "password")
password_field.send_keys(password)

# Procura o botão de autenticação
sign_in_button = driver.find_element(By.CSS_SELECTOR, ".btn__primary--large")
sign_in_button.click()

# Clica no botão para postar imagem
driver.get("https://www.linkedin.com/feed")
button_xpath = '/html/body/div[5]/div[3]/div/div/div[2]/div/div/main/div[1]/div[2]/div[3]/button'
button = WebDriverWait(driver, 10).until(
  EC.element_to_be_clickable((By.XPATH, button_xpath))
)
button.click()

# Botão 2
input_file = WebDriverWait(driver, 10).until(
  EC.presence_of_element_located((By.NAME, 'file'))
)
image_path = r'C:\Users\danil\Downloads\certif_01.jpg'
input_file.send_keys(image_path)
second_button_xpath = '/html/body/div[3]/div/div/div/div[2]/div/div/div[2]/div/button'
second_button = WebDriverWait(driver, 10).until(
  EC.element_to_be_clickable((By.XPATH, second_button_xpath))
)
second_button.click()

# Texto
text_area_xpath = '/html/body/div[3]/div/div/div/div[2]/div/div/div[1]/div[1]/div/div/div/div/div/div[1]/p'

text_area = driver.find_element(By.XPATH, text_area_xpath)
text_area.send_keys('''Esta postagem foi totalmente automatizada em Python!

Salve, pessoal do LinkedIn! Acabo de concluir meu curso e decidi inovar: usei minhas habilidades recém-adquiridas para criar um script de automação que gerou esta mensagem e exibiu meu certificado!

(Confira o código nos comentários!)

Recomendo muito esse curso!

#Python #Certificação #Automação #udemy #automatetheboringstuff #alsweigart''')

# Clica no botão para postar
post_button_xpath = "/html/body/div[3]/div/div/d]/div/div[2]"
post_button = WebDriverWait(driver, 10).until(
  EC.element_to_be_clickable((By.XPATH, post_button_xpath))
)
post_button.click()
```
