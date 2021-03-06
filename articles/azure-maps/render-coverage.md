---
title: Cobertura do Azure Maps de composição | Documentos da Microsoft
description: Saiba mais sobre a cobertura de composição no Azure Maps
author: jingjing-z
ms.author: jinzh
ms.date: 03/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 6e4ebd5bfd7225537046d34dd885d04e8a94878f
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "42054962"
---
# <a name="azure-maps-render-coverage"></a>Cobertura de composição do Azure Maps

Mapas do Azure utiliza quadrícula mosaicos e mosaicos em vetor, a criação de mapas. Em sua resolução menor, todo o mundo se encaixa num único mosaico. Em sua resolução mais elevada, um único mosaico representa 38 metros quadrados. Como aplicar zoom num mapa, por conseguinte, pode ver cada vez mais detalhes sobre continentes, regiões, cidades e ruas individuais. Para obter mais informações, consulte [níveis de Zoom e grelha de mosaico](zoom-levels-and-tile-grid.md).

No entanto, o Maps não tem o mesmo nível de informações e a precisão em todas as regiões. As tabelas seguintes fornecem informações sobre o nível de detalhe composto que pode esperar de cada região.

## <a name="legend"></a>Legenda

| Símbolo | Significado |
|--------|---------|
| ✓ | Região é representada com dados detalhados.   |
| Ø | Região é representada com dados simplificada. |


## <a name="africa"></a>África 


| Região | Mosaicos de varredura unificados | Mosaicos em vetor unificados |
| ------ | :------------------: | :------------------: |
| Argélia                          | ✓ | ✓ |
| Angola                           | ✓ | ✓ |
| Benim                            | ✓ | ✓ |
| Botsuana                         | ✓ | ✓ |
| Burkina Faso                     | ✓ | ✓ |
| Burundi                          | ✓ | ✓ |
| Cabo Verde                       |   | ✓ |
| Camarões                         | ✓ | ✓ |
| República Centro-Africana         |   | Ø |
| Chade                             |   | Ø |
| Comores                          |   | Ø |
| República do Congo                            | ✓ | ✓ |
| República Democrática do Congo | ✓ | ✓ |
| Costa do Marfim (Côte d’Ivoire)                    |   | Ø |
| Jibuti                         |   | Ø |
| Egito                            | ✓ | ✓ |
| Guiné Equatorial                |   | Ø |
| Eritreia                          |   | Ø |
| Etiópia                         |   | Ø |
| Gabão                            | ✓ | ✓ |
| Gâmbia                           |   | Ø |
| Gana                            | ✓ | ✓ |
| Guiné                           |   | Ø |
| Guiné Bissau                    |   | Ø |
| Quénia                            | ✓ | ✓ |
| Lesoto                          | ✓ | ✓ |
| Libéria                          |   | Ø |
| Líbia                            |   | Ø |
| Madagáscar                       |   | Ø |
| Malavi                           | ✓ | ✓ |
| Mali                             | ✓ | ✓ |
| Mauritânia                       | ✓ | ✓ |
| Maurícia                        | ✓ | ✓ |
| Mayotte                          | ✓ | ✓ |
| Marrocos                          | ✓ | ✓ |
| Moçambique                       | ✓ | ✓ |
| Namíbia                          | ✓ | ✓ |
| Níger                            | ✓ | ✓ |
| Nigéria                          | ✓ | ✓ |
| Reunião                          | ✓ | ✓ |
| Ruanda                           | ✓ | ✓ |
| Santa Helena, Ascensão e Tristão da Cunha |   | Ø |
| São Tomé e Príncipe            |   | Ø |
| Senegal                          | ✓ | ✓ |
| Serra Leoa                     |   | Ø |
| Somália                          |   | Ø |
| África do Sul                     | ✓ | ✓ |
| Sudão do Sul                      |   | Ø |
| Sudão                            |   | Ø |
| Suazilândia                        | ✓ | ✓ |
| Unidos República Unida da Tanzânia      | ✓ | ✓ |
| Togo                             | ✓ | ✓ |
| Tunísia                          | ✓ | ✓ |
| Uganda                           | ✓ | ✓ |
| Zâmbia                           | ✓ | ✓ |
| Zimbabué                         | ✓ | ✓ |

## <a name="americas"></a>Américas

