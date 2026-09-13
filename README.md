# 🛡️ Ransomware Simulation with Python

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-blue.svg" alt="Python Version">
  <img src="https://img.shields.io/badge/Cryptography-AES-green.svg" alt="Encryption">
  <img src="https://img.shields.io/badge/License-Educational-orange.svg" alt="License">
</p>

## ⚠️ Aviso Legal (Disclaimer)

> **AVISO:** Este projeto foi desenvolvido **exclusivamente para fins educacionais e de pesquisa em segurança cibernética**. 
> O autor não se responsabiliza pelo uso indevido deste material. Utilizar códigos de criptografia sem autorização explícita em sistemas alheios é ilegal e antiético. O objetivo principal é demonstrar conceitos teóricos de criptografia simétrica e conscientizar profissionais de defesa sobre o funcionamento de ataques de *ransomware*.

---

## 📌 Sobre o Projeto

Este repositório contém uma implementação didática e simplificada de um simulador de *ransomware* escrito em Python. O projeto demonstra o ciclo básico de um ataque simulado em ambiente controlado:
1. **Varredura e Criptografia:** Localização e cifragem de arquivos alvo utilizando o algoritmo **AES (Advanced Encryption Standard)**.
2. **Remoção de Evidências/Originais:** Exclusão segura (ou substituição) dos arquivos originais pós-criptografia.
3. **Descriptografia (Recuperação):** Restauração dos arquivos ao estado original utilizando a chave simétrica correta.

---

## 📂 Estrutura do Repositório

O projeto é composto pelos seguintes arquivos principais:

| Arquivo | Descrição |
| :--- | :--- |
| `encrypter.py` | Script responsável por gerar/carregar a chave de criptografia, cifrar os arquivos-alvo e apagar os originais. |
| `decrypter.py` | Script responsável por ler a chave de segurança e reverter o processo de criptografia nos arquivos. |
| `modelo_phishing.pkl` | Modelo de machine learning (gerenciado via Git LFS) utilitário do escopo do projeto. |
| `teste.txt` | Arquivo de texto padrão utilizado para testes seguros de criptografia e descriptografia. |

---

## 🚀 Como Executar (Ambiente Controlado)

> **Recomendação de Segurança:** Teste este script **apenas** dentro de máquinas virtuais isoladas (Sandboxes) ou containers Docker onde não haja dados importantes.

### Pré-requisitos

Instale a biblioteca necessária:

```bash
pip install pyaes
```

### 1. Executando a Criptografia

Para simular o processo de cifragem no arquivo de teste (`teste.txt`), execute:

```bash
python encrypter.py
```
