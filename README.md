# Gerador de Arquivos EDI - Padrão Proceda (CON / COB)

Este repositório contém a documentação, os templates e as rotinas para a geração automatizada de arquivos EDI (Electronic Data Interchange) baseados no padrão **Proceda**, amplamente utilizado no setor de logística e transportes no Brasil para o intercâmbio eletrônico de dados entre transportadoras e embarcadores.

O projeto garante a conformidade com as regras estritas de formatação posicional de registros planos (*flat files*), estruturados especificamente para os fluxos de Conhecimento de Embarque (**CON**) e Cobrança (**COB**).

---

## 📋 Características dos Padrões Suportados

Os arquivos gerados seguem rigorosamente a especificação de largura fixa de linha, incluindo as regras de *padding* (preenchimento com espaços em branco à direita para campos de texto e preenchimento com zeros à esquerda para valores numéricos).

### 1. Padrão CON (Conhecimento de Embarque)
* **Finalidade:** Utilizado para o envio das informações dos conhecimentos de transporte (CT-e) emitidos pela transportadora para o embarcador.
* **Tamanho do Registro:** Exatamente **732 caracteres** por linha (incluindo a quebra de linha).
* **Identificadores de Linha:**
  * `000`: Cabeçalho do arquivo (Header de Intercâmbio)
  * `320`: Cabeçalho do documento de transporte
  * `321`: Dados do emitente/transportadora
  * `322`: Detalhes do conhecimento (valores, pesos, volumes, chaves)
  * `329`: Resumo de impostos ou dados complementares
  * `323`: Totalizador do arquivo (Trailer)

### 2. Padrão COB (Cobrança)
* **Finalidade:** Destinado à apresentação de faturas, duplicatas e dados de cobrança gerados a partir dos serviços logísticos executados.
* **Tamanho do Registro:** Exatamente **160 caracteres** por linha (incluindo a quebra de linha).
* **Identificadores de Linha:**
  * `000`: Cabeçalho do arquivo (Header de Intercâmbio)
  * `350`: Cabeçalho do documento de cobrança
  * `351`: Identificação da transportadora / cedente
  * `352`: Detalhes da fatura / vencimentos
  * `353`: Dados bancários e valores da cobrança
  * `354`: Dados complementares de rateio por documento
  * `355`: Totalizador do documento de cobrança (Trailer)

---

## 📂 Estrutura do Repositório

```text
├── templates/
│   ├── CON_template.txt     # Arquivo modelo padrão CON (732 posições)
│   └── COB_template.txt     # Arquivo modelo padrão COB (160 posições)
├── src/
│   └── generator.py         # Script de automação e validação de tamanho posicional
└── README.md                # Documentação principal do repositório
```

---

## ⚙️ Diretrizes para Contribuição e Desenvolvimento

Caso vá desenvolver novas integrações ou estender os scripts deste repositório, atente-se às seguintes restrições técnicas do padrão Proceda:

1. **Rigor Posicional:** Nenhuma linha pode conter mais ou menos caracteres do que a especificação padrão (732 para CON, 160 para COB). Caracteres adicionais corrompem a leitura nos sistemas ERP (como SAP, Protheus, etc.).
2. **Tratamento de Strings:** Sempre utilize codificação `ASCII` ou `ISO-8859-1` e realize o *strip* de acentuações antes da geração (ex: converter `á` para `a`, `ç` para `c`), pois o caractere especial pode quebrar a contagem de bytes dependendo do sistema receptor.
3. **Zeros à Esquerda:** Valores monetários e numéricos não devem conter pontos ou vírgulas (ex: R$ 816,33 deve ser formatado rigidamente como `000000000081633`).

---

## 📄 Licença

Este projeto está licenciado sob a licença MIT - consulte o arquivo de Licença para mais detalhes.