| Região | Mosaicos de varredura unificados | Mosaicos em vetor unificados |
| ------ | :------------------: | :------------------: |
| Anguila                  | ✓ | ✓ |
| Antígua e Barbuda       | ✓ | ✓ |
| Argentina                 | ✓ | ✓ |
| Aruba                     | ✓ | ✓ |
| Baamas                   | ✓ | ✓ |
| Barbados                  | ✓ | ✓ |
| Belize                    | ✓ | ✓ |
| Bermudas                   |   | ✓ |
| Estado Plurinacional da Bolívia |   | ✓ |
| Bonaire, Santo Eustáquio e Saba |   | ✓ |
| Brasil                    | ✓ | ✓ |
| Canadá                    | ✓ | ✓ |
| Ilhas Caimão            | ✓ | ✓ |
| Chile                     | ✓ | ✓ |
| Ilha de Clipperton         |   | ✓ |
| Colômbia                  | ✓ | ✓ |
| Costa Rica                |   | ✓ |
| Cuba                      | ✓ | ✓ |
| Curaçao                   | ✓ | ✓ |
| Domínica                  | ✓ | ✓ |
| República Dominicana        | ✓ | ✓ |
| Equador                   |   | ✓ |
| Ilhas Falkland (Malvinas) |   | ✓ |
| Guiana Francesa             | ✓ | ✓ |
| Gronelândia                 |   | Ø |
| Granada                   | ✓ | ✓ |
| Guadalupe                | ✓ | ✓ |
| Guatemala                 |   | ✓ |
| Guiana                    | ✓ | ✓ |
| Haiti                     | ✓ | ✓ |
| Honduras                  | ✓ | ✓ |
| Jamaica                   | ✓ | ✓ |
| Martinica                | ✓ | ✓ |
| México                    | ✓ | ✓ |
| Monserrate                | ✓ | ✓ |
| Nicarágua                 | ✓ | ✓ |
| Ilhas Marianas do Norte  |   | ✓ |
| Panamá                    | ✓ | ✓ | 
| Paraguai                  |   | ✓ |
| Peru                      | ✓ | ✓ |
| Porto Rico               | ✓ | ✓ |
| Quebeque (Canadá)           |   | ✓ |
| São Bartolomeu          | ✓ | ✓ |
| São Cristóvão e Neves     | ✓ | ✓ |
| Santa Lúcia               | ✓ | ✓ |
| São Martinho (francês)     | ✓ | ✓ |
| São Pedro e Miquelão |   | ✓ |
| São Vicente e Granadinas | ✓ | ✓ |
| Sint Maarten (Holandês)      | ✓ | ✓ |
| Ilhas Geórgia do Sul e Sandwich do Sul |   | ✓ |
| Suriname                  |   | ✓ |
| Trinidad e Tobago       | ✓ | ✓ |
| Ilhas Turcas e Caicos  | ✓ | ✓ |
| Estados Unidos             | ✓ | ✓ |
| Uruguai                   | ✓ | ✓ |
| Venezuela                 | ✓ | ✓ |
| Ilhas Virgens Britânicas   | ✓ | ✓ |
| Ilhas Virgens, E. U. A.      | ✓ | ✓ |

## <a name="asia"></a>Ásia 

| Região | Mosaicos de varredura unificados | Mosaicos em vetor unificados |
| ------ | :------------------: | :------------------: |
| Afeganistão               |   | Ø |
| Bahrain                   | ✓ | ✓ |
| Bangladeche                |   | Ø |
| Butão                    |   | Ø |
| Território Britânico do Oceano Índico |   | Ø |
| Brunei                    | ✓ | ✓ |
| Camboja                  |   | Ø |
| China                     |   | Ø |
| Ilhas dos Cocos (Keeling)   |   | Ø |
| República Democrática Popular da Coreia do Norte |   | Ø |
| Takeshima       |   | Ø |
| RAE de Hong Kong                 | ✓ | ✓ |
| Indonésia                 | ✓ | ✓ |
| Irão                      |   | Ø |
| Iraque                      | ✓ | ✓ |
| Israel                    |   | ✓ |
| Japão                     |   | Ø |
| Jordânia                    | ✓ | ✓ |
| Cazaquistão                |   | ✓ |
| Kuwait                    | ✓ | ✓ |
| Quirguizistão                |   | Ø |
| República Democrática Popular do Laos |   | Ø |
| Líbano                   | ✓ | ✓ |
| Macau                     | ✓ | ✓ |
| Malásia                  | ✓ | ✓ |
| Maldivas                  |   | Ø |
| Mongólia                  |   | Ø |
| Myanmar                   |   | Ø |
| Napal                     |   | Ø |
| Omã                      | ✓ | ✓ |
| Paquistão                  |   | Ø |
| Filipinas               | ✓ | ✓ |
| Catar                     | ✓ | ✓ |
| República da Coreia         | ✓ | Ø |
| Arábia Saudita              | ✓ | ✓ |
| Ilhas Senkaku/Diaoyutai           |   | ✓ |
| Singapura                 | ✓ | ✓|
| Sri Lanka                 |   | Ø |
| República Árabe Síria      |   | Ø |
| Taiwan                    | ✓ | ✓ |
| Tajiquistão                |   | Ø |
| Tailândia                  | ✓ | ✓ |
| Timor-Leste               |   | Ø |
| Turquemenistão              |   | Ø |
| Emirados Árabes Unidos      | ✓ | ✓ |
| Ilhas Menores Afastadas dos Estados Unidos |   | Ø |
| Usbequistão                |   | Ø |
| Vietname                   | ✓ | ✓ |
| Iémen                     | ✓ | ✓ |

