# A Matemática do Movimento Browniano
### Conceitos Teóricos e Abordagem Computacional da Equação de Langevin

<div align="center">

[![Python](https://img.shields.io/badge/Python-3.12%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-2.3%2B-013243?style=flat-square&logo=numpy&logoColor=white)](https://numpy.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.10%2B-11557C?style=flat-square)](https://matplotlib.org/)
[![SciPy](https://img.shields.io/badge/SciPy-1.15%2B-8CAAE6?style=flat-square&logo=scipy&logoColor=white)](https://scipy.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![License](https://img.shields.io/badge/License-GPL-green?style=flat-square)](LICENSE)

*Giovani Massayuki Miranda Nagano · Mateus de Jesus Mendes · Matheus Macedo do Nascimento · Vinicius Francisco Wasques*

**[ILUM – Escola de Ciência](https://ilum.cnpem.br/) | [CNPEM](https://cnpem.br/)**

</div>


## Visão Geral

Este repositório contém um Jupyter Notebook de caráter pedagógico-científico dedicado ao estudo rigoroso do **Movimento Browniano** e de sua formulação dinâmica via **Equação de Langevin**. O material integra fundamentação teórica com implementação computacional, cobrindo desde o formalismo do cálculo estocástico até a análise quantitativa de erros de integração numérica.

O notebook foi concebido como projeto final da disciplina em Equações Diferenciais, cursada em 2025.2 na Ilum - Escola de Ciência.


## Conteúdo

O notebook está organizado em sete seções progressivas:

| # | Seção | Descrição |
|---|-------|-----------|
| 1 | **Fundamentação Teórica** | Derivação da equação de Langevin; Processo de Wiener; Cálculo e Lema de Itô; método Euler–Maruyama; Processo de Ornstein–Uhlenbeck; Teorema Flutuação–Dissipação |
| 2 | **Configuração** | Parâmetros físicos globais; estilo de plotagem *paper-like*; funções analíticas centralizadas |
| 3 | **Processo de Wiener** | Simulação em 1D, 2D e 3D; diagnósticos estatísticos (variância, distribuição dos incrementos, MSD, teste KS) |
| 4 | **Equação de Langevin** | Integração Euler–Maruyama vetorizada; trajetória individual e ensemble; evolução temporal da distribuição |
| 5 | **Processo de Ornstein–Uhlenbeck** | Comparação analítico vs. numérico vs. simulação exata; VACF; MSD; heatmap do ensemble; análise de convergência forte e fraca |
| 6 | **Análises Avançadas** | Teorema de Green–Kubo; coeficiente de difusão de Einstein; espaço de fase estocástico; estudo paramétrico em γ e σ |
| 7 | **Potencial Harmônico** | Extensão para armadilha óptica; comparação difusão livre vs. potencial harmônico; distribuição estacionária de posição |


## Fundamentos Teóricos

A **Equação de Langevin** para uma partícula livre em meio viscoso é dada por:

$$m\frac{dv}{dt} = -\gamma v + \sigma\xi(t)$$

onde $-\gamma v$ representa a força de arrasto viscoso (determinística) e $\sigma\xi(t)$ a força estocástica, com $\xi(t)$ o ruído branco formal ($\xi(t) = dW_t/dt$). Na forma de EDE de Itô:

$$dv(t) = -\frac{\gamma}{m}\,v(t)\,dt + \frac{\sigma}{m}\,dW_t$$

cuja solução analítica constitui o **Processo de Ornstein–Uhlenbeck** com os parâmetros $\theta = \gamma/m$ e $\mu = 0$. O esquema numérico empregado é o **Método de Euler–Maruyama**:

$$Y_{n+1} = Y_n - \frac{\gamma}{m}Y_n\,\Delta t + \frac{\sigma}{m}\,\Delta W_n, \qquad \Delta W_n \sim \mathcal{N}(0,\,\Delta t)$$

com ordens de convergência forte $O(\Delta t^{1/2})$ e fraca $O(\Delta t^1)$, verificadas empiricamente no notebook.

O **Teorema de Green–Kubo** estabelece a relação entre o coeficiente de difusão e a autocorrelação da velocidade (VACF):

$$D = \int_0^\infty \langle v(0)\,v(\tau) \rangle\,d\tau = \frac{\sigma^2}{2m^2\gamma}$$


## Funcionalidades Computacionais

- **`euler_maruyama()`** — integrador vetorizado para $M$ trajetórias simultâneas, com suporte a incrementos de Wiener externos para análise de erro forte *pathwise*
- **`ou_exact()`** — simulador de referência baseado na transição condicional gaussiana exata do processo OU, sem acumulação de erros de discretização
- **`langevin_harmonic()`** — extensão para campo de força harmônico externo (modelo de armadilha óptica)
- Funções analíticas centralizadas: $\mathbb{E}[v(t)]$, $\text{Var}[v(t)]$, $C_v(\tau)$, $\mathrm{MSD}(t)$, $p_\infty(v)$
- Análise de convergência forte e fraca com geração de incrementos compatíveis entre diferentes resoluções
- Cálculo do coeficiente de difusão por integração numérica da VACF (Green–Kubo) com comparação analítica


## Pré-requisitos

```
Python >= 3.10
numpy >= 1.24
matplotlib >= 3.7
scipy >= 1.11
tabulate >= 0.9
jupyter >= 1.0
```

Instalação das dependências via `pip`:

```bash
pip install numpy matplotlib scipy tabulate jupyter
```

ou via `conda`:

```bash
conda install numpy matplotlib scipy tabulate jupyter
```


## Execução

Clone o repositório e execute o notebook:

```bash
git clone https://github.com/mateusjmd/Langevin.git
cd Langevin
jupyter notebook Langevin.ipynb
```

Todos os parâmetros físicos do sistema ($m$, $\gamma$, $\sigma$) são definidos centralmente na **Seção 2** do notebook, facilitando a exploração de diferentes regimes dinâmicos sem modificações nas células subsequentes.


## Estrutura do Repositório

```
Langevin/
│
├── Langevin.ipynb    # Notebook principal
├── Langevin.pdf      # Artigo de referência (ILUM/CNPEM, 2026)
└── README.md
```


## Resultados Ilustrativos

O notebook reproduz e valida os seguintes resultados analíticos com precisão numérica:

| Observável | Analítico | Numérico (EM) |
|-----------|-----------|---------------|
| Variância estacionária $\sigma_\text{eq}^2 = \sigma^2 / (2\gamma m)$ | `0.50000` | `≈ 0.501` |
| Coeficiente de difusão $D = \sigma^2 / (2m^2\gamma)$ | `0.50000` | `≈ 0.482` (Green–Kubo) |
| Ordem de convergência forte | `0.5` | `≈ 0.50–0.55` |
| Ordem de convergência fraca | `1.0` | `≈ 0.90–1.05` |

> Valores obtidos com $m = \gamma = \sigma = 1$, $\Delta t = 10^{-2}$, $M = 2000$ trajetórias.

## Autores

O presente projeto foi desenvolvido pelos discentes [Giovani Massayuki Miranda Nagano](https://buscatextual.cnpq.br/buscatextual/visualizacv.do?id=K1100392J7), [Mateus de Jesus Mendes](https://buscatextual.cnpq.br/buscatextual/visualizacv.do?id=K1598982E2) e [Matheus Macedo do Nascimento]() sob a orientação do Prof. Dr. [Vinícius Francisco Wasques](https://buscatextual.cnpq.br/buscatextual/visualizacv.do?id=K4355089T5).

<div align="center">
<sub>Distribuído sob a Licença GPL-3.0. Consulte o arquivo <a href="LICENSE">LICENSE</a> para mais detalhes.</sub>
</div>