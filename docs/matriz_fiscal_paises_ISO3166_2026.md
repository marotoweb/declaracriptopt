# Matriz estrutural de consulta: códigos de países (Norma ISO 3166)

Esta especificação técnica atua como uma tabela de consulta estática e imutável para o motor de pipeline analítico. O seu objetivo primordial é mapear os códigos numéricos oficiais utilizados pela Autoridade Tributária e Aduaneira (AT), dividindo rigorosamente as strings linguísticas de identificação e injetando as flags binárias de cooperação fiscal em conformidade estrita com a Portaria n.º 150/2004 (atualizada pela Portaria n.º 292/2025).

> [!WARNING]
> **Aviso legal e de atualização fiscal**
> Esta matriz foi construída com base na legislação vigente e em fontes oficiais à data de junho de 2026, nomeadamente:
> - Portaria n.º 150/2004 (lista de países não cooperantes), com as alterações da Portaria n.º 292/2025.
> - Tabela oficial de convenções para evitar a dupla tributação (CDT): [Tabela_CDT_2026.pdf](https://info.portaldasfinancas.gov.pt/pt/informacao_fiscal/convencoes_evitar_dupla_tributacao/convencoes_tabelas_doclib/Documents/Tabela_CDT_2026.pdf).
> 
> No entanto:
> 1. A lista de jurisdições não cooperantes e o estado das CDTs são suscetíveis a alterações por parte da Autoridade Tributária (AT) e do Ministério dos Negócios Estrangeiros.
> 2. Este documento não substitui a consulta oficial às fontes primárias (Diário da República e Portal das Finanças) antes da implementação em produção ou da submissão de declarações fiscais.
> 3. A utilização desta matriz para cálculo automático de impostos é da exclusiva responsabilidade do utilizador. Em caso de divergência, prevalecem sempre os diplomas legais oficiais publicados em vigor à data do facto gerador.
> 4. Confirme sempre o estatuto atualizado diretamente no Portal das Finanças (https://info.portaldasfinancas.gov.pt) antes de processar operações críticas.

---

### 1. Impacto lógico e regras de negócio no *pipeline*

O estatuto de cooperação determinado nesta matriz opera sobre as transações encaminhadas para o Anexo J (operações cujo `countryCode` da entidade ou exchange de origem seja diferente de 620 [Portugal]). A deteção de uma jurisdição marcada como não cooperante despoleta duas regras matemáticas no motor:

A aplicação da taxa autónoma base de 28% sobre as mais-valias líquidas geradas nessa entidade é anulada, forçando a aplicação automática da taxa agravada de 35% (Art. 72.º, n.º 4 do CIRS).

Mesmo que os sub-lotes contidos no provenance_history comprovem um período de detenção igual ou superior a 365 dias, o direito à isenção de mais-valias de criptoativos é revogado (Art. 43.º, n.º 9 do CIRS). A totalidade da mais-valia é tributada à taxa de 35%.

A existência de uma convenção para evitar a dupla tributação (CDT) não anula o estatuto de não cooperante para efeitos de tributação de criptoativos no Anexo J. Se o estatuto for não cooperante, aplica-se a taxa de 35%, mesmo que exista CDT (como acontece com os Emirados Árabes Unidos). A coluna CDT serve apenas para cálculo de retenções na fonte sobre dividendos ou juros, não para a taxa liberatória de mais-valias de criptoativos.

---

### 2. Base de dados de consulta (Mapeamento completo IRS)

| Cód. Num. | Cód. Alf2 | Cód. Alf3 | Designação (Português) | Designação em Inglês | Cooperação Fiscal | Possui CDT (Vigente) | Alíquota IRS (Anexo J) |
| :---: | :---: | :---: | :--- | :--- | :---: | :---: | :---: |
| 004 | AF | AFG | AFEGANISTÃO | AFGHANISTAN | Cooperante | Não | 28% (Geral) |
| 710 | ZA | ZAF | ÁFRICA DO SUL | SOUTH AFRICA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 008 | AL | ALB | ALBÂNIA | ALBANIA, PEOPLE'S SOCIALIST REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 276 | DE | DEU | ALEMANHA | GERMANY | Cooperante | Sim | 28% (Geral) |
| 020 | AD | AND | ANDORRA | ANDORRA, PRINCIPALITY OF | Cooperante | Sim | 28% (Geral) |
| 024 | AO | AGO | ANGOLA | ANGOLA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 660 | AI | AIA | ANGUILA | ANGUILLA | Não Cooperante | Não | 35% (Agravada) |
| 010 | AQ | ATA | ANTÁRCTICA | ANTARCTICA (THE TERRITORY SOUTH OF 60 DEG S) | Cooperante | Não | 28% (Geral) |
| 028 | AG | ATG | ANTÍGUA E BARBUDA | ANTIGUA AND BARBUDA | Não Cooperante | Não | 35% (Agravada) |
| 682 | SA | SAU | ARÁBIA SAUDITA | SAUDI ARABIA, KINGDOM OF | Cooperante | Sim | 28% (Geral) |
| 012 | DZ | DZA | ARGÉLIA | ALGERIA, DEMOCRATIC PEOPLE'S REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 032 | AR | ARG | ARGENTINA | ARGENTINE REPUBLIC | Cooperante | Sim | 28% (Geral) |
| 051 | AM | ARM | ARMÉNIA | ARMENIA | Cooperante | Não | 28% (Geral) |
| 533 | AW | ABW | ARUBA | ARUBA | Não Cooperante | Não | 35% (Agravada) |
| 036 | AU | AUS | AUSTRÁLIA | AUSTRALIA, COMMONWEALTH OF | Cooperante | Sim | 28% (Geral) |
| 040 | AT | AUT | ÁUSTRIA | AUSTRIA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 031 | AZ | AZE | AZERBAIDJÃO | AZERBAIJAN, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 044 | BS | BHS | BAHAMAS | BAHAMAS, COMMONWEALTH OF THE | Não Cooperante | Não | 35% (Agravada) |
| 048 | BH | BHR | BAHREIN | BAHRAIN, KINGDOM OF | Não Cooperante | Sim | 35% (Agravada) |
| 050 | BD | BGD | BANGLADESH | BANGLADESH, PEOPLE'S REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 052 | BB | BRB | BARBADOS | BARBADOS | Não Cooperante | Não | 35% (Agravada) |
| 056 | BE | BEL | BÉLGICA | BELGIUM, KINGDOM OF | Cooperante | Sim | 28% (Geral) |
| 084 | BZ | BLZ | BELIZE | BELIZE | Não Cooperante | Não | 35% (Agravada) |
| 204 | BJ | BEN | BENIN | BENIN, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 060 | BM | BMU | BERMUDAS | BERMUDA | Não Cooperante | Não | 35% (Agravada) |
| 112 | BY | BLR | BIELORÚSSIA | BELARUS, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 068 | BO | BOL | BOLÍVIA | BOLIVIA, REPUBLIC OF | Não Cooperante | Não | 35% (Agravada) |
| 070 | BA | BIH | BÓSNIA-HERZEGOVINA | BOSNIA AND HERZEGOVINA | Cooperante | Não | 28% (Geral) |
| 072 | BW | BWA | BOTSUANA | BOTSWANA, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 076 | BR | BRA | BRASIL | BRAZIL, FEDERATIVE REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 096 | BN | BRN | BRUNEI | BRUNEI DARUSSALAM | Não Cooperante | Não | 35% (Agravada) |
| 100 | BG | BGR | BULGÁRIA | BULGARIA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 854 | BF | BFA | BURKINA FASO | BURKINA FASO | Cooperante | Não | 28% (Geral) |
| 108 | BI | BDI | BURUNDI | BURUNDI, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 064 | BT | BTN | BUTÃO | BHUTAN, KINGDOM OF | Cooperante | Não | 28% (Geral) |
| 132 | CV | CPV | CABO VERDE | CAPE VERDE, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 116 | KH | KHM | CAMBOJA | CAMBODIA, KINGDOM OF | Cooperante | Não | 28% (Geral) |
| 120 | CM | CMR | CAMERÕES | CAMEROON, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 124 | CA | CAN | CANADÁ | CANADA | Cooperante | Sim | 28% (Geral) |
| 634 | QA | QAT | CATAR | QATAR, STATE OF | Cooperante | Sim | 28% (Geral) |
| 140 | CF | CAF | CENTRO-AFRICANA (REPÚBLICA) | CENTRAL AFRICAN REPUBLIC | Cooperante | Não | 28% (Geral) |
| 148 | TD | TCD | CHADE | CHAD, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 203 | CZ | CZE | CHECA (REPÚBLICA) | CZECH REPUBLIC | Cooperante | Sim | 28% (Geral) |
| 152 | CL | CHL | CHILE | CHILE, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 156 | CN | CHN | CHINA (REPÚBLICA POPULAR DA) | CHINA, PEOPLE'S REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 196 | CY | CYP | CHIPRE | CYPRUS, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 170 | CO | COL | COLÔMBIA | COLOMBIA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 174 | KM | COM | COMORES | COMOROS, UNION OF THE | Cooperante | Não | 28% (Geral) |
| 178 | CG | COG | CONGO | CONGO, REPUBLIC OF THE | Cooperante | Não | 28% (Geral) |
| 180 | CD | COD | CONGO (REP. DEMOCRÁTICA DO) | CONGO, DEMOCRATIC REPUBLIC OF THE | Cooperante | Não | 28% (Geral) |
| 410 | KR | KOR | COREIA (REPÚBLICA DA) | KOREA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 408 | KP | PRK | COREIA (REP. DEM. POPULAR DA) | KOREA, DEMOCRATIC PEOPLE'S REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 191 | HR | HRV | CROÁCIA | CROATIA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 192 | CU | CUB | CUBA | CUBA, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 531 | CW | CUW | CURAÇAO | CURAÇAO | Não Cooperante | Não | 35% (Agravada) |
| 208 | DK | DNK | DINAMARCA | DENMARK, KINGDOM OF | Cooperante | Sim | 28% (Geral) |
| 212 | DM | DMA | DOMINICA | DOMINICA, COMMONWEALTH OF | Não Cooperante | Não | 35% (Agravada) |
| 214 | DO | DOM | DOMINICANA (REPÚBLICA) | DOMINICAN REPUBLIC | Cooperante | Sim | 28% (Geral) |
| 818 | EG | EGY | EGIPTO | EGYPT, ARAB REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 222 | SV | SLV | EL SALVADOR | EL SALVADOR, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 784 | AE | ARE | EMIRADOS ÁRABES UNIDOS | UNITED ARAB EMIRATES | Cooperante | Sim | 28% (Geral) |
| 218 | EC | ECU | EQUADOR | ECUADOR, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 232 | ER | ERI | ERITREIA | ERITREA, STATE OF | Cooperante | Não | 28% (Geral) |
| 703 | SK | SVK | ESLOVÁQUIA | SLOVAKIA (SLOVAK REPUBLIC) | Cooperante | Sim | 28% (Geral) |
| 705 | SI | SVN | ESLOVÉNIA | SLOVENIA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 724 | ES | ESP | ESPANHA | SPAIN, KINGDOM OF | Cooperante | Sim | 28% (Geral) |
| 840 | US | USA | ESTADOS UNIDOS DA AMÉRICA | UNITED STATES OF AMERICA | Cooperante | Sim | 28% (Geral) |
| 233 | EE | EST | ESTÓNIA | ESTONIA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 231 | ET | ETH | ETIÓPIA | ETHIOPIA, FEDERAL DEMOCRATIC REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 242 | FJ | FJI | FIJI | FIJI, REPUBLIC OF THE FIJI ISLANDS | Não Cooperante | Não | 35% (Agravada) |
| 608 | PH | PHL | FILIPINAS | PHILIPPINES, REPUBLIC OF THE | Cooperante | Sim | 28% (Geral) |
| 246 | FI | FIN | FINLÂNDIA | FINLAND, REPUBLIC OF | Cooperante | Não (Denunciada) | 28% (Geral) |
| 250 | FR | FRA | FRANÇA | FRANCE | Cooperante | Sim | 28% (Geral) |
| 266 | GA | GAB | GABÃO | GABONESE REPUBLIC | Cooperante | Não | 28% (Geral) |
| 270 | GM | GMB | GÂMBIA | GAMBIA, REPUBLIC OF THE | Não Cooperante | Não | 35% (Agravada) |
| 268 | GE | GEO | GEÓRGIA | GEORGIA | Cooperante | Sim | 28% (Geral) |
| 288 | GH | GHA | GANA | GHANA, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 292 | GI | GIB | GIBRALTAR | GIBRALTAR | Não Cooperante | Não | 35% (Agravada) |
| 308 | GD | GRD | GRANADA | GRANADA | Não Cooperante | Não | 35% (Agravada) |
| 300 | GR | GRC | GRÉCIA | GREECE (HELLENIC REPUBLIC) | Cooperante | Sim | 28% (Geral) |
| 312 | GP | GLP | GUADALUPE | GUADELOUPE (DEPARTEMENT DE LA) | Cooperante | Não | 28% (Geral) |
| 316 | GU | GUM | GUAM | GUAM | Cooperante | Não | 28% (Geral) |
| 320 | GT | GTM | GUATEMALA | GUATEMALA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 831 | GG | GGY | GUERNESEY | GUERNSEY | Não Cooperante | Não | 35% (Agravada) |
| 328 | GY | GUY | GUIANA | GUYANA, CO-OPERATIVE REPUBLIC OF | Não Cooperante | Não | 35% (Agravada) |
| 254 | GF | GUF | GUIANA FRANCESA | FRENCH GUIANA (DEPARTEMENT DE LA) | Cooperante | Não | 28% (Geral) |
| 324 | GN | GIN | GUINÉ | GUINEA, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 624 | GW | GNB | GUINÉ-BISSAU | GUINEA-BISSAU, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 226 | GQ | GNQ | GUINÉ EQUATORIAL | EQUATORIAL GUINEA, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 332 | HT | HTI | HAITI | HAITI, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 340 | HN | HND | HONDURAS | HONDURAS, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 344 | HK | HKG | HONG-KONG | HONG KONG (SPECIAL ADMINISTRATIVE REGION) | Cooperante | Sim | 28% (Geral) |
| 348 | HU | HUN | HUNGRIA | HUNGARY, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 887 | YE | YEM | IÉMEN | YEMEN, REPUBLIC OF | Não Cooperante | Não | 35% (Agravada) |
| 833 | IM | IMN | ILHA DE MAN | ISLE OF MAN | Não Cooperante | Não | 35% (Agravada) |
| 162 | CX | CXR | ILHA DO NATAL | CHRISTMAS ISLAND | Não Cooperante | Não | 35% (Agravada) |
| 574 | NF | NFK | ILHA NORFOLK | NORFOLK ISLAND | Não Cooperante | Não | 35% (Agravada) |
| 136 | KY | CYM | ILHAS CAIMÃO | CAYMAN ISLANDS | Não Cooperante | Não | 35% (Agravada) |
| 166 | CC | CCK | ILHAS DOS COCOS | COCOS (KEELING) ISLANDS | Não Cooperante | Não | 35% (Agravada) |
| 184 | CK | COK | ILHAS COOK | COOK ISLANDS | Não Cooperante | Não | 35% (Agravada) |
| 238 | FK | FLK | ILHAS FALKLAND | FALKLAND ISLANDS (MALVINAS) | Não Cooperante | Não | 35% (Agravada) |
| 234 | FO | FRO | ILHAS FAROE | FAEROE ISLANDS | Não Cooperante | Não | 35% (Agravada) |
| 580 | MP | MNP | ILHAS MARIANAS DO NORTE | NORTHERN MARIANA ISLANDS | Não Cooperante | Não | 35% (Agravada) |
| 584 | MH | MHL | ILHAS MARSHALL | MARSHALL ISLANDS, REPUBLIC OF THE | Não Cooperante | Não | 35% (Agravada) |
| 612 | PN | PCN | ILHAS PITCAIRN | PITCAIRN (PITCAIRN, HENDERSON, DUCIE & OENO) | Não Cooperante | Não | 35% (Agravada) |
| 090 | SB | SLB | ILHAS SALOMÃO | SOLOMON ISLANDS | Não Cooperante | Não | 35% (Agravada) |
| 796 | TC | TCA | ILHAS TURCAS E CAICOS | TURKS AND CAICOS ISLANDS | Não Cooperante | Não | 35% (Agravada) |
| 092 | VG | VGB | ILHAS VIRGENS BRITÂNICAS | VIRGIN ISLANDS, BRITISH | Não Cooperante | Não | 35% (Agravada) |
| 850 | VI | VIR | ILHAS VIRGENS DOS ESTADOS UNIDOS | VIRGIN ISLANDS OF THE UNITED STATES | Não Cooperante | Não | 35% (Agravada) |
| 356 | IN | IND | ÍNDIA | INDIA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 360 | ID | IDN | INDONÉSIA | INDONESIA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 364 | IR | IRN | IRRÃO | IRAN, ISLAMIC REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 368 | IQ | IRQ | IRAQUE | IRAQ, REPUBLIC OF | Cooperante | Não | 28% (Geral) |
| 372 | IE | IRL | IRLANDA | IRELAND | Cooperante | Sim | 28% (Geral) |
| 352 | IS | ISL | ISLÂNDIA | ICELAND, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 376 | IL | ISR | ISRAEL | ISRAEL, STATE OF | Cooperante | Sim | 28% (Geral) |
| 380 | IT | ITA | ITÁLIA | ITALY (ITALIAN REPUBLIC) | Cooperante | Sim | 28% (Geral) |
| 388 | JM | JAM | JAMAICA | JAMAICA | Cooperante | Não | 28% (Geral) |
| 392 | JP | JPN | JAPÃO | JAPAN | Cooperante | Sim | 28% (Geral) |
| 832 | JE | JEY | JERSEY | JERSEY | Não Cooperante | Não | 35% (Agravada) |
| 262 | DJ | DJI | JIBUTI | DJIBOUTI, REPUBLIC OF | Não Cooperante | Não | 35% (Agravada) |
| 400 | JO | JOR | JORDÂNIA | JORDAN, HASHEMITE KINGDOM OF | Não Cooperante | Não | 35% (Agravada) |
| 422 | LB | LBN | LÍBANO | LEBANON (LEBANESE REPUBLIC) | Não Cooperante | Não | 35% (Agravada) |
| 430 | LR | LBR | LIBÉRIA | LIBERIA, REPUBLIC OF | Não Cooperante | Não | 35% (Agravada) |
| 438 | LI | LIE | LIECHTENSTEIN | LIECHTENSTEIN, PRINCIPALITY OF | Cooperante | Sim | 28% (Geral) |
| 442 | LU | LUX | LUXEMBURGO | LUXEMBOURG, GRAND DUCHY OF | Cooperante | Sim | 28% (Geral) |
| 446 | MO | MAC | MACAU | MACAU (SPECIAL ADMINISTRATIVE REGION) | Não Cooperante | Não | 35% (Agravada) |
| 462 | MV | MDV | MALDIVAS | MALDIVES, REPUBLIC OF | Não Cooperante | Não | 35% (Agravada) |
| 470 | MT | MLT | MALTA | MALTA, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 480 | MU | MUS | MAURÍCIA | MAURITIUS, REPUBLIC OF | Não Cooperante | Sim | 35% (Agravada) |
| 492 | MC | MCO | MÓNACO | MONACO, PRINCIPALITY OF | Não Cooperante | Não | 35% (Agravada) |
| 500 | MS | MSR | MONSERRATE | MONTSERRAT | Não Cooperante | Não | 35% (Agravada) |
| 520 | NR | NRU | NAURU | NAURU, REPUBLIC OF | Não Cooperante | Não | 35% (Agravada) |
| 570 | NU | NIU | NIUE | NIUE | Não Cooperante | Não | 35% (Agravada) |
| 512 | OM | OMN | OMÃ | OMAN, SULTANATE OF | Não Cooperante | Sim | 35% (Agravada) |
| 591 | PA | PAN | PANAMÁ | PANAMA, REPUBLIC OF | Não Cooperante | Sim | 35% (Agravada) |
| 620 | PT | PRT | PORTUGAL | PORTUGAL (PORTUGUESE REPUBLIC) | Cooperante | N/A | Taxa Nacional |
| 630 | PR | PRI | PORTO RICO | PUERTO RICO | Não Cooperante | Não | 35% (Agravada) |
| 404 | KE | KEN | QUÉNIA | KENYA, REPUBLIC OF | Cooperante | Não (Não vigente) | 28% (Geral) |
| 826 | GB | GBR | REINO UNIDO | UNITED KINGDOM | Cooperante | Sim | 28% (Geral) |
| 882 | WS | WSM | SAMOA | SAMOA, INDEPENDENT STATE OF | Não Cooperante | Não | 35% (Agravada) |
| 016 | AS | ASM | SAMOA AMERICANA | AMERICAN SAMOA | Não Cooperante | Não | 35% (Agravada) |
| 654 | SH | SHN | SANTA HELENA | SAINT HELENA | Não Cooperante | Não | 35% (Agravada) |
| 662 | LC | LCA | SANTA LÚCIA | SAINT LUCIA | Não Cooperante | Não | 35% (Agravada) |
| 659 | KN | KNA | SÃO CRISTÓVÃO E NEVES | SAINT KITTS AND NEVIS | Não Cooperante | Não | 35% (Agravada) |
| 674 | SM | SMR | SÃO MARINHO | SAN MARINO, REPUBLIC OF | Não Cooperante | Sim | 35% (Agravada) |
| 666 | PM | SPM | SÃO PEDRO E MIQUELÃO | SAINT PIERRE AND MIQUELON | Não Cooperante | Não | 35% (Agravada) |
| 670 | VC | VCT | SÃO VICENTE E GRANADINAS | SAINT VINCENT AND THE GRENADINES | Não Cooperante | Não | 35% (Agravada) |
| 690 | SC | SYC | SEICHELES | SEYCHELLES, REPUBLIC OF | Não Cooperante | Sim | 35% (Agravada) |
| 702 | SG | SGP | SINGAPURA | SINGAPORE, REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 748 | SZ | SWZ | SWAZILÂNDIA | SWAZILAND, KINGDOM OF | Não Cooperante | Não | 35% (Agravada) |
| 744 | SJ | SJM | SVALBARD E JAN MAYEN | SVALBARD AND JAN MAYEN | Não Cooperante | Não | 35% (Agravada) |
| 752 | SE | SWE | SUÉCIA | SWEDEN, KINGDOM OF | Cooperante | Não (Denunciada) | 28% (Geral) |
| 756 | CH | CHE | SUÍÇA | SWITZERLAND (SWISS CONFEDERATION) | Cooperante | Sim | 28% (Geral) |
| 772 | TK | TKL | TOQUELAU | TOKELAU (TOKELAU ISLANDS) | Não Cooperante | Não | 35% (Agravada) |
| 776 | TO | TON | TONGA | TONGA, KINGDOM OF | Não Cooperante | Não | 35% (Agravada) |
| 780 | TT | TTO | TRINDADE E TOBAGO | TRINIDAD AND TOBAGO, REPUBLIC OF | Não Cooperante | Não | 35% (Agravada) |
| 798 | TV | TUV | TUVALU | TUVALU | Não Cooperante | Não | 35% (Agravada) |
| 858 | UY | URY | URUGUAI | URUGUAI, EASTERN REPUBLIC OF | Cooperante | Sim | 28% (Geral) |
| 548 | VU | VUT | VANUATU | VANUATU, REPUBLIC OF | Não Cooperante | Não | 35% (Agravada) |

---

### 3. Notas técnicas de consistência do motor

Países omitidos nesta tabela seguem o padrão regulamentar de Cooperante (28%), exceto se pertencerem aos sub-regimes específicos estipulados na Portaria n.º 150/2004.

Para efeitos de mais-valias de criptoativos (Anexo J), a classificação da coluna Cooperação Fiscal determina a alíquota de forma absoluta. Jurisdições como Luxemburgo, Malta e Suíça encontram-se integradas nos sistemas de cooperação internacional ou da União Europeia, sendo incorreto atribuir-lhes o estatuto de não cooperante ou a alíquota de 35%.

As convenções de dupla tributação com a Suécia (2022) e a Finlândia (2019) foram denunciadas e o tratado com o Quénia não se encontra vigente. Estes estados permanecem classificados como cooperantes devido às regras gerais de transparência, mas não partilham de regimes de benefícios recíprocos ativos decorrentes de CDT.