## <a name="oceania"></a>Oceânia

| Região | Mosaicos de varredura unificados | Mosaicos em vetor unificados |
| ------ | :------------------: | :------------------: |
| Samoa Americana            |   | ✓ |
| Austrália                 | ✓ | ✓ |
| Ilhas Cook              |   | Ø |
| Fiji                      |   | Ø |
| Polinésia Francesa          |   | Ø |
| Guam                      | ✓ | ✓ |
| Quiribati                  |   | Ø |
| Ilhas Marshall          |   | Ø |
| Micronésia                |   | Ø |
| Nauru                     |   | Ø |
| Nova Caledónia             |   | Ø |
| Nova Zelândia               | ✓ | ✓ |
| Niue                      |   | Ø |
| Ilha Norfolk            |   | Ø |
| Ilhas Palaus                     |   | Ø |
| Papua-Nova Guiné          |   | Ø |
| Pitcairn                  |   | Ø |
| Estado Independente de Samoa                     |   | Ø |
| Ilhas Salomão           |   | Ø|
| Toquelau                   |   | Ø |
| Tonga                     |   | Ø |
| Tuvalu                    |   | Ø |
| Vanuatu                   |   | Ø |
| Wallis e Futuna         |   | Ø |


## <a name="europe"></a>Europa

| Região | Mosaicos de varredura unificados | Mosaicos em vetor unificados |
| ------ | :------------------: | :------------------: |
| Albânia                   | ✓ | ✓ |
| Andorra                   | ✓ | ✓ |
| Arménia                   |   | Ø |
| Áustria                   | ✓ | ✓ |
| Azerbaijão                |   | Ø |
| Bielorrússia                   | Ø | ✓ |
| Bélgica                   | ✓ | ✓ |
| Bósnia e Herzegovina        | ✓ | ✓ |
| Bulgária                  | ✓ | ✓ |
| Croácia                   | ✓ | ✓ |
| Chipre                    | ✓ | ✓ |
| República Checa            | ✓ | ✓ |
| Dinamarca                   | ✓ | ✓ |
| Estónia                   | ✓ | ✓ |
| Ilhas Faroé             |   | Ø |
| Finlândia                   | ✓ | ✓ |
| França                    | ✓ | ✓ |
| Geórgia                   |   | Ø |
| Alemanha                   | ✓ |✓ |
| Gibraltar                 | ✓ |   |
| Grécia                    | ✓ | ✓ |
| Guernsey                  |   | ✓ |
| Hungria                   | ✓ | ✓ |
| Islândia                   | ✓ | ✓ |
| Irlanda (República da)     | ✓ | ✓ |
| Ilha de Man               |   | ✓ |
| Itália                     | ✓ | ✓ |
| Jan Mayen                 |   | ✓ |
| Jersey                    |   | ✓ |
| Letónia                    | ✓ | ✓ |
| Liechtenstein             | ✓ | ✓ |
| Lituânia                 | ✓ | ✓ |
| Luxemburgo                | ✓ | ✓ |
| Macedónia                 | ✓ | ✓ |
| Malta                     | ✓ | ✓ |
| Moldova                   | ✓ | ✓ |
| Mónaco                    | ✓ | ✓ |
| Montenegro                | ✓ | ✓ |
| Países Baixos               | ✓ | ✓ |
| Noruega                    | ✓ | ✓ |
| Polónia                    | ✓ | ✓ |
| Portugal                  | ✓ | ✓ |
| Roménia                   | ✓ | ✓ |
| Federação Russa        | ✓ | ✓ |
| São Marino                | ✓ | ✓ |
| Sérvia                    | ✓ | ✓ |
| Eslováquia                  | ✓ | ✓ |
| Eslovénia                  | ✓ | ✓ |
| Territórios Austrais Kurils           |   | ✓ |
| Espanha                     | ✓ | ✓ |
| Svalbard                  |   | ✓ |
| Suécia                    | ✓ |   |
| Suíça               | ✓ | ✓ |
| Turquia                    | ✓ | ✓ |
| Ucrânia                   | ✓ | ✓ |
| Reino Unido            | ✓ | ✓ |
| Cidade do Vaticano              | ✓ | ✓ |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a composição do Azure Maps, consulte [níveis de Zoom e grelha de mosaico](zoom-levels-and-tile-grid.md).

Saiba mais sobre o [áreas de cobertura para os mapas de serviço de encaminhamento](routing-coverage.md). 