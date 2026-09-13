# 🛡️ Ransomware em Python

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![Criptografia](https://img.shields.io/badge/Criptografia-AES--128%20%7C%20CTR-8250DF)
![Finalidade](https://img.shields.io/badge/Finalidade-Educacional-DB61A2)

Laboratório de segurança cibernética para estudar a criptografia e a recuperação de um arquivo com Python. O projeto demonstra, de forma simplificada, a etapa de cifragem de dados associada a ransomware, utilizando **AES em modo CTR** e a biblioteca **pyaes**.

A implementação atua exclusivamente sobre o arquivo `teste.txt`, no diretório em que os scripts são executados. O foco está na compreensão do código, das operações sobre arquivos e das limitações de uma implementação didática.

> [!WARNING]
> Execute somente em um ambiente de laboratório, com arquivos descartáveis e autorização para realizar o teste. **Os scripts excluem o arquivo de entrada antes de salvar o arquivo de saída.** Uma falha nesse intervalo pode causar perda de dados. Preserve uma cópia do arquivo de teste fora do diretório de execução.

## Objetivos de aprendizagem

- Compreender o uso de criptografia simétrica para cifrar e recuperar dados.
- Trabalhar com leitura e escrita de arquivos em modo binário em Python.
- Observar a sequência de leitura, exclusão e criação de arquivos.
- Relacionar esses comportamentos à análise de atividade suspeita em endpoints.
- Identificar limitações de segurança e confiabilidade em código experimental.

## Estrutura do repositório

| Arquivo | Função |
| --- | --- |
| [`encrypter.py`](encrypter.py) | Lê `teste.txt`, exclui o original e grava o conteúdo cifrado em `teste.txt.ransomwaretroll`. |
| [`decrypter.py`](decrypter.py) | Lê o arquivo cifrado, descriptografa o conteúdo, exclui a entrada e recria `teste.txt`. |
| [`teste.txt`](teste.txt) | Arquivo de exemplo fornecido para o laboratório. |
| [`.gitattributes`](.gitattributes) | Define o uso de Git LFS para arquivos com extensão `.pkl`. |
| [`README.md`](README.md) | Apresentação, instruções de uso e limitações do projeto. |

A configuração de Git LFS não é utilizada pelos dois scripts Python. Não há um modelo de machine learning integrado à implementação atual.

## Como funciona

Os dois scripts utilizam a mesma chave de **16 bytes**, definida diretamente no código, e a classe `pyaes.AESModeOfOperationCTR`. Esse tamanho de chave corresponde ao AES-128.

### Criptografia

O `encrypter.py` realiza as seguintes operações:

1. Abre `teste.txt` em modo binário e carrega todo o conteúdo em memória.
2. Exclui o arquivo original com `os.remove()`.
3. Inicializa o AES em modo CTR com a chave definida no script.
4. Cifra o conteúdo que permanece em memória.
5. Grava o resultado em `teste.txt.ransomwaretroll`.

### Recuperação

O `decrypter.py` realiza o processo complementar:

1. Abre `teste.txt.ransomwaretroll` e lê os dados cifrados.
2. Inicializa o AES em modo CTR com a mesma chave.
3. Descriptografa os dados em memória.
4. Exclui o arquivo cifrado.
5. Grava o conteúdo recuperado em `teste.txt`.

Os nomes dos arquivos são fixos no código. Os scripts não recebem caminhos como argumentos e não percorrem diretórios automaticamente.

## Preparação do ambiente

### Pré-requisitos

- Python 3, com `pip` e suporte ao módulo `venv`.
- Git, para obter o repositório pelos comandos abaixo. Também é possível baixar e extrair o ZIP pelo GitHub.
- Uma máquina virtual de laboratório e um arquivo de teste descartável.

A única dependência externa dos scripts é `pyaes`. O módulo `os` já faz parte da biblioteca padrão do Python.

### 1. Obter o projeto

```bash
git clone https://github.com/bielecky/ransomware-python.git
cd ransomware-python
```

### 2. Criar e ativar um ambiente virtual

**Windows — PowerShell**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**Linux ou macOS — Bash/Zsh**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

O ambiente virtual separa as dependências Python. Ele **não isola o acesso aos arquivos do sistema** e não substitui uma máquina virtual de laboratório.

### 3. Instalar a dependência

Com o ambiente virtual ativo:

```bash
python -m pip install pyaes
```

## Executar o laboratório

Execute os comandos na raiz do projeto, onde estão os scripts e o arquivo de teste. Confirme que `teste.txt` contém apenas dados descartáveis e preserve uma cópia antes de continuar.

### 1. Registrar o hash do arquivo original

O hash permite verificar se os bytes recuperados são iguais aos originais. Escolha o comando do seu sistema e guarde o resultado:

| Sistema | Comando |
| --- | --- |
| Windows — PowerShell | `Get-FileHash .\teste.txt -Algorithm SHA256` |
| Linux | `sha256sum teste.txt` |
| macOS | `shasum -a 256 teste.txt` |

### 2. Criptografar

```bash
python encrypter.py
```

**Resultado esperado em uma execução bem-sucedida:** `teste.txt` deixa de existir e o arquivo `teste.txt.ransomwaretroll` é criado com o conteúdo cifrado.

### 3. Recuperar o conteúdo

```bash
python decrypter.py
```

**Resultado esperado em uma execução bem-sucedida:** `teste.txt.ransomwaretroll` é removido e `teste.txt` é recriado com o conteúdo original.

### 4. Verificar a recuperação

Repita o comando de SHA-256 utilizado antes da criptografia. Os hashes devem ser iguais. A simples recriação de `teste.txt` não confirma que o conteúdo foi recuperado corretamente.

Os scripts não exibem mensagens de sucesso nem geram relatórios. Observe os arquivos e compare os hashes para verificar o resultado.

## Limitações da implementação

| Aspecto | Comportamento atual e implicação |
| --- | --- |
| Chave no código | A chave é fixa e está presente nos dois scripts. Não há geração, armazenamento protegido ou gerenciamento de chaves. |
| Contador do CTR | O construtor é utilizado sem um contador explícito. O projeto não implementa uma estratégia para garantir valores únicos entre operações com a mesma chave. |
| Integridade do conteúdo | Não há autenticação criptográfica nem verificação de integridade embutida. Uma alteração nos dados cifrados pode não ser identificada pelo script de recuperação. |
| Exclusão antes da gravação | Ambos os scripts removem a entrada antes de persistir a saída. Não há backup automático ou mecanismo de recuperação em caso de falha. |
| Exclusão de arquivos | `os.remove()` remove o arquivo; não implementa sobrescrita segura do conteúdo nem limpeza de evidências. |
| Tratamento de erros | Não há tratamento explícito para arquivos ausentes, falhas de permissão ou problemas de escrita. |
| Escopo e memória | Cada script processa um único nome de arquivo fixo e carrega todo o conteúdo em memória. |

Essas características tornam a implementação apropriada para estudo do fluxo básico, mas inadequada para proteger dados reais. O código não implementa propagação, persistência, comunicação de rede, exfiltração ou cobrança de resgate.

## Aplicação em estudos de defesa

Em um laboratório com telemetria de processos e arquivos, o projeto pode apoiar a observação de:

- Execução de um interpretador Python associada a alterações no sistema de arquivos.
- Exclusão do arquivo original e criação de um arquivo com extensão diferente.
- Importância de correlacionar processo, usuário, caminho e contexto da execução.
- Validação de recuperação por comparação do conteúdo ou de hashes.

A extensão `.ransomwaretroll`, isoladamente, não comprova a ocorrência de ransomware. Uma análise de segurança precisa considerar o conjunto de comportamentos e o contexto.

## Problemas comuns

| Situação | O que verificar |
| --- | --- |
| `ModuleNotFoundError: No module named 'pyaes'` | Instale a dependência com `python -m pip install pyaes` usando o mesmo interpretador que executará os scripts. |
| `FileNotFoundError` | Confirme o diretório atual e a existência da entrada esperada: `teste.txt` para cifrar ou `teste.txt.ransomwaretroll` para recuperar. |
| Ativação bloqueada no PowerShell | Use diretamente `.\.venv\Scripts\python.exe` no lugar de `python` nos comandos de instalação e execução. |
| Conteúdo recuperado diferente do original | Interrompa o teste, preserve os arquivos restantes e confira se a chave e os dados cifrados foram alterados. Utilize a cópia de segurança para recuperar o arquivo de teste. |

## Autoria

Projeto de estudo de **Talita Bielecky**.

[Perfil no GitHub](https://github.com/bielecky) · [Código do projeto](https://github.com/bielecky/ransomware-python)

---

**Uso educacional:** realize os experimentos somente em arquivos e ambientes próprios ou explicitamente autorizados.
