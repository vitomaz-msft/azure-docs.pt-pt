---
title: Exemplos de transformação de afirmações StringCollection para a identidade experiência de estrutura de esquema do Azure Active Directory B2C | Documentos da Microsoft
description: StringCollection exemplos de transformação de afirmações para a identidade experiência de estrutura de esquema do Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 924da8c92bad1c5345d5d1833723a0c4e8ad7d29
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47432669"
---
# <a name="stringcollection-claims-transformations"></a>Transformações de afirmações de StringCollection

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo fornece exemplos para usar as transformações de afirmações de coleção de cadeia de caracteres do esquema de arquitetura de experiências de identidade no Azure Active Directory (Azure AD) B2C. Para obter mais informações, consulte [ClaimsTransformations](claimstransformations.md).

## <a name="additemtostringcollection"></a>AddItemToStringCollection

Adiciona uma afirmação de cadeia de caracteres para uma nova afirmação stringCollection. 

| Item | TransformationClaimType | Tipo de Dados | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | Item | cadeia | ClaimType sejam adicionados ao afirmações de saída. |
| InputClaim | coleção | stringCollection | [Opcional] Se especificado, a transformação de afirmações copia os itens desta coleção e adiciona o item no final do conjunto de afirmações de saída. |
| outputClaim | coleção | stringCollection | Os ClaimTypes que são produzidos este ClaimsTransformation po vyvolání. |

Utilize este afirmações de transformação para adicionar uma cadeia de caracteres a um stringCollection novo ou existente. Que é frequentemente utilizada uma **AAD UserWriteUsingAlternativeSecurityId** perfil técnico. Antes da criação de uma nova conta de redes sociais, **CreateOtherMailsFromEmail** transformação de afirmações lê o ClaimType e adiciona o valor para o **otherMails** ClaimType. 

A transformação de declarações seguinte adiciona o **e-mail** ClaimType para **otherMails** ClaimType.

```XML
<ClaimsTransformation Id="CreateOtherMailsFromEmail" TransformationMethod="AddItemToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Exemplo

- Afirmações de entrada:
    - **recolha**: ["someone@outlook.com"]
    - **item**: "admin@contoso.com"
- Afirmações de saída: 
    - **recolha**: ["someone@outlook.com","admin@contoso.com"]

## <a name="addparametertostringcollection"></a>AddParameterToStringCollection

Adiciona um parâmetro de cadeia de caracteres para uma nova afirmação stringCollection. 

| Item | TransformationClaimType | Tipo de Dados | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | coleção | stringCollection | [Opcional] Se especificado, a transformação de afirmações copia os itens desta coleção e adiciona o item no final do conjunto de afirmações de saída. |
| InputParameter | Item | cadeia | O valor a ser adicionado para a afirmação de saída. |
| outputClaim | coleção | stringCollection | Os ClaimTypes que serão produzidos este ClaimsTransformation po vyvolání. |

Utilize este afirmações de transformação para adicionar um valor de cadeia de caracteres para um stringCollection novo ou existente. O exemplo seguinte adiciona um endereço de e-mail constante (admin@contoso.com) para o **otherMails** de afirmação. 

```XML
<ClaimsTransformation Id="SetCompanyEmail" TransformationMethod="AddParameterToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="item" DataType="string" Value="admin@contoso.com" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Exemplo

- Afirmações de entrada:
    - **recolha**: ["someone@outlook.com"]
- Parâmetros de entrada 
    - **item**: "admin@contoso.com"
- Afirmações de saída:
    - **recolha**: ["someone@outlook.com","admin@contoso.com"]

## <a name="getsingleitemfromstringcollection"></a>GetSingleItemFromStringCollection

Obtém o primeiro item da coleção cadeia fornecida. 

| Item | TransformationClaimType | Tipo de Dados | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | coleção | stringCollection | Os ClaimTypes que são utilizados pela transformação de declarações para obter o item. |
| outputClaim | extractedItem | cadeia | Os ClaimTypes que são produzidos este ClaimsTransformation po vyvolání. O primeiro item na coleção. |

O exemplo seguinte lê a **otherMails** de afirmação e retornar o primeiro item para o **e-mail** de afirmação. 

```XML
<ClaimsTransformation Id="CreateEmailFromOtherMails" TransformationMethod="GetSingleItemFromStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedItem" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Exemplo

- Afirmações de entrada:
    - **recolha**: ["someone@outlook.com","someone@contoso.com"]
- Afirmações de saída: 
    - **extractedItem**: "someone@outlook.com"

