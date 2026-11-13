# Inventário Europa

Sistema do suporte de TI do CESAR para controlar equipamentos (Macs, iPhones, iPads e monitores), colaboradores, máquinas secundárias e solicitações do Zeev.

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

## Dados

Os dados ficam no Firestore e **não** fazem parte deste repositório. Backups, planilhas e arquivos de importação têm dados pessoais de colaboradores e não devem ser versionados (ver `.gitignore`).

O ID do projeto Firebase foi definido na criação e não pode ser alterado.
