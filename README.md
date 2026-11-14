# Inventário Europa

Sistema do suporte de TI do CESAR para controlar equipamentos (Macs, iPhones, iPads e monitores), colaboradores, máquinas secundárias, solicitações do Zeev e licenças de software.

## Estrutura

| Caminho | O que é |
|---|---|
| `public/index.html` | A aplicação inteira (HTML, CSS e JavaScript, sem etapa de build) |
| `firestore.rules` | Regras de segurança do banco: quem lê, quem edita e o que não pode ser apagado |
| `firebase.json` | Configuração do Firebase Hosting e das regras |
| `.firebaserc` | Projeto Firebase usado no deploy |
| `docs/` | Documentação do projeto (roadmap no padrão CESAR) |

## Tecnologias

Preact com htm (via CDN), Firebase Authentication (login Google restrito a @cesar.org.br), Cloud Firestore (região São Paulo) e Firebase Hosting.

## Como publicar

Pré-requisitos: Node.js LTS, `npm install -g firebase-tools` e `firebase login` com uma conta que tenha acesso ao projeto.

```
firebase deploy --only hosting          # publica a página
firebase deploy --only firestore:rules  # publica as regras do banco
```

Para testar localmente antes do deploy:

```
firebase serve --only hosting
```

e abra http://localhost:5000.

## Acessos

Quem pode entrar fica na coleção `acessos` do Firestore, um documento por e-mail (em minúsculas) com o campo `papel`:

- `leitor`: só consulta.
- `editor`: consulta e altera equipamentos, colaboradores e Zeevs.
- `admin`: também libera acessos e faz backup, pela página Administração.

O primeiro administrador é cadastrado pelo console do Firebase. Os demais, pela própria página.

## Licenças e Zeevs

- Cada licença tem programa, edição, assentos, prazo e expiração. A situação (ativa, vence em breve, vencida, fora de uso) é calculada pela data.
- As chaves ficam na coleção `licencas_chaves`, que só administradores leem e gravam.
- Quando um Zeev do tipo Licença é aprovado, cada licença ligada a ele ganha mais um ano a partir da expiração atual (ou da data da aprovação, se já venceu). Se o Zeev for cancelado, rejeitado ou a licença for desmarcada, a extensão é desfeita. O mesmo Zeev nunca estende a mesma licença duas vezes.

## Dados

Os dados ficam no Firestore e **não** fazem parte deste repositório. Backups, planilhas e arquivos de importação têm dados pessoais de colaboradores e não devem ser versionados (ver `.gitignore`).

O ID do projeto Firebase foi definido na criação e não pode ser alterado.
