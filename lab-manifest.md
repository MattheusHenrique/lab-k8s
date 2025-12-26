# Laboratorio manifestos

## Contexto da Atividade

Nossos desenvolvedores estão construindo um app simples de votação para uso interno em nossas decisões compartilhadas.
Porém, nossos desenvolvedores não possuem experiência com os manifestos YAML usados para implementar as funcionalidades no Kubernetes.
Então, precisamos do seu apoio para construir esses manifestos da melhor forma possível e levar o app e tudo que necessita para nosso ambiente.

## Sobre o App de Votação

O aplicativo de votação é uma solução web distribuída que permite aos membros da equipe votarem em decisões compartilhadas de forma democrática. A arquitetura do sistema é composta por múltiplos serviços que trabalham em conjunto:

![Arquitetura do App de Votação](https://raw.githubusercontent.com/dockersamples/example-voting-app/main/architecture.excalidraw.png)

### Componentes da Arquitetura

1. **Serviço Vote (Python)**
   - Frontend web desenvolvido em Python
   - Interface para os usuários registrarem seus votos
   - Armazena os votos temporariamente no Redis

2. **Serviço Result**
   - Frontend web para visualização dos resultados das votações
   - Exibe os resultados em tempo real
   - Consulta dados do banco PostgreSQL

3. **Serviço Worker (.NET)**
   - Processa os votos armazenados no Redis
   - Transfere e consolida os dados no banco PostgreSQL
   - Garante a persistência dos votos

4. **Redis**
   - Cache em memória para armazenamento temporário de votos
   - Alta performance para escrita de dados
   - Atua como fila entre o serviço vote e o worker

5. **PostgreSQL (db)**
   - Banco de dados relacional para armazenamento persistente
   - Armazena os resultados consolidados das votações
   - Fonte de dados para o serviço result

### Fluxo de Dados

1. Usuário acessa o serviço **vote** e registra seu voto
2. O voto é armazenado no **Redis** (cache temporário)
3. O **worker** processa os votos do Redis e consolida no **PostgreSQL**
4. Usuário acessa o serviço **result** para visualizar os resultados
5. O serviço **result** consulta o **PostgreSQL** para exibir os dados atualizados

### Imagens Docker

Os desenvolvedores forneceram os requisitos das imagens Docker que devem ser utilizadas nos manifestos:

- `postgres:15-alpine` - Banco de dados PostgreSQL
- `redis:alpine` - Cache Redis
- `dockersamples/examplevotingapp_result` - Serviço de resultados
- `dockersamples/examplevotingapp_vote` - Serviço de votação
- `dockersamples/examplevotingapp_worker` - Worker de processamento

### Considerações Importantes

- O ambiente Kubernetes já está configurado e pronto para receber os manifestos
- Resource limits e requests apropriados
- Namespace: considerar uso de namespace

## Objetivo

Criar todos os manifestos YAML necessários para fazer o deploy completo do app de votação no cluster Kubernetes, seguindo as melhores práticas.

## Adicional

Se possível, faça um fork desse repositório no GitHub e adicione os manifestos Kubernetes em uma pasta chamada `lab-manifest` no repositório forkado.

É importante fazer commits dos manifestos no repositório Git para manter controle de versão, histórico de mudanças e facilitar rollback em caso de problemas.
